[//]: # (title: Setting Up TeamCity for Amazon EC2)
[//]: # (auxiliary-id: Setting Up TeamCity for Amazon EC2)

TeamCity Amazon EC2 integration allows TeamCity auto-scale its building resources by automaically starting and stopping cloud-hosted agents on-demand, depending on the current build queue workload.


## Prerequisites

This section describes steps that you need to perform in your AWS account before setting up a cloud profile in TeamCity UI.

### Create and Setup an EC2 Instance

1. Open the [Amazon EC2 console](https://console.aws.amazon.com/ec2/).
2. Create the required instance. Follow these Amazon tutorials if you experience issues: [Linux](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html), [Windows](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/EC2_GetStarted.html), [macOS](https://aws.amazon.com/getting-started/hands-on/launch-connect-to-amazon-ec2-mac-instance/).
   
   > If you intend to create an Amazon Machine Image (AMI) based on this instance and use this AMI as a template for TeamCity to dynamically spawn and terminate cloud agents, the exact instance type you choose on this step (for instance, `t2.small` or `x1e.xlarge`) is irrelevant. You will be able to specify the instance type later in TeamCity settings.
   > 
   {type="tip"}
   
3. Install an agent on your instance. Depending on the OS type, the required steps may vary. See these articles for more information:

   * [](install-teamcity-agent.md)
   * [](configure-agent-installation.md)

4. Install any additional software that is required to run builds and the TeamCity agent itself: JDKs and JREs, SDKs, runtime frameworks, building tools such as Git and Docker, and so on.
   
   > When using EC2-hosted agents, you occasionally need to update software installed on corresponding instances and re-build AMIs. Otherwise, outdated cloud agents that connect to your TeamCity server will instantly disconnect and spend some time upgrading their tools.
   > 
   > To minimize the number of software that needs regular updates, consider opting for container orchestration solutions instead. This approach allows TeamCity to dynamically create pods that pull the [Agent Docker images](https://hub.docker.com/r/jetbrains/teamcity-agent/tags) with the latest building tools.
   > 
   > TeamCity supports integration with the following solutions:
   > 
   > * [Kubernetes](setting-up-teamcity-for-kubernetes.md) (both self-hosted clusters and hosting services, such as Google Cloud GKE or Amazon EKS)
   > * [Amazon Container Service (ECS)](https://github.com/JetBrains/teamcity-amazon-ecs-plugin)
   > 
   {type="tip"}

5. Run the agent and ensure it successfully connects to you TeamCity server and is compatible with all required configurations.
6. Configure the system to [automatically run a TeamCity agent](start-teamcity-agent.md#Automatic+Start) whenever an instance starts.
7. For Windows instances, see also [Additional configuration for Windows agents](#Additional+Configuration+for+Windows+Agents).
8. Restart your instance to ensure the build agent starts automatically and is able to connect to the server.

### Create an AMI

If you only need one single instance of a machine created in the previous step (for example, if you create a dedicated instance for one specific configuration that rarely runs), you can skip this step. Otherwise, if you want TeamCity to automatically scale the number of active cloud agents based on the current workload, follow the steps below.

1. Remove all temporary and excess data from your instance: installation wizards, downloaded archives, build logs, and so on.
2. Stop the build agent. For Windows instances, stop the agent service but leave its startup type as _Automatic_.
3. Optional: Delete the `<agent_home>/logs` and `<agent_home>/temp` directories.
4. Optional: Delete the `<agent_home>/conf/amazon-*` file.
5. Remove the following properties from the `<agent_home>/conf/buildAgent.properties` file:
   * `name` — TeamCity will automatically assign unique names to your instances.
   * `serverUrl` — for the EC2 integraion plugin, this property is safe to remove since you will provide a server URL for all instances later, when setting up a cloud profile. Other plugins may require this property is present and set to a correct value.
   * (optional) `authorizationToken` — new cloud agents are authorized automatically.
6. Stop the instance.
7. On the overview page of your instance, click **Actions | Image and Templates | Create Image**.
   
   <img src="dk-ec2-createAmi.png" width="706" alt="Create an AMI"/>

## Setup EC2 Integration in TeamCity

After you have created a required instance or AMI, you can set up cloud profiles in TeamCity UI.

### Create a Cloud Profile

A **cloud profile** is a collection of common settings for TeamCity to start virtual machines.

1. Navigate to **Administration | &lt;Required Project&gt; | Cloud Profiles**. If you want cloud agents set up in this profile to be available globally, choose the **&lt;Root project&gt;**. Profiles owned by individual projects can be used to spawn agents that can be used only in these projects.

2. Click **Create new profile**.

3. Set **Cloud type** to "Amazon EC2".

4. Enter your profile name and optional description.

5. Choose between authentication via access key/secret pair, or via credentials stored locally on the server machine. Regardless of the selected mode, an IAM role used by TeamCity should have all permissions listed in this section: [Required IAM Permissions](#Required+IAM+permissions).

   <tabs>
   
   <tab title="Access Keys">

   This option allows you to specify credentials that TeamCity will use to access your AWS resources.
   
   1. Go to the [AWS Identity and Access Management](https://console.aws.amazon.com/iam/) (IAM) dashboard.
   2. Switch to the **Users** tab and find a user whose credentials can be used by TeamCity to access your EC2 instances and AMIs.
   3. Switch to the **Security Credentials** tab and scroll down to the **Access keys** section.
   4. Create a new key. Paste its ID an secret to the related fields in TeamCity UI.
   
   </tab>
   
   <tab title="Local Credentials">

   The **Use default credential provider chain** option allows TeamCity to look for AWS credentials stored on the server machine. Typically, the `config` file with your AWS credentials is located at `~/.aws/config` on Linux or macOS, or at `C:\Users\USERNAME\.aws\config` on Windows.
   
   See the following article for more information about locally stored AWS credentials: [Configuration and credential file settings](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html).

   </tab>

   </tabs>

6. Choose an AWS region in which your instances are hosted.
7. Set up the agents limit. This number specifies the overall limit for agents created from all cloud images of this profile.
8. Specify the [TeamCity server URL](configuring-server-url.md). This value will be automatically passed to agents' `buildAgent.properties` files. If not specified, agents will use the same value as set on the __Administration | Global Settings__ page.
9. Specify the set of criteria for winding down active cloud agents.
10. Click **Apply changes** to save the profile and exit the profile settings page.


### Add a Cloud Image

Cloud profiles specify global settings, such as authorization credentials and instance regions. Each profile has at least one **image** that stores settings related to the specific type of cloud instance that should be started. You can add as many images to a single profile as your needs dictate. However, note that the total number of agents started by all images cannot exceed the limit set in the profile settings (and the number of agents permitted by your license).


1. Click the **Add image** button.
2. Specify the image name. Agents started from this image will use this value as a prefix for their names. All images in a cloud profile must have unique names.

   <anchor name="Amazon+EC2+Spot+Fleet+Support"/>
   <anchor name="Amazon+EC2+Spot+Instances+Support"/>

3. Choose the required image type.

   <tabs>
   
   <tab title="AMI">
   
   With this image type you can configure settings for agents started from the specific AMI.
   
   1. Check **Use launch template** if you want TeamCity to import and use a specific [launch template](https://docs.aws.amazon.com/autoscaling/ec2/userguide/launch-templates.html). When the default/latest version of the template is updated on the server, TeamCity detects these changes and updates the running instances.
   2. Choose a required AMI.
   
      * **Own AMI** — TeamCity scans a collection of AMIs available under credentials specified in the cloud profile settings. You can browse all found AMIs and choose a required image.
      * **AMI by ID** — Allows you to utilize a [shared AMI](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/sharing-amis.html).
      * **AMI by tags** — Specify a comma-separated list of [AWS tags](https://docs.aws.amazon.com/tag-editor/latest/userguide/tagging.html), for example `Owner=Mike,Project=Glacier,Subnet=Public`. If the specified tags point to multiple AMIs, the last used AMI with matching tags will be used. If no AMIs were found, the image name under the **Agents** section will be "Image name (no data)" instead of "Image name (ami-xxxxxxxxx)".
      
         <img src="dk-ec2-invalidTags.png" width="460" alt="Invalid AMI tags"/>
   
   3. Specify one or multiple [instance types](https://aws.amazon.com/ec2/instance-types/).
      
      If you need to invoke new on-demand instances, create multiple identical images with different types. For example, you may have three images that utilize the same Linux AMI: the type of image #1 is `t2.small`, image #2 is `t2.medium`, and image #3 is `t2.large`. This approach allows you to control how many instances of each type are allowed to boot up, and to manually start instances of specific types.
      
      If you intend to order spot instances (see below), you may specify multiple instance types in the same image. This increases your chances to have a spot instance assigned.
      
      > Mac instances support only `mac1.metal` and `mac2.metal` types and are available only as bare metal instances on [Dedicated Hosts](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/dedicated-hosts-overview.html), with a minimum allocation period of 24 hours before you can release the Dedicated Host. Learn more: [Amazon EC2 Mac instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-mac-instances.html).
      > 
      {type="note"}
   
   4. Optional: Specify additional image settings.
      * **IAM Role** — The IAM role that all launched instances will assume. This role specifies the set of permissions that will be [granted to applications](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-ec2.html) running on your EC2 instances. The AWS account used by TeamCity must have `iam:ListInstanceProfiles` and `iam:PassRole` permissions to utilize IAM roles.
      * **Key pair** — Required if you may need to connect to your EC2 instances [using SSH](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstancesLinux.html).
      * **User data** — Allows you to specify a script that will be run when an instance launches. Learn more: [Windows](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ec2-windows-user-data.html), [Linux](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/user-data.html).
      * **Tags** — The list of comma-separated instance tags. For example: `LaunchedBy=TeamCity,TeamCityCloudProfileName=MyProfile1`. Tagging requires the `ec2:*Tags` permission. See the following section for more information: [](#Tagging).
   
   5. Check **Use spot instances** if you prefer to use cheaper [spot instances](https://aws.amazon.com/ec2/spot/) rather than reguar On-Demand instances. The **Max price** field allows you to specify your maximum bid price for spot instances (in US dollars). If the bid price is not specified, the default On-Demand price will be used.

      TeamCity can automatically choose Regions or Availability Zones in which your spot requests are most likely to succeed based on their [spot placement scores](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/spot-placement-score.html#sps-example-configs). To allow TeamCity request and utilize these scores, add the `ec2:GetSpotPlacementScores` [IAM permission](#Required+IAM+permissions).
   
      > It is not recommended to use spot instances for production-critical builds due to the possibility of an [unexpected spot instance termination by Amazon](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/spot-interruptions.html#using-spot-instances-managing-interruptions). If a spot instance is terminated, TeamCity will fail the build with a corresponding build problem, and will re-add this build to the build queue.
      >
      {type="note"}
   6. Specify networking settings for your EC2 instances.
      * **VPC** — The [virtual private cloud](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-vpc.html) to which new instances will belong.
      * **Subnets** — The [IP addresses range](https://docs.aws.amazon.com/vpc/latest/userguide/configure-subnets.html) for your VPS.
      * **Security groups** — Specify [rules for incoming and outgoing connections](https://docs.aws.amazon.com/vpc/latest/userguide/security-groups.html) to (from) your EC2 instance.
   
   </tab>

   
   <tab title="Spot Fleet Request">
   
   Amazon [Spot Fleet](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/spot-fleet.html) allows you to book a combination of regular (On-Demand) and [spot]((https://aws.amazon.com/ec2/spot/)) instances based on the given criteria. See the following AWS documentation article for sample requests: [Spot Fleet example configurations](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/spot-fleet-examples.html).

   1. In [AWS Management Console](https://aws.amazon.com/console/), Go to __Instances | Spot Requests | Request Spot Instances__.
   2. Specify the required AMI, minimum compute unit, availability zones, and other request details.
   3. Click __JSON config__ to download a JSON file with the spot fleet configuration parameters. Note that only fields from the [`SpotFleetRequestConfigData`](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_SpotFleetRequestConfigData.html) class are supported.
   
      <img src="SpotFleetConfigButton.png" width="600" alt="Spot fleet config generation button in Amazon"/>
   
   4. Paste the generated JSON configuration file in the **Spot Fleet Request** field in TeamCity.

      To run the image, TeamCity will launch spot instances matching the allocation strategy specified in the spot fleet configuration.

   > TeamCity uses own values instead of the following parameters of the JSON config file:
   > * `TerminateInstancesWithExpiration`: `false`
   > * `ValidFrom`: the time when the image is created
   > * `ValidUntil`: the `ValidFrom` value plus 5 minutes
   > * `Type`: `request`
   >
   {type="note"}

   </tab>
   
   </tabs>

4. Set up the maximum number of active cloud agents started from this image. Note that the total number of agents started from all images added to a profile cannot exceed the limit set on the settings page of this profile.
5. Specify the [agent pool](configuring-agent-pools.md) to which newly created instances will belong.
6. Click **Save** to exit the image settings page.

After you have configured an image, TeamCity winds up one test agent for this image to test whether it is able to start and connect to the server. When an agent is connected and authorized, TeamCity saves its parameters to be able to correctly assign builds to compatible agents.
   
If your image has the **Use spot instances** setting checked and a test instance cannot be launched at the moment (for instance, if there is no available capacity or your bid price is too low), TeamCity will repeat attempts to launch a spot instance once a minute, as long as there are queued builds which can run on this agent.


## Manage Cloud Agents

When a build is queued, TeamCity attempts to run queued builds on regular (non-cloud) agents first. If none are currently available, TeamCity finds a compatible cloud image and (if the limit of simultaneously running instances is not yet reached) starts a new instance.

You can manually start and stop cloud agents from the **Agents** tab. Note that the **Start** button is disabled if the number of active cloud agents reached the limit specified in profile or image settings.

<img src="dk-ec2-startAndStopAgents.png" width="706" alt="Start and stop cloud agents"/>

The **Running instances** block shows all agents started from this specific image.

* Instances that successfully started but have not yet connected to the TeamCity server have AWS instance IDs as names ("i-xxxxxxxxxxxx").
* Connected and authorized instances have names merged from a parent image name and an AWS instance ID ("Ubuntu-22.04-Large-i-xxxxxxxxxxxx").
* If a connected agent reports outdated build tools, it will automatically disconnect for an upgrade. Once all required software is updated, the agent will reconnect to the server. TeamCity shows a corresponding warning for outdated agent images.
   
   <img src="dk-ec2-outdatedAgent.png" width="706" alt="Outdated agent image"/>

You can open an [interactive terminal](install-and-start-teamcity-agents.md#Debug+Agents+Remotely) to any active agent instance via the **Open Terminal** button. The terminal allows you to debug and maintain your EC2 instances.

## Additional Information and Advanced Concepts

### Required IAM permissions

TeamCity requires the following permissions for Amazon EC2 Resources:

* `ec2:Describe*`
* `ec2:StartInstances`
* `ec2:StopInstances`
* `ec2:TerminateInstances`
* `ec2:RebootInstances`
* `ec2:RunInstances`
* `ec2:ModifyInstanceAttribute`
* [`ec2:*Tags`](#Tagging+Instances+Launched+by+TeamCity)

To use [spot instances](#Amazon+EC2+Spot+Instances+Support), grant the following permissions in addition to those listed above:

* `ec2:RequestSpotInstances`
* `ec2:CancelSpotInstanceRequests`
* `ec2:GetSpotPlacementScores` (optional, allows TeamCity to choose AWS Regions or Availability Zones based on their [spot placement scores](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/spot-placement-score.html#sps-example-configs)).

To use [Spot Fleets](#Amazon+EC2+Spot+Fleet+Support), the following additional permissions are required:
* `ec2:RequestSpotFleet`
* `ec2:DescribeSpotFleetRequests`
* `ec2:CancelSpotFleetRequests`
* `iam:CreateServiceLinkedRole` (if you are getting _"The provided credentials do not have permission to create the service-linked role for EC2 Spot Fleet"_ error; you can safely revoke this permission once the service role is created)

To launch an instance with an IAM Role (applicable to instances cloned from AMIs and launch templates), the following additional permissions are required:
* `iam:ListInstanceProfiles`
* `iam:PassRole`

To use [encrypted EBS volumes](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSEncryption.html), the following additional permissions are required:
* `kms:CreateGrant`
* `kms:Decrypt`
* `kms:DescribeKey`
* `kms:GenerateDataKeyWithoutPlainText`
* `kms:ReEncrypt`

<!--To [connect to an agent via SSM](#debugging-and-maintenance), the following additional permissions are required:
* `sts:GetFederationToken`
* `ssm:*Session`-->

The snippet below illustrates a custom IAM policy definition that allows all EC2 operations from a specified IP address:

```Shell
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "1",
            "Effect": "Allow",
            "Action": "ec2:*",
            "Condition": {
                "IpAddress": {
                    "aws:SourceIp": "<TeamCity server IP address>"
                }
            },
            "Resource": "*"
        }
    ]
}
```

See also: [Example policies (Linux)](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ExamplePolicies_EC2.html), [Example policies (Windows)](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ExamplePolicies_EC2.html).


### Additional Configuration for Windows Agents

_To ensure proper TeamCity agent communication with EC2 API (including access to additional drives) on Windows_, add a dependency from the TeamCity Build Agent service on the [AmazonSSMAgent](https://docs.aws.amazon.com/systems-manager/latest/userguide/ssm-agent.html) or [EC2Launch/EC2Config](http://docs.amazonwebservices.com/AWSEC2/latest/WindowsGuide/UsingConfig_WinAMI.html) service (the service which ensures the machine is fully initialized in regard to AWS infrastructure use). This can be done, for example, via the [Registry](https://support.microsoft.com/en-us/windows/how-to-open-registry-editor-in-windows-10-deab38e6-91d6-e0aa-4b7c-8878d9e07b11) or using [sc config](https://technet.microsoft.com/en-us/library/cc990290(v=ws.11).aspx) (for instance, `sc config TCBuildAgent depend=EC2Config`).   
Alternatively, you can use the "Automatic (delayed start)" service starting mode.

> Due to the [bug in the network settings](https://forums.aws.amazon.com/thread.jspa?threadID=242194), instance metadata is not available by default on instances based on Windows Server 2016 image. It means the TeamCity agent service cannot retrieve its properties and cloud integration doesn't work (the agent does not connect to the server or is not automatically authorized). To fix this issue, do the following:
> 
> 1. Install the [latest EC2Config](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/UsingConfig_Install.html?shortFooter=true#ec2config-update-version).
> 2. Set the dependency of the `TCBuildAgent` service on both `EC2Config` and `AmazonSSMAgent` via the command: `sc config TCBuildAgent depend=Ec2Config/AmazonSSMAgent`.
>
{type="warning"}




### Amazon EBS-Optimized Instances

The behavior of [EBS optimization](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSOptimized.html) in TeamCity is similar to that offered by EC2 console. When configuring an image of the Amazon [cloud profile](agent-cloud-profile.md), the optimization can be set using the corresponding box of the Instance Type. Note that
* EBS optimization is turned on by default for `c4.*`, `d2.*`, and `m4.*` (non-configurable)
* EBS optimization is turned off by default for any other instance types and  can be turned on for instances that support it (such as `c3.xlarge`, and so on)


### Tagging

#### Tagging Instances Launched by TeamCity

The following requirements must be met for tagging instances launched by TeamCity:
* you have the `ec2:*Tags` permissions
* the [maximum number of tags (50)](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Tags.html#tag-restrictions) for your Amazon EC2 resource is not reached

In the absence of tagging permissions, TeamCity will still launch Amazon AMI and EBS images with no tags applied; Amazon EC2 spot instances will not be launched.

TeamCity enables users to get instance launch information by marking the created instances with the `teamcity:TeamcityData` tag containing `<server UUID>:-<cloud profile ID>:-<image reference>`. __This tag is necessary for TeamCity integration with EC2 and must not be deleted.__

Custom tags can be applied to EC2 cloud agent instances: when configuring Cloud profile settings, in the __Add Image/Edit Image__ dialog use the __Instance tags__: field to specify tags in the format of `<key1>=<value1>,<key2>=<value2>`. [Amazon tag restrictions](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Tags.html#tag-restrictions) need to be considered.

When using the equal(=) sign in the tag value, no escaping is needed. For instance, the string `extraParam=name=John` will be parsed into `<key=extraParam>` and value `<name=John>.`

#### Tagging Instance-Dependent Resources

When launching Amazon EC2 instances, TeamCity tags all the resources (for example, volumes and network adapters) associated with the created instances, which is important when evaluating the overall cost of an instance (taking into account the storage drive type and size, I/O operations (for standard drives), network (transfers out), and so on.

#### Sharing a EBS Instance Between Multiple TeamCity Servers

As mentioned [above](#Tagging+Instances+Launched+by+TeamCity), TeamCity tags every instance it launches with the `teamcity:TeamcityData` tag that represents a server, cloud profile, and source (AMI or EBS\-instance). So, when several TeamCity servers try to use the same EBS instance, the second one will see the following message "Instance is used by another TeamCity server. Unable to start/stop it". If you are sure that no other TeamCity servers are working with this instance, you can delete the `teamcity:TeamcityData` tag and the instance will become available for all TeamCity servers again.

### Proxy settings
{product="tc"}

If your TeamCity server needs to use a proxy to connect to AWS API endpoint, configure the following server [internal properties](server-startup-properties.md#TeamCity+Internal+Properties) to connect to Amazon AWS addresses.

* `teamcity.http.proxy.host.ec2` — proxy server host name
* `teamcity.http.proxy.port.ec2` — proxy server port

For proxy server authentication: 

* `teamcity.http.proxy.user.ec2` — proxy access username 
* `teamcity.http.proxy.password.ec2` — proxy access user password

For NTML authentication: 
* `teamcity.http.proxy.domain.ec2` — proxy user domain for NTLM authentication 
* `teamcity.http.proxy.workstation.ec2` — proxy access workstation for NTLM authentication

### Estimating EC2 Costs

Usual Amazon EC2 pricing applies. Note that Amazon charges can depend on the specific configuration implemented to deploy TeamCity. We advise you to check your configuration and Amazon account data regularly in order to discover and prevent unexpected charges as early as possible.

Note that traffic volumes and necessary server and agent machines characteristics depend a big deal on the TeamCity setup and nature of the builds run. See also [Estimate Hardware Requirements for TeamCity](system-requirements.md#Estimating+External+Database+Capacity).
{product="tc"}

#### Estimating Traffic

Here are some points to help you estimate TeamCity-related traffic:

* If TeamCity server is not located within the same EC2 region or availability zone that is configured in TeamCity EC2 settings for agents, traffic between the server and agent is subject to usual Amazon EC2 external traffic charges.
* When estimating traffic, bear in mind that there are lots types of traffic related to TeamCity (see a non-complete list below).

__External connections originated by server__:

* VCS servers
* Email servers
* Maven repositories

__Internal connections originated by server__:

* TeamCity agents (checking status, sending commands, retrieving information like thread dumps, and so on)

__External connections originated by agent__:
* VCS servers (in case of agent-side checkout)
* Maven repositories
* Any connections performed from the build process itself

__Internal connections originated by agent__:
* TeamCity server (retrieving build sources in case of server-side checkout or personal builds, downloading artifacts, and so on)

__Usual connections served by the server__:
* Web browsers
* IDE plugins

#### Uptime Costs

As Amazon rounds machine uptime to the full hour for some configurations (more at [How are Amazon EC2 instance hours billed?](https://aws.amazon.com/premiumsupport/knowledge-center/ec2-instance-hour-billing/)), adjust timeout setting on the EC2 image setting on TeamCity cloud integration settings according to your usual builds length.

It is also highly recommended to set execution timeout for all your builds so that a build hanging does not cause prolonged instance running with no payload.
