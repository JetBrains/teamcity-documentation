[//]: # (title: Deploying TeamCity to Azure)
[//]: # (auxiliary-id: Deploying TeamCity to Azure)

This page describes how to launch TeamCity on [Microsoft Azure](https://azure.microsoft.com/en-us/) using the official [Azure Resource Manager template](https://github.com/JetBrains/teamcity-azure-template).

The template covers the following installation stages:
* Setting up an external database
* Creating and configuring a virtual machine for the TeamCity server
* Installing the TeamCity server
* Installing a TeamCity build agent

On this page:

<tag-list of="chapter" mode="tree" depth="5"/>

## Implementation Details

* To install TeamCity on Azure, the deployment service allocates a Linux CoreOS virtual machine with the Docker support.
* The virtual machine exposes port 80 for the web access and port 22 for maintenance in the network security group thus providing an endpoint for SSH connection.
* The TeamCity server and build agents are started from their official Docker images.
* Configuration and data files are stored on the disk attached to the virtual machine.
* An instance of the Azure database for MySQL is created with the _teamcitydb_ schema. Network security rules are set to make the database accessible from the TeamCity server machine.
* In the virtual machine, the TeamCity operation is controlled by the following `systemd` services:
   * `teamcity-server.service` controls the lifecycle of the TeamCity server
   * `teamcity-agent.service` controls the lifecycle of TeamCity build agents
   * `teamcity-update.service` controls the TeamCity updates

## Deployment Prerequisites

Before installing, make sure you have a Microsoft Azure [account](https://azure.microsoft.com/en-us/free/).

To choose a proper _TeamCity installation size_ during the deployment, consider the following evaluations:
* _Small_: typically 3 users in a team, 100 builds/day
* _Medium_: typically 5 users in a team, 300 builds/day
* _Large_: typically 20 users in a team, 1000 builds/day

## Deploying Teamcity via Azure Template

To install TeamCity to Microsoft Azure:
1. Go to the [TeamCity section](https://www.jetbrains.com/teamcity/download/#section=azure) on the JetBrains website and click __Deploy to Azure__.   
You will be redirected to the Microsoft Azure authentication screen.
2. Select an active Azure account or enter your Azure credentials to sign in.
3. On successful authentication, you will be redirected to the __Home | TeamCity__ page in Azure Portal.
4. Click __Create__.   
   <img src="azure-create-teamcity.png" width="593" alt="Create TeamCity in Microsoft Azure"/>
5. Enter all the necessary information to install TeamCity in 5 steps:
   1. __Basics__: TeamCity version, Azure resource group, and so on.   
      <img src="azure-teamcity-inst-1.png" width="626" alt="Create TeamCity in Microsoft Azure, Step 1"/>
   2. __Virtual machine settings__: public IP address, virtual network, and so on.   
   We highly recommend selecting _SSH public key_ as the VM authentication type. Follow [this instruction](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/create-ssh-keys-detailed) to generate an SSH key.
   3. __Additional settings__: installation size, MySQL password, and storage account.   
   Refer to [Prerequisites](#Prerequisites) for recommendations on installation size.
   4. __Summary__: wait for validation of resources and review the summary of deployment parameters.
   5. __Finalization__: review the TeamCity Terms of Use and click __Create__.
   
TeamCity deployment will start. You can monitor its status in the __Notifications__ block.
<img src="azure-deployment-status.png" width="515" alt="TeamCity deployment status"/>

After the deployment is finished, go to __Home | Virtual machines__ and open the __Overview__ of the created virtual machine to see the IP address of the installed TeamCity server.
<img src="azure-vm-overview.png" width="1393" alt="TeamCity virtual machine overview"/>
   
Open the TeamCity web UI on the provided IP address and proceed with the standard procedure to complete the installation: accept the license agreement and create the root user account.


<tip>

To access the virtual machine via command line, use the private SSH key paired with the public key used in Step 5.2.
</tip>

## TeamCity Plugins for Integration with Azure

The Azure version of TeamCity server comes with preinstalled plugins for a tighter integration with Azure services:
* [Azure Resource Manager Cloud Support](https://plugins.jetbrains.com/plugin/9260-azure-resource-manager-cloud-support) allows scaling the pool of TeamCity agents by creating cloud build agents in Microsoft Azure cloud via [Resource Manager](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) deployments.
* [Azure Artifacts Storage](https://plugins.jetbrains.com/plugin/9617-azure-artifact-storage) allows storing build artifacts in [Azure Blob Storage](https://azure.microsoft.com/en-us/services/storage/blobs/).
* [Azure Active Directory](https://plugins.jetbrains.com/plugin/9083-azure-active-directory) allows using [Azure AD](https://azure.microsoft.com/en-us/services/active-directory/) authentication in TeamCity.


## Upgrading TeamCity in Azure

Note that TeamCity [autoupdate](upgrade.md#Automatic+Update) is not supported in Azure installations.

To upgrade TeamCity, change the value of the `teamcity-version` tag:

<img src="azure-teamcity-tag.png" width="864" alt="TeamCity virtual machine overview"/>

The new version of the TeamCity Docker image will be pulled, and TeamCity will be upgraded automatically.

After the upgrade, restart the virtual machine entirely or restart the server and agent services via the following commands:

```Shell
sudo systemctl restart teamcity-server.service
sudo systemctl restart teamcity-agent.service

```

## Diagnostics

You can connect to the virtual machine and use default `systemd` tools to control TeamCity services. For example, to monitor the server log during the upgrade use the following command:

```Shell
sudo systemctl status teamcity-server.service

```