[//]: # (title: Migrate from TeamCity On-Premises to TeamCity Cloud)
[//]: # (auxiliary-id: Migrate from TeamCity On-Premises to TeamCity Cloud)

TeamCity offers two distribution models:
* __On-Premises__: you decide where to install the server and agents and how to administer them. The [pricing](https://www.jetbrains.com/teamcity/buy/#on-premises) is based on the number of build agents.
* __Cloud__: the server and agents are installed in the cloud and managed by JetBrains. Self-hosted agents can be connected to the cloud server as well. The [pricing](https://www.jetbrains.com/teamcity/buy/#cloud) is based on the number of active committers.

While the On-Premises model suits some of our clients, the Cloud version might be a better match for those teams who prefer a managed service for their CI/CD infrastructure. This article gives general guidelines on how to migrate from your existing On-Premises instance to Cloud. If you still have concerns or questions after reading it, don’t hesitate to [contact us](https://www.jetbrains.com/teamcity/get-in-touch/) for a consultation.

## Differences Between TeamCity On-Premises and Cloud Licensing

TeamCity On-Premises installations are charged based on the maximum number of build agents allowed on the server. You can increase this number by acquiring more agent licenses.

TeamCity Cloud installations are charged based on the number of active committers — users who make more than 10 commits to the source code each month. Naturally, the more commits are made to your projects, the more resources are required to run builds on these commits. To cover these needs, a Cloud subscription grants a set of virtual resources and a certain number of build credits per committer. Credits can be used to acquire extra resources on demand. If you run out of subscription-granted credits, they can be purchased on-demand.  
See more details about the Cloud licensing model [here](teamcity-cloud-subscription-and-licensing.md).

This core difference between the models complicates mapping the costs of TeamCity On-Premises directly onto TeamCity Cloud. The following sections explain how to estimate the potential cost of the Cloud subscription based on the usage data of your On-Premises server.

## Choosing Model

Before making a decision to migrate, please consider the following factors:

* There are differences in functionality between Cloud and On-Premises versions. Cloud instances are installed, updated, and administered by JetBrains. This approach offloads routine maintenance from you and provides a ready-to-use instance in the cloud. However, it may not be the best option for customers who want to fully control their CI/CD environment, or have a highly customized TeamCity instance.  
  Read about all the functional differences [here](getting-started-with-teamcity-cloud.md#Differences+Between+TeamCity+Cloud+and+On-Premises).
* TeamCity Cloud doesn’t allow installing third-party plugins. If there are external plugins installed on your server, you might lose some of the related functionality on migrating to Cloud. We can potentially install plugins [per user request](https://youtrack.jetbrains.com/issues/TCC), but only those specifically verified to work well in the cloud.
* While the On-Premises version limits the number of concurrent builds, the Cloud one scales the number of active agents in correlation with the current demand on the server. This approach allows always running as many parallel builds as needed and prevents long queue times during peak hours.
* You should thoroughly estimate what distribution model works best for you. Depending on the specifics of your operations, the Cloud subscription might eventually cost more or less than the On-Premises one. TeamCity Cloud offers a [14-day trial period](https://www.jetbrains.com/teamcity/download/#section=cloud) — we suggest that you try it before migrating.
* Existing licenses of On-Premises build agents cannot be transferred to your Cloud subscription. After migrating, you can reconnect any number of these agents to the Cloud instance, but the Cloud model will limit how many concurrent builds can be run on them. To raise this limit, you will need to exchange build credits for more slots (for the exact rate, see Option 3 [here](https://www.jetbrains.com/teamcity/buy/#cloud)).
* Migration takes time and effort. While you can export data from your On-Premises installation, not all of it can be imported to a Cloud instance. Read more details in [this section](#Migration+Process).

In general, TeamCity Cloud gives an advantage of using a software-as-a-service delivery model for TeamCity. It is a great option for those looking to reduce the time spent managing installations, upgrades, and infrastructure.  
TeamCity On-Premises may be more suitable for organizations that require fine-grained control of their build environment or are looking to heavily customize or make use of third-party plugins in TeamCity. It is also more convenient if you keep other parts of the infrastructure, such as VCS or issue trackers, in-house.

## Evaluating Costs

It might be hard to predict the exact amount of required building resources in advance, but it is possible to roughly estimate the cost of your potential Cloud subscription by inspecting the following On-Premises data:
* __Total build time__ you typically expect per month across all builds (in hours).  
To get these statistics, go to __Administration | Build Time__ and choose the “last month” filter to see the monthly total of building hours.
* A __number of active committers__ to your source code, per month.  
If you have trouble estimating, you can try using [this dedicated plugin](https://plugins.jetbrains.com/plugin/17629-committers-count). After [installing it](https://www.jetbrains.com/help/teamcity/installing-additional-plugins.html), you will see a new __Committers__ tab in __Administration__. It shows the number of users who made commits to your source code during the previous 30 days.
* __OS and hardware of agent machines required to run your builds__.  
TeamCity Cloud supports two types of agents: _hosted by JetBrains_ (autostarted in the Cloud on demand) and _self-hosted_ (machines running within your network or private cloud). It’s up to you where to install the self-hosted agents and what specs/OS to use; JetBrains-hosted agents can be run on [several types of virtual machines](supported-platforms-and-environments.md#JetBrains-Hosted+Agents).  
You can mix these types of agents however you like. Using only JetBrains-hosted agents might be enough for some pipelines. But, if you plan to install and use self-hosted agents as well, it is important to estimate the maximum number of concurrent builds you want to run on them.

After gathering all this information, please read the [notes on subscription levels](teamcity-cloud-subscription-and-licensing.md#Subscription+Levels) and use [this calculator](https://www.jetbrains.com/teamcity/buy/#cloud) to estimate the billing:
* Enter the _number of active committers_ in the calculator. You will see how the granted virtual resources (storage and data transfer capacity) and build credits scale in correlation with the number, and how much will be charged for the respective subscription, yearly or monthly.
* You can spend the granted build credits on whatever resources you like. For example, buy build minutes on JetBrains-hosted agents (the calculator shows how many you can get if spending all the granted credits). You can also use them to get more _committer slots_, more resources, or more _concurrent builds on self-hosted agents_ (that’s when you will need the knowledge of the specifics of your agent machines).  
Note that the number of credits granted per subscription resets every month. Additional build credits can be purchased on-demand whenever you require more resources. Any unused additionally purchased build credits roll over month-to-month.

Per request, our sales engineers can provide you with a more thorough estimation. Make sure to gather all the information listed above and use [this form](https://www.jetbrains.com/teamcity/get-in-touch/) to contact us.

## Migration Process

In general, the migration to Cloud looks as follows:

1. [Register a TeamCity Cloud instance](https://www.jetbrains.com/teamcity/cloud/) with the domain name of your choice (`<name>.teamcity.com`) and get 14-day trial access free of charge.
2. Back up the TeamCity On-Premises data, as described [here](https://www.jetbrains.com/help/teamcity/creating-backup-from-teamcity-web-ui.html). Make sure that the major version of your TeamCity On-Premises server is the same as the current Cloud version (currently, 2021.2).  
   Note that the backup file of a big server may be quite large as well. TeamCity Cloud will allow importing files up to \~1.5 GB. If your backup file exceeds this limit, consider excluding the build history from its scope.  
  This will allow transferring the settings of your projects, build configurations, and users, as well as build history, changelogs, and statistics. Some data, such as build artifacts and build logs, cannot be imported to a new server. See the full list of potential transfer implications [here](projects-import.md#Data+not+included+into+import).
3. [Import](projects-import.md) the backed-up On-Premises data to your new Cloud instance.
4. If necessary, install extra self-hosted agents or [switch the existing ones to the new server URL](configure-agent-installation.md).
5. Evaluate the TeamCity Cloud functionality during the trial period. To finalize the migration, purchase the Cloud subscription and shut down the On-Premises server.

<seealso>
        <category ref="admin-guide">
            <a href="teamcity-cloud-subscription-and-licensing.md">TeamCity Cloud Subscription and Licensing</a>
        </category>
        <category ref="external">
            <a href="https://teamcity-support.jetbrains.com/hc/en-us/categories/360003110659-TeamCity-Cloud-FAQ">TeamCity Cloud FAQ</a>
        </category>
</seealso>
