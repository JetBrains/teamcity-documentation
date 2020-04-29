[//]: # (title: Setting Up TeamCity for Amazon EC2)
[//]: # (auxiliary-id: Setting Up TeamCity for Amazon EC2)

TeamCity Amazon EC2 integration allows you to configure TeamCity with your Amazon account and then start and stop images with TeamCity agents on-demand based on the queued builds.

Integrations with other cloud solutions are [available](teamcity-integration-with-cloud-solutions.md#Available+Integrations).

## General Description

It is assumed that the machine images are preconfigured to start TeamCity agent on boot (see details [below](#Preparing+Image+with+Installed+TeamCity+Agent)).

Once a cloud profile is configured in TeamCity with one or several images, TeamCity does a test start for all the new images to learn about the agents on them.   
Once the agents are connected, TeamCity stores their parameters to be able to process the build configurations-to-agents compatibility correctly.

For each queued build, TeamCity first tries to start it on one of the regular, non-cloud agents. If there are no usual agents available, TeamCity finds a matching cloud image with a compatible agent and starts a new instance for the image. TeamCity ensures that the running instances limit configured in the cloud profile is not exceeded.

Once an agent is connected from a cloud instance started by TeamCity, it is automatically authorized (provided there are available agent licenses). After that, the agent is processed as a regular agent.   
If running timeout is configured on the cloud profile and it is reached, the instance is terminated (or stopped, if an existing Amazon instance has been selected as an image source).

On the instance terminating/stopping, its disconnected agent is removed from the authorized agents list and is deleted from the system.

TeamCity supports Amazon EC2 [Spot Instances](#Amazon+EC2+Spot+Instances+support) and [Spot Fleets](#Amazon+EC2+Spot+Fleet+support).

## Configuration

Understanding Amazon EC2 and ability to perform EC2 tasks is a prerequisite for configuring and using TeamCity Amazon EC2 integration.

Basic TeamCity EC2 setup involves:
* preparing an Amazon EC2 image (AMI) with an installed TeamCity agent
* configuring EC2 integration on TeamCity server

<note>

Note that the number of EC2 agents is limited by the total number of agent licenses you have in TeamCity.
</note>

Make sure the server URL specified on the __Administration | Global Settings__ page is correct since agents will use it to connect to the server, if a custom server URL is not specified in the [cloud profile settings](agent-cloud-profile.md#Specifying+profile+settings).

If you need TeamCity to use a proxy to access EC2 services, please read on a current workaround in the dedicated [issue](http://youtrack.jetbrains.com/issue/TW-23637#comment=27-380892).

### Required IAM permissions

TeamCity requires the following permissions for Amazon EC2 Resources:
* `ec2:Describe*`
* `ec2:StartInstances`
* `ec2:StopInstances`
* `ec2:TerminateInstances`
* `ec2:RebootInstances`
* `ec2:RunInstances`
* `ec2:ModifyInstanceAttribute`
* [`ec2:*Tags`](#Tagging+for+TeamCity-launched+instances)

To use [spot instances](#Amazon+EC2+Spot+Instances+support), the following additional permissions are required:
* `ec2:RequestSpotInstances`
* `ec2:CancelSpotInstanceRequests`

To use [spot fleets](#Amazon+EC2+Spot+Fleet+support), the following additional permissions are required:
* `ec2:ec2:RequestSpotFleet`
* `ec2:DescribeSpotFleetRequests`
* `ec2:CancelSpotFleetRequests`

To launch an [instance with the IAM Role](#Configuring+an+Amazon+EC2+cloud+profile) (applicable to instances cloned from AMI-s only), the following additional permissions are required:
* `iam:ListInstanceProfiles`
* `iam:PassRole`

An example of custom IAM policy definition (allows all EC2 operations from a specified IP address):

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

#### Optional permissions

See the [section below](#Configuring+an+Amazon+EC2+cloud+profile) for permissions to set IAM roles on an agent instance.

View information on example policies for [Linux](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ExamplePolicies_EC2.html) and [Windows](http://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ExamplePolicies_EC2.html) on the Amazon website.

### Preparing Image with Installed TeamCity Agent

Here are the requirements for an image that can be used for TeamCity cloud integration:
* TeamCity agent should be correctly [installed](setting-up-and-running-additional-build-agents.md).
* TeamCity agent should start on machine startup.
* `buildAgent.properties` can be left as is. The `serverUrl`, `name`, and `authorizationToken` properties can be empty or set to any value; they are ignored when TeamCity starts the instance.

Provided these requirements are met, usual TeamCity agent installation and cloud-provider image bundling procedures are applicable.

If you need the [connection](setting-up-and-running-additional-build-agents.md#Agent-Server+Data+Transfers) between the server and the agent machine to be secure, you will need to set up the agent machine to establish a secure tunnel (for example, VPN) to the server on boot so that TeamCity agent receives data via the secure channel.

Recommended image (for example, Amazon AMI) preparation steps:

1. Choose one of the existing generic images.
2. Start the image.
3. Configure the running instance:   
   * Install and configure a build agent:  
     * Configure server name and agent name in `conf/buildAgent.properties` – this is optional if the image will be started by TeamCity, but it is useful to test if the agent is configured correctly.
     * It usually makes sense to specify `tempDir` and `workDir` in `conf/buildAgent.properties` to use the non-system drive (`d:` under Windows).
   * Install any additional software necessary for the builds on the machine.
   * Run the agent and check it is working OK and is compatible with all necessary build configurations, and so on.
   * Configure the system so that the agent is started on machine boot (and make sure TeamCity server is accessible on machine boot).
   * For Windows, see also [Additional configuration for Windows agents](#Additional+configuration+for+Windows+agents)   
4. Test the setup by rebooting the machine and checking that the agent connects normally to the server.
5. Prepare the Image for bundling:    
   * Remove any temporary/history information in the system.
   * Stop the agent (under Windows stop the service but leave it in _Automatic_ startup type)
   * Delete the content `logs` and `temp` directories in agent home (optional)
   * Delete the `<Agent Home>/conf/amazon-*` file (optional)
   * Change `config/buildAgent.properties` to remove properties: `name`, `serverUrl`, `authorizationToken` (optional). `serverUrl` can be removed for EC2 integration plugins. Other plugins might require that it is present and set to correct value.    
6. Stop the running instance and make a new image from it (or just stop it if an existing Amazon instance has been selected as an image source).

#### Additional configuration for Windows agents

_To ensure proper TeamCity agent communication with EC2 API (including access to additional drives) on Windows_, add a dependency from the TeamCity Build Agent service on the [AmazonSSMAgent](https://docs.aws.amazon.com/systems-manager/latest/userguide/ssm-agent.html) or [EC2Launch/EC2Config](http://docs.amazonwebservices.com/AWSEC2/latest/WindowsGuide/UsingConfig_WinAMI.html) service (the service which ensures the machine is fully initialized in regard to AWS infrastructure use). This can be done, for example, via the [Registry](https://support.microsoft.com/en-us/kb/193888) or using [sc config](https://technet.microsoft.com/en-us/library/cc990290(v=ws.11).aspx) (for instance, `sc config TCBuildAgent depend=EC2Config`).   
Alternatively, you can use the "Automatic (delayed start)" service starting mode.

##### Important note for images based on Windows Server 2016 image

Due to the [bug in the network settings](https://forums.aws.amazon.com/thread.jspa?threadID=242194), instance metadata is not available by default. It means the TeamCity agent service cannot retrieve its properties and cloud integration doesn't work (the agent does not connect to the server or is not automatically authorized). To fix this issue, do the following:

1. Install the [latest EC2Config](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/UsingConfig_Install.html?shortFooter=true#ec2config-update-version).
2. Set the dependency of the `TCBuildAgent` service on both `EC2Config` and `AmazonSSMAgent` via the command: `sc config TCBuildAgent depend=Ec2Config/AmazonSSMAgent`.

### Agent auto-upgrade Note

TeamCity agent auto-upgrades whenever distribution of agent plugins on the server changes (for example, after TeamCity upgrade). If you want to cut agent startup time, you might want to rebundle the agent AMI after agent plugins have been auto-updated.

### Configuring an Amazon EC2 cloud profile

You can configure an Amazon EC2 [agent cloud profile](agent-cloud-profile.md) in the __Cloud Profiles__ section of the __Project Settings__.

#### Custom Image Name

The value of the _Custom Image Name_ parameter of the image serves as a unique identifier for all TeamCity agents using this image. For example, this value is added as a prefix to agents IDs in the cloud profiles log on the __Agents | Cloud__ tab.

Each image in a cloud profile must have a unique custom name.

#### IAM profiles

It is possible to use IAM (Identity and Access Management) profiles with build agents launched as Amazon EC2 instances, which requires the supplied AWS account to have the following permissions:
 
* `iam:ListInstanceProfiles`
* `iam:PassRole`

IAM profiles must be [preconfigured](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-ec2_instance-profiles.html) in Amazon EC2. In the TeamCity web UI, the __IAM profile__ drop-down menu enables you to select a role. Every new launched EC2 instance will assume the [selected IAM role](http://docs.aws.amazon.com/IAM/latest/UserGuide/roles-usingrole-ec2instance.html).

#### Amazon EC2 Spot Instances support

TeamCity supports [Amazon EC2 Spot Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-spot-instances.html) which allows you to place your bid on unused EC2 capacity and use it, as long as your suggested price exceeds the current _Spot price_.

You can enable Spot Instances for an Amazon EC2 image in your [cloud profile settings](agent-cloud-profile.md). Click __Add image__ and select a _Source_, or edit an existing image. Select the __Spot instances__ option, specify a __Bid price__, and save the image.   
If the bid price is not specified, the default On-Demand price will be used.

TeamCity launches a spot instance as soon as the cloud profile is saved. If it is impossible to launch the instance (for example, if there is no available capacity or if your bid price is lower than the current minimum Spot Price in Amazon), TeamCity will be repeating the launch attempt once a minute, as long as there are queued builds which can run on this agent.

<note>

It is not recommended to use spot instances for production\-critical builds due to the possibility of an [unexpected spot instance termination by Amazon](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/spot-interruptions.html#using-spot-instances-managing-interruptions).

</note>

#### Amazon EC2 Spot Fleet support

A _Spot Fleet_ is a set of [Spot Instances](#Amazon+EC2+Spot+Instances+support) launched based on the predefined criteria. You can request a Spot Fleet instead of regular Spot Instances, and TeamCity will be able to select and launch Spot Instances with the lowest price in the availability zones you specify.

To use a Spot Fleet in your project:

1. In [AWS Management Console](https://aws.amazon.com/console/), generate a Spot Fleet request:
   * Go to __Instances | Spot Requests | Request Spot Instances__.    
   * Specify the required AMI, minimum compute unit, availability zones, and other request details.
   * Click __JSON config__ to download a JSON file with the Spot Fleet configuration parameters.    

   <img src="SpotFleetConfigButton.png" width="600" alt="Spot fleet config generation button in Amazon"/>
   
2. In TeamCity, add a Spot Fleet to a cloud profile:
   * On the __Cloud Profile__ page click __Add image__.
   * In the _Source_ drop-down menu, select _Spot Instance Fleet_. Enter the maximum number of required Spot Instances (_Max instances_) in a Fleet and select an _[agent pool](agent-pools.md)_.    
   * In the _Spot Fleet Config_ field, insert the JSON configuration file generated in AWS Management Console.    

   <img src="SpotFleetConfig.png" width="550" alt="Spot Fleet configuration in TeamCity"/>
   
   * Save the image.
   
To run the image, TeamCity will launch spot instances matching the allocation strategy specified in the Spot Fleet configuration.

<note>

TeamCity uses own values instead of the following parameters of the JSON config file:
* `TargetCapacity`: TeamCity sets it to `1` and runs instances in accordance with the _Max instances_ number instead
* `TerminateInstancesWithExpiration`: `false`
* `ValidFrom`: the time when the image is created
* `ValidUntil`: the `ValidFrom` value plus 5 minutes
* `Type`: `request`

</note>

#### Amazon EC2 Launch Templates support

TeamCity supports [Amazon EC2 launch templates](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-launch-templates.html) for cloud instances. Launch templates allow reusing a once defined launch specification for all new instances, which eliminates the need to describe the launch settings every time new instances are requested.

If your cloud profile is connected to the Amazon server, TeamCity will automatically detect launch templates available on this server. When adding an image, select a required template as the _Source_ and specify its version, and TeamCity will request instances based on the template parameters. Optionally, you can also limit the number of launched instances and assign them to a certain [agent pool](agent-pools.md).

When the default/latest version of the template is updated on the server, TeamCity will detect these changes and update the running instances.

#### Amazon EBS-Optimized Instances

The behavior of [EBS-optimization](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSOptimized.html) in TeamCity is similar to that offered by EC2 console. When configuring an image of the Amazon [cloud profile](agent-cloud-profile.md), the optimization can be set using the corresponding box of the Instance Type. Note that
* EBS\-optimization is turned on by default for `c4.*`, `d2.*`, and `m4.*` (non-configurable)
* EBS\-optimization is turned off by default for any other instance types and  can be turned on for instances that support it (such as `c3.xlarge`, and so on)

#### Tagging for TeamCity-launched instances

##### Requirements

The following requirements must be met for tagging instances launched by TeamCity:
* you have the `ec2:*Tags` permissions
* the [maximum number of tags (50)](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Tags.html#tag-restrictions) for your Amazon EC2 resource is not reached

In the absence of tagging permissions, TeamCity will still launch Amazon AMI and EBS images with no tags applied; Amazon EC2 Spot Instances will not be launched.

##### Automatic tags

TeamCity enables users to get instance launch information by marking the created instances with the `teamcity:TeamcityData` tag containing `<server UUID>:-<cloud profile ID>:-<image reference>.`

##### Custom tags

Custom tags can be applied to EC2 cloud agent instances: when configuring Cloud profile settings, in the __Add Image/Edit Image__ dialog use the __Instance tags__: field to specify tags in the format of `<key1>=<value1>,<key2>=<value2>`.  [Amazon tag restrictions](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Tags.html#tag-restrictions) need to be considered.

When using the equal(=) sign in the tag value, no escaping is needed. For instance, the string `extraParam=name=John` will be parsed into `<key=extraParam>` and value `<name=John>.`

#### Tagging instance-dependent resources

When launching Amazon EC2 instances, TeamCity tags all the resources (for example, volumes and network adapters) associated with the created instances, which is important when evaluating the overall cost of an instance (taking into account the storage drive type and size, I/O operations (for standard drives), network (transfers out), and so on.

#### Sharing single EBS instance between several TeamCity servers

As mentioned [above](#Automatic+tags), TeamCity tags every instance it launches with the `teamcity:TeamcityData` tag that represents a server, cloud profile, and source (AMI or EBS\-instance). So, when several TeamCity servers try to use the same EBS instance, the second one will see the following message "Instance is used by another TeamCity server. Unable to start/stop it". If you are sure that no other TeamCity servers are working with this instance, you can delete the `teamcity:TeamcityData` tag and the instance will become available for all TeamCity servers again.

### New instance types

Since Amazon doesn't provide a robust API method to retrieve all instance types, Amazon integration relies on the periodical update of AWS SDK to make new instance types available.

However, there is a workaround if you are not willing to wait. To register new Instance Types, use the `teamcity.ec2.instance.types` [internal property](configuring-teamcity-server-startup-properties.md#TeamCity+internal+properties) with new instance types separated by ","

### Proxy settings

If your TeamCity server needs to use a proxy to connect to AWS API endpoint, configure the following server [internal properties](configuring-teamcity-server-startup-properties.md#TeamCity+internal+properties) to connect to Amazon AWS addresses.

* `teamcity.http.proxy.host.ec2` – proxy server host name
* `teamcity.http.proxy.port.ec2` – proxy server port

For proxy server authentication: 

* `teamcity.http.proxy.user.ec2` – proxy access username 
* `teamcity.http.proxy.password.ec2` – proxy access user password

For NTML authentication: 
* `teamcity.http.proxy.domain.ec2` – proxy user domain for NTLM authentication 
* `teamcity.http.proxy.workstation.ec2` – proxy access workstation for NTLM authentication

### Custom script

It is possible to run a custom script on the instance start (applicable to instances cloned from AMI's only).   
The Amazon website details the script format for [Linux](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/user-data.html) and [Windows](http://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ec2-instance-metadata.html#user-data-execution).

### Estimating EC2 Costs

Usual Amazon EC2 pricing applies. Note that Amazon charges can depend on the specific configuration implemented to deploy TeamCity. We advise you to check your configuration and Amazon account data regularly in order to discover and prevent unexpected charges as early as possible.

Note that traffic volumes and necessary server and agent machines characteristics depend a big deal on the TeamCity setup and nature of the builds run. See also [Estimate Hardware Requirements for TeamCity](how-to.md#Estimate+Hardware+Requirements+for+TeamCity).

### Traffic Estimate

Here are some points to help you estimate TeamCity-related traffic:

* If TeamCity server is not located within the same EC2 region or availability zone that is configured in TeamCity EC2 settings for agents, traffic between the server and agent is subject to usual Amazon EC2 external traffic charges.
* When estimating traffic, bear in mind that there are lots types of traffic related to TeamCity (see a non\-complete list below).

__External connections originated by server:__

* VCS servers
* Email and Jabber servers
* Maven repositories

__Internal connections originated by server:__

* TeamCity agents (checking status, sending commands, retrieving information like thread dumps, and so on)

__External connections originated by agent:__
* VCS servers (in case of agent\-side checkout)
* Maven repositories
* any connections performed from the build process itself

__Internal connections originated by agent:__
* TeamCity server (retrieving build sources in case of server\-side checkout or personal builds, downloading artifacts, and so on)

__Usual connections served by the server:__
* web browsers
* IDE plugins

#### Uptime Costs

As Amazon rounds machine uptime to the nearest full hour, adjust timeout setting on the EC2 image setting on TeamCity cloud integration settings according to your usual builds length.

It is also highly recommended to set execution timeout for all your builds so that a build hanging does not cause prolonged instance running with no payload.

__ __
