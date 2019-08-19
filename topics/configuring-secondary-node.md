[//]: # (title: Configuring Secondary Node)
[//]: # (auxiliary-id: Configuring Secondary Node)

In addition to the main TeamCity server, you can start a secondary server (node) sharing the data directory and database with the main one. Read more in [Multinode Setup](multinode-setup.md).

This page describes how to configure a secondary node.

On this page:

<tag-list of="chapter" mode="tree" depth="4"/>

## Installing Secondary Node

To install a secondary node, follow these steps:

1. On the secondary node machine, [install](installing-and-configuring-the-teamcity-server.md) the TeamCity software as usual: download the distribution package and follow the installation wizard.
2. Provide path to the shared data directory via the `TEAMCITY_DATA_PATH` environment variable.
3. Add additional arguments to the `TEAMCITY_SERVER_OPTS` [environment variable](configuring-teamcity-server-startup-properties.md):


```Plain Text
TEAMCITY_SERVER_OPTS = -Dteamcity.server.nodeId=<nodeID> -Dteamcity.node.data.path=<path_to_node_data_directory>  -Dteamcity.server.rootURL=<node url>

```

where

* `<nodeID>` is the ID of the node that will be displayed on the __Administration | Nodes Configuration__ page.
* `<path_to_node_data_directory>` is the path to the node data directory (see [Node-Specific Data Directory](multinode-setup.md#Node-Specific+Data+Directory)).
* `<node url>` is the secondary node root URL. Make sure this URL is accessible from the main server and agents.

### Secondary Node Memory Settings

The secondary node requires the same memory settings as the main node. If you already configured `TEAMCITY_SERVER_MEM_OPTS` [environment variable](configuring-teamcity-server-startup-properties.md) environment variable for the main node, make sure you're using the same variable for the secondary node. And if your main node uses a 64-bit JVM, make sure you're using the 64-bit JVM for the secondary node too.

## Startup & Shutdown

A secondary node can be started\/stopped using regular TeamCity scripts (`teamcity-server.bat`, `teamcity-server.sh`) or Windows services.

Before starting more than one secondary node, make sure that the database is configured to accept enough parallel connections to handle requests from all nodes. By default, each node requires 50 connections to the database.

The secondary node server uses the same approach to logging as the main server. You can check the state of the startup in the `<TeamCity_installation_directory>/logs/teamcity-server.log` file. Open `<secondary_node_URL>` in your browser to see the regular TeamCity startup screens.

A secondary node, as well as the main server, can be stopped or restarted while builds are running. If agents cannot connect to a secondary node for some time, they will reroute their data to the main server. If the main server is unavailable either, agents will keep their data and resend it once the servers are available again.

## Assigning Responsibilities

By default, a newly started secondary node provides a read-only user interface and does not perform any background activity. You can assign the following additional responsibilities to each secondary node in __Administration | Server Administration | Nodes Configuration__:

* Processing data produced by builds
* VCS repositories polling

![Nodes.png](Nodes.png)

You can enable and disable responsibilities for nodes at any moment.

### Processing Data Produced by Builds on Secondary Node

It is possible to use one or more secondary nodes to process traffic from the TeamCity agents. This allows moving the related load to a separate machine from the main TeamCity server which will improve the TeamCity performance when handling hundreds of concurrent and actively logging builds.

In general, you do not need a separate node for running builds unless you have more than 400 agents connected to a single server. Using a secondary node allows you to significantly increase the number of agents which the setup can handle.

Once you assign the _Processing data produced by builds_ responsibility for a node, all newly started builds will be routed to this node. The existing running builds will continue being executed on the main server. When you disable the responsibility, only the newly started builds will be switched to the main server. The builds that were already running on the secondary node will continue running there.

### VCS Repositories Polling on Secondary Node

Usually, the main TeamCity server polls the VCS repositories for changes to detect new commits. You can delegate VCS polling to the secondary node thus improving the performance of the main server. Only one secondary node can be assigned to this responsibility.

Once you assign the _VCS repositories polling_ responsibility for a node, it may take some time for the main server to finish the polling activities in progress, and then the secondary node will pick up this task. When you disable the responsibility, the main server will start polling VCS repositories.

If you have commit hooks configured on the main server, no changes in hooks are required: the hooks will continue working if the VCS polling is delegated to the secondary node.

## Upgrade & Downgrade

It is recommended that the main TeamCity server and all secondary nodes have the same version. Read more in [Multinode Setup](multinode-setup.md#Upgrade+%26+Downgrade).

## Authentication & License

The main and secondary nodes operate under the same license.

Secondary notes use the same authentication settings as the main server.

## Global Settings

Secondary notes use the same [global settings](teamcity-configuration-and-maintenance.md) (path to artifacts, version control settings, and so on) as the main server.

## Project Import

You can import projects to the main server, [as usual](projects-import.md). Secondary nodes will detect the imported data in the runtime, without restarting.

Projects cannot be imported on secondary servers.

## Using Plugins

Secondary nodes have access to all plugins enabled on the main server. If you enable a plugin on the main server, you need to reload secondary nodes so they can use this plugin.

Currently, the following bundled plugins are disabled on secondary nodes:
* Investigations Auto Assigner
* Install Agent on remote host (agent push)

<note>

Secondary nodes operate as read-only servers and can use only a limited set of external plugins.
</note>

## Cleanup

The TeamCity [cleanup](clean-up.md) task runs on the main TeamCity server only. In a multinode configuration, as well as in a single node configuration, the task can run while the secondary servers are handling their operations.

## Backup & Restore

The [backup](teamcity-data-backup.md) through the TeamCity web interface can be done on the main TeamCity server only. The backup from the command line can be done on both the main server and the secondary nodes.

The [restore](restoring-teamcity-data-from-backup.md) operation can be done on either of the nodes, but only if all servers using the TeamCity database and data directory are stopped.


