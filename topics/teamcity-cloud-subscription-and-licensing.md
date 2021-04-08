[//]: # (title: TeamCity Cloud Subscription and Licensing)
[//]: # (auxiliary-id: TeamCity Cloud Subscription and Licensing)

TeamCity Cloud services can be provided in two modes:
* By subscription
* On demand, if any of the subscription limits is reached

This document details the terms of subscription and licensing in TeamCity Cloud. For more information, see our [F.A.Q]() or contact us via our [issue tracker](https://youtrack.jetbrains.com/issues/TCC).

Refer to the [subscription glossary](#Cloud+Licensing+Terminology) to learn about the terms used in this document.

## TeamCity Cloud Subscription

### Committers-Based Subscription

The TeamCity Cloud subscription plans are predominantly based on the number of _[committers](#cloud-committers)_, that are users who make changes to the project code. All users who author VCS changes require an active _committer slot_ in TeamCity Cloud. Before acquiring the subscription, we suggest that you analyze the number of potential committers in all projects you are about to run in TeamCity. If you need help estimating the subscription plan, [contact us for a demo](https://www.jetbrains.com/teamcity/request-a-demo/).

>If you have utilized all the committer slots provided in terms of the subscription and a new user makes a commit to your source project, TeamCity Cloud will not start new builds with changes from that committer. Other builds will continue to start as usual. Read how you can [get extra resources](#On-demand+Cloud+Resources) in this case.

Note that only contributors to the source take the committer clots. An unlimited number of users (managers, QA, [and so on](#cloud-web-users)) can get access to the TeamCity web interface.

Each committer slot is provided with a set of _build resources_:
* [build minutes]() on our [JetBrains-hosted agents](#cloud-jb-hosted-agents);
* slots for [self-hosted build agents](#cloud-self-hosted-agents);
* [storage](#cloud-storage);
* [data transfer capacity](#cloud-data-transfer).

The more committers you have, the more build resources you are granted with. Please refer to [our website]() to see the exact rates.

### Build Credits

_Build credits_, granted per every committer, are your currency in TeamCity Cloud and can be flexibly utilized as needed. You determine how you want to utilize the build credits when you need them.

At the end of each month build credits from your subscription expire, and a new set of build credits is replenished.

Each minute of build time on [JetBrains-hosted agents](#cloud-jb-hosted-agents) will draw on your [build-credit](#cloud-build-credits) allotment. The exact rate depends on the type of your build agent instance tier. We also offer prepurchased build agents for customers who can more accurately predict their build agent utilization.

By default, when using the JetBrains-hosted build agents, we draw build credits at the per-minute rate of that instance type. If you anticipate a high utilization rate of our JetBrains-hosted agent, you have the option to pay for that agent upfront to help reduce costs. [Prepaid build agents](#cloud-prepaid-agents) are permanently connected; JetBrains-hosted build agents that allow unlimited builds and do not draw on your build credits at a per minute rate.


Build credits from your subscription expire at the end of each month and are renewed at the beginning of every month.

If you purchase build credits a-la-carte, those build credits do not expire.

Are build credits drawn while agents are being spun up and terminated?
No, you only pay for the build time when utilizing JetBrains-hosted build agents.

Do build credits expire?
The number of build credits you receive through your subscription plan is dependent on the number of committer slots you purchase. Any remaining build credits at the end of the month expire, and a new set of credits are replenished at the beginning of the next month.

Do self-hosted or monthly pre-purchased build agents draw on build credits at a per-minute rate?
No, self-hosted build agents, or monthly pre-purchased JetBrains-hosted build agents can be connected to TeamCity Cloud at a flat rate per month. Once connected, builds running on self-hosted and pre-purchased agents do not draw on any additional build credits.


### Free Trial Subscription

You can [try TeamCity Cloud for free]() for 14 days. The free trial subscription plan includes 12,000 build credits (good for up to 20 build hours on our Linux small instance type), unlimited parallel builds, 120 GB of storage, and up to 3 self-hosted build agents.

## On-demand Cloud Resources


You can redeem existing build credits to add a new committer slot in this situation or purchase additional committer slots through the JetBrains e-store.

Customers can utilize their build credits to add self-hosted build agents to their instance at a flat monthly rate of 20,000 build credits, or about $16 USD. Self-hosted build agents do not draw on any additional build credits. You can run an unlimited number of builds on self-hosted build agents.

When a self-hosted build agent is redeemed in your TeamCity Cloud instance, it increases the number of concurrent builds you can perform on self-hosted agents by 1. You will be able to connect as many self-hosted agents as you wish. This allows you to connect a pool of self-hosted agents to your TeamCity instance that can suit a wide variety of build configurations and requirements.

Additional build credits or build resources can be purchased on demand. Build credits purchased this way do not expire.

There are two ways to add more build credits to your TeamCity instance. You can add additional committer slots, which come with a set of build credits, or if you don’t require any additional committers, you can purchase build credits a-la-carte. More information is available on our [pricing] page.

Build credits from your subscription (credits you receive alongside each committer) expire at the end of each month and are replenished at the beginning of the next month. Build credits purchased a-la-carte do not expire.

Can I purchase build credits, storage, data transfer a-la-carte?
Yes. You get a set of build resources with each committer slot, but you can purchase additional build credits a-la-carte to expand your capabilities. Build credits can be redeemed for additional storage, data transfer, build time, self hosted agents, and committers as needed.


Can I use build credits to pay for additional resources?
Yes, build credits act as your “currency” in TeamCity Cloud. You can exchange build credits for additional storage, data transfer, self-hosted build agents, build minutes, monthly pre-purchased build agents, or additional committer slots.

Build credits from your subscription expire and are replenished at the end of each month. Build credits purchased a-la-carte do not expire and carry over month to month.


Can I exchange unused artifact storage, data transfer for additional build credits?
No, this type of exchange is not allowed.


### Cloud Licensing Terminology

<table>
<tr><td></td><td></td></tr>

<tr>
<td id="cloud-build-credits" auxiliary-id="cloud-build-credits">

__Build credits__

</td>

<td>

For each [committer](#cloud-committers) slot purchased, TeamCity Cloud customers are granted build credits. They act as your "currency" in TeamCity Cloud. Build credits can be redeemed in TeamCity Cloud for build minutes on our [JetBrains-hosted agents](#cloud-jb-hosted-agents) or exchanged at a flat rate for [self-hosted build agents](#cloud-self-hosted-agents). You can also exchange build credits for additional [storage](#cloud-storage) or [data transfer](#cloud-data-transfer-capacity).

</td>

</tr>

<tr>
<td id="cloud-build-time" auxiliary-id="cloud-build-time">

__Build time__

</td>

<td>

Build time is calculated based on the total number of minutes your builds spend from start to finish.

</td>

</tr>

<tr>
<td id="cloud-storage" auxiliary-id="cloud-storage">

__Cloud storage__

</td>

<td>


Storage is counted for the results of builds, primarily build artifacts and build logs.

You can assure you remain under your storage capacity limits by maintaining proper clean-up rules in your TeamCity Cloud instance.

</td>

</tr>

<tr>
<td id="cloud-committers" auxiliary-id="cloud-committers">

__Committer__

</td>

<td>

A user who authors VCS (version control system) changes for projects that are built by TeamCity. Once a user authors 10 changes in a VCS, they will occupy one committer slot. Committer slots are released after 30 days of inactivity.

Roughly, each developer for a project will require a committer slot in TeamCity Cloud. Regular [web users](#cloud-web-users) are unlimited in TeamCity Cloud.

</td>

</tr>

<tr>
<td id="cloud-data-transfer" auxiliary-id="cloud-data-transfer">

__Cloud data transfer__

</td>

<td>

Cloud data transfer is counted when you download artifacts and build logs from TeamCity Cloud to outside locations. For example, between self hosted agents and TeamCity Cloud, artifacts downloaded from the browser, or through tools like wget or curl.

</td>

</tr>

<tr>
<td id="cloud-jb-hosted-agents" auxiliary-id="cloud-jb-hosted-agents">

__JetBrains-hosted build agent__

</td>

<td>

In TeamCity Cloud, we provide [build agents](build-agent.md) (with various instance sizes and OS platforms) that can be used out of the box, without any additional configuration. These build agents are configured, maintained, and hosted by [JetBrains](https://www.jetbrains.com/). When a build is added to the [queue](build-queue.md), an agent matching the [build requirements](agent-requirements.md) will automatically spin up and start your build.

JetBrains-hosted build agents come with a preinstalled set of commonly used build software. You can view a complete list [here](supported-platforms-and-environments.md). Additionally, TeamCity build configurations allow you to run [build steps](configuring-build-steps.md) within a [Docker container](docker-wrapper.md). This provides the ability to execute build steps which require software that may not be on the build agents by default.

</td>

</tr>

<tr>
<td id="cloud-self-hosted-agents" auxiliary-id="cloud-self-hosted-agents">

__Self-hosted build agent__

</td>

<td>

Self-hosted [build agents](build-agent.md) are "bring-your-own" build agents that can be connected to TeamCity Cloud, but are hosted and managed by the customer. Self-hosted build agents are useful if you require your own specialized set of build software, specialized build environments, and so on.

</td>

</tr>

<tr>
<td id="cloud-prepaid-agents" auxiliary-id="cloud-prepaid-agents">

__Prepaid build agent__

</td>

<td>

Prepaid build agents can provide cost savings, however one drawback is reduced parallelism: while per-minute pricing allows for an unlimited number of parallel builds, with per-month pricing you can only run one build at a time for each per-month build agent you’ve purchased.

</td>

</tr>

<tr>
<td id="cloud-web-users" auxiliary-id="cloud-web-users">

__Web users__

</td>

<td>

QA, managers, or other team members who do not author changes will not take up committer slots. They can view build results, modify build configurations, and take other actions in the TeamCity user interface with no licensing limits.

</td>

</tr>

</table>

