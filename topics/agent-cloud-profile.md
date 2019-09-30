[//]: # (title: Agent Cloud Profile)
[//]: # (auxiliary-id: Agent Cloud Profile)

On this page:

<tag-list of="chapter" mode="tree" depth="4"/>

A _cloud profile_ is a collection of settings for TeamCity to start virtual machines with installed TeamCity agents on\-demand while distributing a build queue. Configuring a cloud provider profile is one of the steps required to [enable agent cloud integration](teamcity-integration-with-cloud-solutions.md) between TeamCity and a cloud provider. The settings of profiles slightly vary depending on the cloud type. 

<tip>

Note that after an upgrade, if you use Kotlin DSL for your TeamCity project settings, you need to update your DSL as described [here](upgrading-dsl.md).
</tip>

## Configuring Cloud Profile

Cloud profiles are configured in the __Cloud Profiles__ section of the __Project Settings__.

### Specifying profile settings

The following profile settings have to be provided:

<table><tr>

<td>

Setting


</td>

<td>

Description


</td></tr><tr>

<td>

Profile name


</td>

<td>

Provide a name for the profile.


</td></tr><tr>

<td>

Description


</td>

<td>

Provide an optional profile description.


</td></tr><tr>

<td>

Cloud type


</td>

<td>

Select the cloud provider type from the drop-down menu.


</td></tr><tr>

<td>

Server URL


</td>

<td>

The URL that the agents cloned from the image will use to connect to the TeamCity server. This URL must be available from the build agent machine.   
If this field is left empty, agents will use the [TeamCity server URL](configuring-server-url.md) specified on the __Administration | Global Settings__ page.


</td></tr><tr>

<td>

Terminate instance idle time


</td>

<td>

Instruct TeamCity to stop a cloud agent machine using this setting and the __Additional terminate condition__ options. Specify the period (in minutes) for TeamCity to wait before stopping an idle build agent.   
Leave empty for no timeout.


</td></tr><tr>

<td>

Additional terminate conditions


</td>

<td>

* _After certain work time (minutes)_:   
Specify the working time (in minutes) for the agent after which the instance will be terminated. If the time elapses while a build is in progress, the agent will wait until the current build finishes.
* _On idle, close to an hour (minutes left to the end of hour)_:   
Specify how many minutes before the full hour an idle instance should be stopped: this allows avoiding charges for partial hours if your virtual machines are billed in whole hours, for example, when using Amazon EC2 instances.
* _After the first build_:   
Select this option if you want TeamCity to stop the virtual machine immediately after the first build finishes. TeamCity will disable the build agents, and no more builds will be run on the same machine.


</td></tr></table>

Next, you need to provide the cloud access information which will differ depending on the provider. After that, you can check the connection and add an image to be used as a source for TeamCity cloud agents.

You can limit the number of instances across all images in the cloud profile (__Maximum instances count__) and/or set the limit per image (in [image settings](#Adding+Agent+Image)).

### Adding Agent Image

Click __Add Image__ and configure the required options for the image. 

You can specify which [agent pool](agent-pools.md) the agents should belong to. You can only select the pool that contains this project and/or its subprojects. Pools containing projects other than the current one and its subprojects will not be available for assignment. If the selected agent pool is changed in the future so that the criteria are not met or if the agent pool is not specified, TeamCity will automatically assign the cloud agents to the _project agent pool_. You can also select the _\<Project pool\>_ in the drop-down menu manually.   
TeamCity automatically composes the project pool containing agents from all cloud profiles of the current project and all its subprojects. Thus, the added image will be available to all the subprojects as well. On the __Agents | Pools__ page, this pool is marked as "_\<Project name\> project pool_". Project pools cannot be deleted or modified.


After an Agent Cloud profile is created with one or several sources for virtual machines, TeamCity does a test start for all the virtual machines specified in the profile to learn about the agents configured on them. Once agents are connected, TeamCity calculates their build configurations\-to\-agents compatibility and stores this information.

When a cloud profile is changed, TeamCity detects modifications immediately and forces shutdown of the agents started prior to these changes once the agents finish the current build.

## Viewing Cloud Agent Information

The agents' information is displayed on the __Agents | Cloud__ page under the __&lt;Profile name&gt;__ drop\-down.

## Enabling/disabling Cloud Integration in Project

You can enable or disable integration for a project and/or its subprojects via the TeamCity web UI using the _Change cloud integration status_ option on the __Cloud Profiles__ page.

__ __