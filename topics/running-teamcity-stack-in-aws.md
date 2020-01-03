[//]: # (title: Running TeamCity Stack in AWS)
[//]: # (auxiliary-id: Running TeamCity Stack in AWS)

You can run the TeamCity stack in AWS using the [CloudFormation template](https://github.com/JetBrains/teamcity-cloudformation-template). Note that this is an experimental option, which might not yet be fully ready for the production use. You can still use it as a base for your deployment approach.

On this page:

<tag-list of="chapter" mode="tree" depth="4"/>

## Stack Overview

The current setup uses 2 subnets, a public and a private one.
* The private subnet includes all the essential items:
  * ECS cluster of a CoreOS EC2 instance with the official TeamCity server of the specified version from Docker Hub and one TeamCity Build Agent. The official Docker images with the TeamCity server and build agent are used.
  * RDS MySQL database
* The public subnet includes:
  * Application Load Balancer
  * NAT gateway ensuring the publicly available IPs

Both subnets are placed into a Virtual Private Cloud (VPC) which is completely secure. The database allows only internal connections within the VPC and its possible to connect to the Server via HTTP(s) or SSH only.

## Prerequisites

To create a TeamCity stack and connect to it, you will need:
* [EC2 KeyPair](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html) in the same region as the TeamCity stack
* Installed SSH client to connect to the TeamCity server and view the logs
* IAM permissions to create the service\-linked role and apply a policy to it for the IAM entity creating the stack

## Using Template

1\.  On the _Select Template_ page, select the default TeamCity Template and click __Next__.

2\.  Specify the stack name and parameters provided by the template:

### Template Parameters

__  __

<table><tr>

<td>

Setting


</td>

<td>

Description


</td></tr><tr>

<td>

Name


</td>

<td>

The name for your TeamCity server, set to _test_ by default.


</td></tr><tr>

<td>

TeamCity Version


</td>

<td>

By default, the template will create a TeamCity installation of the latest version. You can also specify the exact version number here, for example, 2017.1.5, 2017.2.


</td></tr><tr>

<td>

Internet\-Facing Stack

 


</td>

<td>

By default, set to `false`.  This setting affects the Application Load Balancer: by default it has no public IP address and only receives traffic inside VPC. To create a publicly available TeamCity instance, set it to `true`.

</td></tr><tr>

<td>

EC2 KeyPair (required)


</td>

<td>

Specify an existing EC2 KeyPair for SSH access to the TeamCity Server EC2 instance. If you fail to provide the key pair, the stack creation will fail with the following error: "Template validation error: Parameter 'KeyName' must match pattern .\+"


</td></tr><tr>

<td>

SSL Certificate Domain

</td>

<td>

Optional. If you are a domain owner, you can specify the domain here and get the certificate that will be automatically registered in your load balancer. Your stack creation will be paused until you validate your email.

</td></tr><tr>

<td>

EC2 instance Type


</td>

<td>

Specify the [type of instance](https://aws.amazon.com/ec2/instance-types/) for the TeamCity server


</td></tr><tr>

<td>
Container CPU

</td>

<td>
Container CPU in virtual CPU units

</td></tr><tr>

<td>
Container Memory

</td>

<td>

Set to 3700 MiB by default.

</td></tr><tr>

<td>

RDS Database Instance Type


</td>

<td>

Specify the type for the RDS MySQL instance used as the external database for TeamCity. Default: `db.t2.medium`.


</td></tr><tr>

<td>

TeamCity Database Password (required)


</td>

<td>

Specify any password for the TeamCity database


</td></tr></table>



### Build Agents


<table><tr>

<td>

Setting


</td>

<td>

Description


</td></tr><tr>

<td>

Agents number

</td>

<td>

Specify how many agents you want to start. Every agent will be launched on a separate machine. If 0 is specified, no agent will start.

</td></tr><tr>

<td>

EC2 instance Type


</td>

<td>

Specify the [type of instance](https://aws.amazon.com/ec2/instance-types/) for the TeamCity agents


</td></tr><tr>

<td>

Container CPU

</td>

<td>

Container CPU in virtual CPU units

</td></tr><tr>

<td>

Container Memory

</td>

<td>

Set to 2048 MiB by default.

</td></tr></table>



3\. Click __Next__. (Optional) In the dialog that appears, provide additional options if required.

4\. Click __Next__, review  your settings, accept the creation of AWS roles.

<note>

In most cases Amazon ECS creates the service\-linked role for you, if it does not already exist. If a user account creating the stack misses the appropriate permissions, the stack creating will fail with the following error: "Unable to assume the service linked role. Verify that the ECS service linked role exists".    
In this case you can create a service linked role with the following command:    

```Shell

 aws iam create-service-linked-role --aws-service-name ecs.amazonaws.com
```


</note>

5\. Click __Сreate__. No other actions are required. It takes about 15 minutes for the template to deploy the whole stack. Once the deployment is ready, you will see the TeamCity server endpoint in the _Output_ section which points you to your TeamCity installation. 

6\. Access the TeamCity instance from your browser, create the administrators account and start using your TeamCity.

## Connecting to server and viewing logs

To connect to the servers console, you need to use your instance private key:


```Shell
ssh -i <path to private key\privatekey.pem> ec2-user@<server_IP_address>

```


To see  `teamcity-agent.log` or `teamcity-server.log`, just run the `docker logs` command for the desired container. For example, for the server logs, run: 


```Shell

docker logs teamcity-server

```


## Next Steps

Once you have TeamCity up and running, consider the following steps:
* Use the [Setting Up TeamCity for Amazon EC2](setting-up-teamcity-for-amazon-ec2.md) to run and connect more build agents to your server
* Configure TeamCity to use the S3 bucket as [Configuring Artifacts Storage](configuring-artifacts-storage.md#Amazon+S3+Support).

## Upgrading TeamCity in AWS

To update TeamCity started from the CloudFormation template:
1. In the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation), from the list of stacks, select the running TeamCity stack and use the [Update Stack](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-updating-stacks-direct.html) option.
2. You will be redirected to the _Select Template_ page: use the _Current Template_ option and click __Next__.
3. On the template settings page, enter the TeamCity version you want to update to. Note that if you previously used the TeamCity version tagged latest, you will now need to provide the actual version number as the `latest` tag can be applied to the server only once. 
4. Click __Next__, provide additional options if required, review the new settings and click __Update__. Once the Update is complete, access the TeamCity web UI from the browser.
5. If required, provide the [Super User token](super-user.md): to obtain it, you need to connect to your server instance, get the TeamCity server log as described [above](#Connecting+to+server+and+viewing+logs), and retrieve the maintenance token.
6. Wait for the server to upgrade, log in to the TeamCity server and wait for the agent to upgrade and connect to the server.


__  __

__See also:__

__TeamCity blog__: [The official TeamCity CloudFormation template](https://blog.jetbrains.com/teamcity/2017/10/teamcity-aws/)

__ __
