[//]: # (title: Managing Subscription and Resources)
[//]: # (auxiliary-id: Managing Subscription and Resources)

The __Administration | Subscription & Resources__ page allows you to manage the TeamCity Cloud subscription, get extra build resources, and view the statistics of expenses. This page is only available to [Cloud Account Administrators](#Managing+Cloud+Account+Administrators).

## Upgrade Subscription

Under the __My Subscription__ section, you can see how many credits and resources are left in your subscription. By clicking __Upgrade subscription__, you will be redirected to the JetBrains e-store where you can increase your subscription level if necessary.

## Get Extra Credits and Resources

Under the __Available Resources__ section, you can view more detailed information on resources and credits left, and exchange credits for more committer slots, concurrent builds on self-hosted agents, and artifact storage. Read more about on-demand resources in [TeamCity Cloud Subscription and Licensing](teamcity-cloud-subscription-and-licensing.md#On-demand+Cloud+Resources).

## Prepay JetBrains-Hosted Build Agents Monthly

You can prepay one or more JetBrains-hosted agents on a monthly basis and run an unlimited number of builds on them, instead of paying per build time. A single prepaid agent can run one build at a time.

If thoroughly planned, using such agents allows significantly reducing costs: if an agent is supposed to run builds more than 6 hours per workday, it makes sense to prepay it in advance.

Paying per build time on regular JetBrains-hosted agents allows starting as many parallel builds as needed and makes it a more flexible approach. However, using prepaid agents is convenient if you don't mind how many builds run in parallel and just want to pay a fixed sum monthly.

You can prepay a JetBrains-hosted agent anytime. To do this:
1. Click __Buy agents__ under the __Per-Month Agents__ section.
2. Choose the [type of the agent instance](supported-platforms-and-environments.md#JetBrains-Hosted+Agents) and the number of instances to add.
3. Apply the changes.

Note that the agent is prepaid until the end of the current calendar month. The price is scaled depending on the number of days left in this month.

>You can distinguish a prepaid agent in the system by the word `Monthly` in its name.

Prepaid agents can be used together with regular JetBrains-hosted agents. If a new build can be assigned to either a regular or prepaid agent, TeamCity will always assign it to the prepaid one.

## Get Self-Hosted Build Agents

Self-hosted build agents are "bring-your-own" agents that can be connected to TeamCity Cloud, but are hosted and managed by the customer. Self-hosted build agents are useful if you require your own specialized set of build software, specialized build environments, and so on.

Customers can utilize their build credits to add self-hosted build agents to their instance at a flat monthly rate of 20,000 build credits. Self-hosted build agents do not draw on any additional build credits. You can connect an indefinite number of self-hosted agents to your TeamCity Cloud instance and run an unlimited number of builds on them. When such an agent is redeemed, it increases the number of concurrent builds you can perform on self-hosted agents by 1.

To redeem a self-hosted agent, go to __Administration | Subscription & Resources__ and click __Manage__ in the _Concurrent Builds_ column. Enter the number of required agents and click __Apply__. Note that you can only acquire as many agents as the current quantity of build credits allows it, even if no credits are to be charged in the current month.

<img src="get-self-hosted-agent.png" width="700"/>

After you redeem a self-hosted agent(s), you need to install them on your machine(s) and connect them to your TeamCity Cloud instance. To download an agent distribution package, go to the __Agents__ page, click __Install agent__, and choose a preferred format. Read how to [install and connect them to your instance](configure-agent-installation.md). For a more convenient connection, make sure to generate a unique authentication token for each agent as described [here](install-and-start-teamcity-agents.md#Generating+Authentication+Token).

## Review Statistics of Expenses

This page also lets you track the details of your TeamCity cloud instance:
* The __Committers Report__ tab shows statistics of active committers, who actively contribute to the source code, and users with a free license, who have authored less than 10 commits during the last 30 days.
* The __Build Credits__ tab shows all operations with acquiring and spending credits, in a table or chart view.
* The __Storage__ tab shows trends of used artifact storage and data transfer capacity.

## Managing Cloud Account Administrators

System Administrators can assign a user to a _Cloud Account Administrator_ role in __Administration | Cloud Account Administrators__. Such a user will be granted access to the __Subscription & Resources__ page and will be able to exchange build credits for cloud resources. Roles can be unassigned anytime.