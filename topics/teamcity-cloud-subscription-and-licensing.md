[//]: # (title: TeamCity Cloud Subscription and Licensing)
[//]: # (auxiliary-id: TeamCity Cloud Subscription and Licensing)

TeamCity Cloud services are provided by a monthly or annual subscription which includes a predefined set of resources. If you reach any of the subscription limits, you can get additional resources on demand.

This document details the terms of a subscription and licensing in TeamCity Cloud. For more information, see our [Terms of Service](https://www.jetbrains.com/legal/docs/teamcity/teamcity_cloud.html), [F.A.Q](https://teamcity-support.jetbrains.com/hc/en-us/categories/360003110659-TeamCity-Cloud-FAQ), or contact us via our [issue tracker](https://youtrack.jetbrains.com/issues/TCC).

Refer to the [licensing glossary](#Cloud+Licensing+Terminology) to learn about the terms used in this document.

## TeamCity Cloud Subscription

### Subscription Levels

TeamCity Cloud subscription levels are predominantly based on the number of _[committers](#cloud-committers)_, that are users who make changes in the project code. All users who author 10 or more VCS changes over a 30-day period require a _committer slot_ in TeamCity Cloud. Before acquiring the subscription, we suggest that you estimate the number of potential committers in all projects you are about to run in TeamCity. If you need help choosing the subscription level, [contact us for a demo](https://www.jetbrains.com/teamcity/get-in-touch/).

>If you have utilized all the committer slots provided with your subscription and a new user makes 10 or more commits to your source project within a 30-day period, TeamCity Cloud will not be able to start new builds with the successive changes from that committer until an additional committer slot is acquired. Other builds will continue to start as usual. Read how you can [get extra committer slots](#On-demand+Cloud+Resources) in this case.

Note that only contributors to the source code take the committer slots. An unlimited number of non-committing users (managers, QA, [and so on](#cloud-web-users)) can get access to the TeamCity web interface.

Each acquired committer slot in the subscription is provided with a fixed amount of [storage](#cloud-storage) for build results and [data transfer capacity](#cloud-data-transfer). You also get a number of [build credits](#cloud-build-credits) per each committer: they are automatically spent on build time on agents and can be exchanged for extra [build resources]() when necessary. The more committers you have, the more credits and build resources you are proportionally granted with. Please refer to [our website](https://www.jetbrains.com/teamcity/buy/#cloud) to see the exact rates.

You can manage your subscription on the __[Resources & Subscription](managing-subscription-and-resources.md)__ administration page in TeamCity.

### Using Build Credits

_[Build credits](#cloud-build-credits)_ are granted per committer and serve as your currency in TeamCity Cloud and can be flexibly utilized according to your needs. At the end of each month, the remaining build credits expire from your subscription, and a new set of build credits is provided at the beginning of the next month.  
If you purchase [additional build credits](#On-demand+Cloud+Resources), they do not expire.

Build credits are automatically spent on build time on agents. You can also exchange them to get more committer slots and concurrent builds on self-hosted agents, as well as on extra [storage](#cloud-storage) and [data transfer capacity](#cloud-data-transfer).

>It is easier to stay under storage capacity limits if you configure proper [clean-up rules](clean-up.md) in your TeamCity Cloud instance.

TeamCity Cloud can run builds on two types of agents: hosted by JetBrains and self-hosted. Each minute of build time on [JetBrains-hosted agents](#cloud-jb-hosted-agents) will expend a certain number of build credits. The exact rate depends on your build agent [instance type](#cloud-instance-type).

In case with [self-hosted agents](#cloud-self-hosted-agents), build time is not counted. Instead, you need to purchase an extra slot for each concurrent build running on a self-hosted agent. For example, if you purchase 3 slots monthly, you would be able to run up to 3 builds on self-hosted agents, connected to your TeamCity server, at each moment of time during this month.

>Coming soon: We will also offer [prepaid build agents](#cloud-prepaid-agents) for customers who can more accurately predict their build agent utilization. If you anticipate a high utilization rate of your JetBrains-hosted agents, you will be able to use this method to help reduce costs.

### Free Trial Subscription

You can try TeamCity Cloud for free for 14 days. The free trial subscription plan includes unlimited committer slots, 12,000 build credits (good for up to 20 build hours on our Linux small instance type), unlimited parallel builds, 120 GB of storage, and up to 3 concurrent self-hosted builds. See the detailed information on [our website](https://www.jetbrains.com/teamcity/cloud/).

## On-demand Cloud Resources

You can get more build credits, atop the ongoing subscription, and spend them on necessary resources. Credits are purchased in packs of 25,000. More information on pricing is available on [our website](https://www.jetbrains.com/teamcity/cloud/).

Build credits purchased this way do not expire. Similarly to credits provided with your subscription, they are spent on build time on JetBrains-hosted build agents and can be redeemed for additional storage, concurrent builds on [self-hosted agents](#cloud-self-hosted-agents), committers, and (in the future updates) [prepaid build agents](#cloud-prepaid-agents). When you buy a resource, its price for the current month is lowered proportionally to how many days are left in the month.

Note that when you buy an additional resource for build credits, it will be automatically renewed each month (that is, build credits will be automatically spent on it) unless you cancel it. If you cancel a purchased resource, it will only reset at the beginning of the next month.

To acquire credits and manage build resources, use the __[Resources & Subscription](managing-subscription-and-resources.md)__ administration page in TeamCity.

## Cloud Licensing Terminology

<table>
<tr><td></td><td></td></tr>

<tr>
<td id="cloud-build-credits" auxiliary-id="cloud-build-credits">

__Build credits__

</td>

<td>

Build credits act as your "currency" in TeamCity Cloud. They are redeemed for build minutes on [JetBrains-hosted agents](#cloud-jb-hosted-agents) or can be exchanged at a flat rate for concurrent builds on [self-hosted agents](#cloud-self-hosted-agents). You can also exchange build credits for additional [storage](#cloud-storage), [data transfer](#cloud-data-transfer-capacity), and committer slots.

Build credits are granted per every committer slot in a subscription and can be purchased on demand.

</td>

</tr>

<tr>
<td id="cloud-build-time" auxiliary-id="cloud-build-time">

__Build time__

</td>

<td>

Build time is calculated based on the total number of minutes your builds spend running from start to finish.

</td>

</tr>

<tr>
<td id="cloud-storage" auxiliary-id="cloud-storage">

__Cloud storage__

</td>

<td>

Provided for storing the results of builds, primarily build artifacts and build logs.

</td>

</tr>

<tr>
<td id="cloud-committers" auxiliary-id="cloud-committers">

__Committer__

</td>

<td>

A user who authors VCS (version control system) changes for projects that are built by TeamCity. A user occupies a committer slot after committing 10 or more changes into your projects during 30 days. This slot is reserved for this user until their last 10th commit gets older than 30 days. For example, if a user makes 5 commits on June 10 plus 5 commits on June 11 and then stops committing, TeamCity will release their committer slot on July 10, after 30 days since the first of these last 10 commits.

</td>

</tr>

<tr>
<td id="cloud-data-transfer" auxiliary-id="cloud-data-transfer">

__Cloud data transfer__

</td>

<td>

Cloud data transfer is counted when you download artifacts and build logs from TeamCity Cloud to outside locations. For example, to self-hosted agents, via the TeamCity web UI, or using tools like wget or curl.

</td>

</tr>

<tr>
<td id="cloud-instance-type" auxiliary-id="cloud-instance-tier">

__Instance type__

</td>

<td>

A type of virtual machine with a preinstalled TeamCity agent instance. A type is defined by an operating system and instance size. For example, Linux Small. Depending on its expected load, each type is provided with a predefined set of hardware resources.

</td>

</tr>

<tr>
<td id="cloud-jb-hosted-agents" auxiliary-id="cloud-jb-hosted-agents">

__JetBrains-hosted build agent__

</td>

<td>

A [build agent](build-agent.md) that can be used out of the box, without any additional configuration. Such build agents are configured, maintained, and hosted by [JetBrains](https://www.jetbrains.com/). When a build is added to the [queue](build-queue.md), a new JetBrains-hosted agent, matching the [build requirements](agent-requirements.md), automatically launches and starts your build.

JetBrains-hosted build agents come with a preinstalled set of commonly used build software. You can view a complete list [here](supported-platforms-and-environments.md). Additionally, TeamCity can run certain [build steps](configuring-build-steps.md) within a [Docker container](docker-wrapper.md). This allows executing steps that require software that may not be on the build agents by default.

</td>

</tr>

<tr>
<td id="cloud-self-hosted-agents" auxiliary-id="cloud-self-hosted-agents">

__Self-hosted build agent__

</td>

<td>

A build agent that can be connected to TeamCity Cloud, but is hosted and managed by the customer. Self-hosted build agents are useful if you require a custom set of build software, build environments, and so on.

You can spend build credits on adding self-hosted build agents to your instance at a flat monthly rate. These agents do not draw on any additional build credits and can run an unlimited number of builds. When a self-hosted build agent is redeemed in your TeamCity Cloud instance, it increases the number of concurrent builds you can perform on self-hosted agents by 1.

</td>

</tr>

<tr>
<td id="cloud-prepaid-agents" auxiliary-id="cloud-prepaid-agents">

__Prepaid build agent__

</td>

<td>

[Coming soon]

A build agent hosted by JetBrains and prepaid with a fixed number of credits monthly, instead of paying per build time. If thoroughly planned, using such agents allows saving costs but is not as flexible as using [regular JetBrains-hosted agents](#cloud-jb-hosted-agents), as you pay for each agent that can run one build at a time instead of autostarting as many parallel builds as needed. You can combine these agents with regular JetBrains-hosted agents whenever necessary.

</td>

</tr>


<tr>
<td id="cloud-web-users" auxiliary-id="cloud-web-users">

__Web users__

</td>

<td>

QA, managers, or other team members who do not author changes and do not occupy committer slots. They can view build results, modify build configurations, and perform other actions in the TeamCity UI with no licensing limits.

</td>

</tr>

</table>