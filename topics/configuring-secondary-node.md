[//]: # (title: Configuring Secondary Node)
[//]: # (auxiliary-id: Configuring Secondary Node)

In addition to the main TeamCity server, you can start a secondary server (node) sharing the Data Directory and external database with the main one. Read more in [Multinode Setup](multinode-setup.md).

This page describes how to configure a secondary node.

<anchor name="running-builds-node-discontinued"/>

<note>

Note that since TeamCity 2019.2 the [Running Builds Node](https://confluence.jetbrains.com/display/TCD18/Configuring+Running+Builds+Node) is discontinued in favor of the secondary node. Read more in [Multinode Setup](multinode-setup.md#running-builds-node-discontinued).

</note>

## Installing Secondary Node

To install a secondary node, follow these steps on the secondary node machine:

1. [Install](installing-and-configuring-the-teamcity-server.md) the TeamCity software as usual: download the distribution package and follow the installation wizard.
2. Provide the path to the shared Data Directory via the `TEAMCITY_DATA_PATH` [environment variable](configuring-teamcity-server-startup-properties.md#Standard+TeamCity+Startup+Scripts).
3. Add additional arguments to the `TEAMCITY_SERVER_OPTS` environment variable:


```Plain Text
TEAMCITY_SERVER_OPTS = -Dteamcity.server.nodeId=<nodeID> -Dteamcity.node.data.path=<path_to_node_data_directory>  -Dteamcity.server.rootURL=<node url>

```

where

* `<nodeID>` is the ID of the node that will be displayed on the __Administration | Nodes Configuration__ page.
* `<path_to_node_data_directory>` is the path to the node Data Directory (see [Node-Specific Data Directory](multinode-setup.md#Node-Specific+Data+Directory)).
* `<node url>` is the secondary node root URL. Make sure this URL is accessible from the main server and agents.

### Secondary Node Memory Settings

The secondary node requires the same memory settings as the main node. If you have already configured the `TEAMCITY_SERVER_MEM_OPTS` [environment variable](configuring-teamcity-server-startup-properties.md) for the main node, make sure to use the same variable for the secondary node. If your main node uses a 64-bit JVM, the secondary node must use a 64-bit JVM as well.

## Startup & Shutdown

A secondary node can be started/stopped using regular TeamCity scripts (`teamcity-server.bat`, `teamcity-server.sh`) or Windows services.

Before starting more than one secondary node, make sure the database is configured to accept enough parallel connections to handle requests from all nodes. By default, each node requires 50 connections to the database.

The secondary node server uses the same approach to logging as the main server. You can check the state of the startup in the `<TeamCity_installation_directory>/logs/teamcity-server.log` file. Open `<secondary_node_URL>` in your browser to see the regular TeamCity startup screens.

A secondary node, as well as the main server, can be stopped or restarted while builds are running. If agents cannot connect to a secondary node for some time, they will reroute their data to the main server. If the main server is unavailable either, agents will keep their data and resend it once the servers are available again.

## Assigning Responsibilities

By default, a newly started secondary node provides a read-only user interface and does not perform any background activity. You can assign the following additional responsibilities to each secondary node in __Administration | Server Administration | Nodes Configuration__:

* Processing data produced by builds
* VCS repositories polling
* Processing build triggers

<img src="Nodes.png" alt="Secondary node responsibilities"/>

A node assigned with any responsibility will allow users to perform the [most common actions](#User-level+Actions+on+Secondary+Node) on builds.

You can enable and disable responsibilities for nodes at any moment.

### Processing Data Produced by Builds on Secondary Node

It is possible to use one or more secondary nodes to process traffic from the TeamCity agents. This allows moving the related load to a separate machine from the main TeamCity server which will improve the TeamCity performance when handling hundreds of concurrent and actively logging builds.

In general, you do not need a separate node for running builds unless you have more than 400 agents connected to a single server. Using a secondary node allows you to significantly increase the number of agents which the setup can handle.

Once you assign the _Processing data produced by builds_ responsibility to a node, all newly started builds will be routed to this node. The existing running builds will continue being executed on the main server. When you disable the responsibility, only the newly started builds will be switched to the main server. The builds that were already running on the secondary node will continue running there.

### VCS Repositories Polling on Secondary Node

Usually, the main TeamCity server polls the VCS repositories for changes to detect new commits. You can delegate VCS polling to the secondary node thus improving the performance of the main server. Only one secondary node can be assigned to this responsibility.

Once you assign the _VCS repositories polling_ responsibility to a node, it may take some time for the main server to finish the polling activities in progress, and then the secondary node will pick up this task. When you disable the responsibility, the main server will start polling VCS repositories.

If you have commit hooks configured on the main server, no changes in hooks are required: the hooks will continue working if the VCS polling is delegated to the secondary node.

### Processing Triggers on Secondary Node

In setups with many build agents, a significant amount of the main server\'s CPU is allocated to constant processing of build triggers. By enabling the _Processing build trigger_ responsibility for one or more secondary nodes, you can distribute the trigger processing tasks and CPU load between the main node and the responsible secondary ones. TeamCity distributes the triggers automatically but you can see what triggers are currently assigned to each node.

## User-level Actions on Secondary Node

If at least one responsibility is assigned to a secondary node, it will allow performing the most common user-level actions:
* Triggering a build, including a custom or personal one
* Stopping/deleting and pinning/tagging/commenting builds
* Assigning investigations and muting build problems and tests
* Marking a build as successful/failed
* Editing build changes\' descriptions
* Merging sources and labeling sources actions
* Adding builds to favorites
* Changing settings of user profiles, including general settings, groups and roles, access tokens, VCS usernames, and notification rules
* Reordering and hiding projects and build configurations
* Checking for pending changes
* Agent-related actions (see the [list of actions](https://youtrack.jetbrains.com/issue/TW-65199))

See the related tasks in our issue tracker for the full list of available actions: [TW-62749](https://youtrack.jetbrains.com/issue/TW-62749), [TW-63346](https://youtrack.jetbrains.com/issue/TW-63346).

Administrator-level actions are not yet available on secondary nodes. Use the main server if you need to change the server configuration.

## Upgrade & Downgrade

We recommend running the main server and all secondary nodes on the same version of TeamCity. Read more in [Multinode Setup](multinode-setup.md#Upgrade+%26+Downgrade).

## Authentication & License

The main and secondary nodes operate under the same license.

Secondary nodes use the same authentication settings as the main server.

## Global Settings

Secondary notes use the same [global settings](teamcity-configuration-and-maintenance.md) (path to artifacts, version control settings, and so on) as the main server.

## Internal Properties

All secondary nodes rely both on common [internal properties](configuring-teamcity-server-startup-properties.md#TeamCity+internal+properties), stored in the [shared data directory](multinode-setup.md#Shared+Data+Directory), and specific properties, configured separately for each node. The node-specific properties have a higher priority and overwrite the common values in case of a conflict.

To disable any common property on a given secondary node, pass it using the following syntax: `-<property_name>`.

## Project Import

You can import projects to the main server, [as usual](projects-import.md). Secondary nodes will detect the imported data in the runtime, without restarting.

Projects cannot be imported on secondary servers.

## Using Plugins

Secondary nodes have access to all plugins enabled on the main server. If you enable a plugin on the main server, you need to reload secondary nodes so they can use this plugin.

Currently, the following bundled plugins are disabled on secondary nodes:
* Investigations Auto Assigner
* Jira Cloud integration
* Kubernetes support
* Install Agent on a remote host (agent push)
* WebHooks support

<note>

Secondary nodes can use only a limited set of external plugins.

</note>

## Clean-up

The TeamCity [clean-up](clean-up.md) task runs on the main TeamCity server only. In a multinode configuration, as well as in a single node configuration, the task can run while the secondary servers are handling their operations.

## Backup & Restore

The [backup](teamcity-data-backup.md) through the TeamCity web interface can be done on the main TeamCity server only. The backup from the command line can be done on both the main server and the secondary nodes.

The [restore](restoring-teamcity-data-from-backup.md) operation can be done on either of the nodes, but only if all servers using the TeamCity database and Data Directory are stopped.
