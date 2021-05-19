[//]: # (title: Multinode Setup)
[//]: # (auxiliary-id: Multinode Setup;Configuring Secondary Node)

The TeamCity server can be configured to use multiple nodes (or servers) for high availability and flexible load distribution. It is possible to set up a cluster of TeamCity nodes, where each node is responsible for different tasks, like processing data from builds or collecting changes from VCS repositories. Or, to keep one main node that does all the work and a secondary node which provides a read-only interface. In case the main node goes down, all data processing can be switched to the secondary node with minimum downtime.

As the main use case of a multinode setup is to achieve high availability (HA), this article will focus on configuring an HA cluster. However, you can use the same methods for any setup with multiple TeamCity servers: for example, to distribute load between several machines.

## Main vs. secondary node

A TeamCity cluster can have one _main node_ and multiple _secondary nodes_. The main node is the "preferred" one. By default, it receives all incoming HTTP requests. It also performs critical background tasks, such as starting builds. A secondary node mostly serves as a backup server, necessary for the failover. For better load distribution and performance optimization, it can also be granted with [additional responsibilities](#Responsibilities).

<img src="multinode-setup.png" width="702" alt="TeamCity setup with two nodes"/>

## Shared Data Directory

The main TeamCity node and secondary nodes must be able to access and share the same [TeamCity Data Directory](teamcity-data-directory.md).

Here are main recommendations on setting up the shared Data Directory:
* For a high-availability setup, store it on a separate and well-performing machine, so it is accessible even when the main node goes down.
* All TeamCity nodes’ machines should be able to access it in the read\/write modes.
* The typical Data Directory mounting options are SMB and NFS. TeamCity uses the Data Directory as a regular file system so all basic file system operations should be supported.
* The I/O operations count or I/O volume limits should not be restricted by the storage or mounting option.
* Make sure to review performance guidelines for your storage solution. For example, increasing MTU for the network connection between the server and the storage usually increases the artifact transfer speed.

### Disable Network Client Caches on Data Directory Mounts

It is important that all the nodes "see" the current state of the shared Data Directory without delay. If this is not the case, it is likely to result in unstable behavior and frequent build log corruptions.

If TeamCity nodes run on Windows with Data Directory shared via SMB protocol, make sure all the registry keys mentioned in [this article](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-7/ff686200(v=ws.10)) are set to 0 on all the TeamCity nodes.

If the Data Directory is shared via NFS, make sure all nodes have the following option in their mount settings: `lookupcache=positive`.

### Main Node Caches Directory

If the main node accesses the Data Directory via a network location, it is highly recommended moving the `system/caches` directory to a [local disk](teamcity-data-directory.md#caches_folder).

### Node-specific Data Directory

Besides the Data Directory shared with the main server, a secondary node requires a _local_ Data Directory where it stores some caches, unpacked external plugins, and other configuration.

On the first start of the node, the local Data Directory is automatically created as `<TeamCity Data Directory>/nodes/<node_ID>`. This is usually the location of the shared Data Directory used by all nodes.

To reduce the load caused by extra IO requests from all nodes to the shared TeamCity Data Directory and to speed up the nodes\' access to data, we highly recommend redefining the location of the node-specific Data Directory to use the node\'s local disk.

To define a new path to a local directory, use the `-Dteamcity.node.data.path` property in the TeamCity [start-up scripts](configuring-teamcity-server-startup-properties.md#Standard+TeamCity+Startup+Scripts). Read more [below](#Installing+Secondary+Node).

## High-Availability Setup

### Prerequisites

>The instructions are provided for TeamCity 2021.1 or later. If you use an earlier version, please refer to its [respective documentation](https://confluence.jetbrains.com/display/TW/Documentation).
> 
{type="note"}

A basic HA setup must include the following components:
* __Dedicated database server__: an external database server from the list of [supported databases](supported-platforms-and-environments.md#Supported+Databases).
* __Dedicated server for the TeamCity data directory__: the [data directory](#Shared+Data+Directory) should be shared with nodes by network, via NFS or SMB.
* __At least two servers for TeamCity nodes__ with the mounted [shared data directory](#Shared+Data+Directory): both servers should have the same or comparable hardware. Otherwise, if a secondary node is less performant, you can experience a significant performance drop in case of a failover.
* __Local storage on the TeamCity nodes__: necessary for the TeamCity installation and storing logs and local caches. To estimate the size of caches, see the size of the`<TeamCity data directory>/system/caches` directory in your current  installation.
* __Dedicated reverse HTTP proxy server__.

The minimum number of server machines necessary for a high-availability setup is 5: a database, a network storage server, two TeamCity nodes, and one server for the reverse HTTP proxy. A simpler load-balancing solution might be achieved with fewer machines.

>All nodes that use the same data directory share the same license key. If you own a TeamCity Enterprise license, no additional purchases are required.
>
{type="note"}

### Configuring HA Setup

This sections relies on the following assumptions:
* All the [prerequisites](#Prerequisites) are met.
* [TeamCity is installed](installing-and-configuring-the-teamcity-server.md) on both nodes. (See how to [install a regular secondary node](#Installing+Secondary+Node).)
* [TeamCity Data Directory](teamcity-data-directory.md) and database are already initialized.

Now, we can proceed with the transition from a single server setup to a high-availability cluster setup.

Used terms:
* [TeamCity Data Directory](teamcity-data-directory.md) — a path to a mount point of the shared data directory.
* [TeamCity Home Directory](teamcity-home-directory.md) —  a path to the TeamCity server installation directory.
* [Node-specific Data Directory](#Node-specific+Data+Directory) —  a path to each node’s local storage where the node keeps its specific configuration and caches.
* `<node_ID>` — a unique identifier of a TeamCity node, which can be used in the UI and configuration files.
* `<node_root_URL>` — a direct URL of a node, usually `http://<node_hostname>`. This URL is required for inter-node communications; a firewall should be configured to allow connections to this URL from one node to another.

>Paths like `<TeamCity Data Directory>` or `<Node-specific data directory>` can be different on each node. To keep the setup simple, we recommend that you keep them the same.

To configure a TeamCity cluster consisting of two nodes, follow these steps:
1. Check that the version of the TeamCity server installed on both nodes is the same and corresponds to the version of the Data Directory.
2. Select an ID for each of the nodes: for example, a short ID based on a hostname.
3. Create the `TEAMCITY_SERVER_OPTS` environment variable on each node. This variable should have the following arguments:
    ```Shell
    -Dteamcity.server.nodeId=<node_ID> -Dteamcity.server.rootURL=<node_ID> -Dteamcity.data.path=<TeamCity Data Directory> -Dteamcity.node.data.path=<Node-specific Data Directory>
    ```
    >You can also specify a path to the shared TeamCity Data Directory via the TEAMCITY_DATA_PATH environment variable.  
    >Make sure the database is configured to accept enough parallel connections to handle requests from both nodes. By default, each node requires 50 connections to the database.
4. Start both nodes using [regular TeamCity scripts](installing-and-configuring-the-teamcity-server.md#Starting+TeamCity+server) or via the TeamCity service.
5. Open the TeamCity __Administration | Nodes Configuration__ page on any of the two servers and enable the "_[Main TeamCity node](#Main+Node+Responsibility)_" responsibility for a node you want to make main.
6. Proceed with [configuring the reverse HTTP proxy](#Proxy+Configuration).

## Proxy Configuration

The reverse HTTP proxy serves a single endpoint for TeamCity users and for [build agents](build-agent.md). This is also a good place to configure HTTPS connection settings for the entire cluster.

In case with TeamCity, the proxy server also acts as a load balancer for incoming requests. It can determine where the request should be sent and route it to the corresponding upstream server. In this article, we provide examples of the proxy configuration for two most popular proxy servers: NGINX and HAProxy.

<tabs>

<tab title="HAProxy">

```Plain Text
defaults
    mode http
    timeout connect 5s
    timeout client 10800s
    timeout server 10800s

frontend http-in
    bind *:80

    stats enable
    stats uri /healthz
    
    default_backend web_endpoint
    option httplog
    log /dev/log local0 info

    # Uncomment if logging to stdout is desired (e.g. when running in a containerized environment)
    #log stdout local0  info

    option http-buffer-request
    declare capture request len 40000000
    http-request capture req.body id 0
    capture request header user-agent len 150
    capture request header Host len 15

    capture cookie X-TeamCity-Node-Id-Cookie= len 100

    http-request add-header X-TeamCity-Proxy "type=haproxy; version=2021.1" 
    http-request set-header X-Forwarded-Host %[req.hdr(Host)]


    acl is_build_agent hdr_beg(User-Agent) -i "TeamCity Agent"

    use_backend agents_endpoint if is_build_agent
    use_backend web_endpoint unless is_build_agent

backend agents_endpoint
    acl cookie_found req.cook(X-TeamCity-Node-Id-Cookie) -m found
    use-server MAIN_NODE unless cookie_found
    cookie X-TeamCity-Node-Id-Cookie
    
    option httpchk GET /app/rest/version
    server MAIN_NODE {main_node_hostname} check inter 5s downinter 2s fall 3 rise 5 cookie {main_node_id}
    server SECONDARY_NODE {secondary_node_hostname} check inter 5s downinter 2s fall 3 rise 5 cookie {secondary_node_id}

backend web_endpoint
    balance first # choose first available server
    option httpchk GET /app/rest/version
    server WEB_MAIN_NODE {main_node_hostname} check inter 5s downinter 2s fall 3 rise 5
    server WEB_SECONDARY_NODE {secondary_node_hostname} check inter 5s downinter 2s fall 3 rise 5

```
</tab>

<tab title="NGINX">

```Plain Text
upstream {main_node_id} {
    server {main_node_hostname} max_fails=0;
    server {secondary_node_hostname} backup;
}
upstream {secondary_node_id} {
    server {secondary_node_hostname} max_fails=0;
    server {main_node_hostname} backup;
}

upstream web_requests {
    server {main_node_hostname} max_fails=0;
    server {secondary_node_hostname} backup;
}

map $http_cookie $backend_cookie {
    default "{main_node_id}";
    "~*X-TeamCity-Node-Id-Cookie=(?<node_name>[^;]+)" $node_name;
}

map $http_user_agent $is_agent {
    default @users;
    "~*TeamCity Agent*" @agents;
}
   
map $http_upgrade $connection_upgrade { # WebSocket support
   default upgrade;
   '' '';
}   

server {
  listen        80;
  server_name   {proxy server hostname};

  set $proxy_header_host $host; 

  location / {
            try_files /dev/null $is_agent;
  }
    
  location @agents {
     proxy_pass http://$backend_cookie;
     proxy_next_upstream error timeout http_503 non_idempotent;
     proxy_intercept_errors on;
     proxy_set_header Host $host:$server_port;
     proxy_redirect off;
     proxy_set_header X-TeamCity-Proxy "type=nginx; version=2021.1";
     proxy_set_header X-Forwarded-Host $http_host; # necessary for proper absolute redirects and TeamCity CSRF check
     proxy_set_header X-Forwarded-Proto $scheme;
     proxy_set_header X-Forwarded-For $remote_addr;
     proxy_set_header Upgrade $http_upgrade; # WebSocket support
     proxy_set_header Connection $connection_upgrade; # WebSocket support
  }

  location @users {
     proxy_pass http://web_requests;
     proxy_next_upstream error timeout http_503 non_idempotent;
     proxy_intercept_errors on;
     proxy_set_header Host $host:$server_port;
     proxy_redirect off;
     proxy_set_header X-TeamCity-Proxy "type=nginx; version=2021.1";
     proxy_set_header X-Forwarded-Host $http_host; # necessary for proper absolute redirects and TeamCity CSRF check
     proxy_set_header X-Forwarded-Proto $scheme;
     proxy_set_header X-Forwarded-For $remote_addr;
     proxy_set_header Upgrade $http_upgrade; # WebSocket support
     proxy_set_header Connection $connection_upgrade; # WebSocket support
  }
}

```
</tab>
</tabs>

In the examples above, the following values should be replaced:
* `{main_node_hostname}` — a hostname of the main node. If the UI of the main node operates on a port different from the proxy server port, this value should be specified in the form `hostname:port`.
* `{secondary_node_hostname}` — a hostname of the secondary node.
* `{main_node_id}` — the ID of the main node.
* `{secondary_node_id}` — the ID of the secondary node.

On a failover, if a former secondary node is assigned with the _Main TeamCity node_ responsibility, the configuration of the main and secondary nodes in the proxy config should be updated: `{main_node_hostname}` and `{main_node_id}` should be replaced with the hostname and ID of the new main node; `{secondary_node_hostname}` and `{secondary_node_id}` — with the hostname and ID of the former main node.

>Since all users will be using the URL of the proxy server for accessing the UI, this URL should be specified as a "Server URL" on the __Administration | Global Settings__ page.
> 
{type="note"}

The HAProxy config sets a special header `X-TeamCity-Proxy`. It tells TeamCity that a request comes through a properly configured proxy. The header also defines a version of the proxy config: it helps ensure that the TeamCity server is compatible with the proxy configuration.

## Failover

In the case of a failover, when the main node is no longer available either because of a crash or during maintenance, a secondary node can be granted with the "_Main TeamCity node_" responsibility and act as a main node temporarily or permanently.

Each secondary node tracks the activity of the current main node and, if it is inactive for several minutes (3 by default), shows the corresponding health report. After you see this report, you can assign the "_Main TeamCity Node_" responsibility to another node. However, if this inactivity has not been planned (that is, the main node has crashed), it is important to verify that no TeamCity server processes are left running on the inactive node. If you detect such processes, you need to stop them before reassigning the "_Main TeamCity node_" responsibility.

After switching the responsibility, you also need to update the IDs and hostnames of nodes in the [reverse proxy configuration](#Proxy+Configuration) and reload the proxy server configuration. Otherwise, if you decide to start the former main node again, the proxy won’t be able to properly route agents and users.

## Installing Secondary Node

To install a secondary node, follow these steps on the secondary node machine:

1. [Install](installing-and-configuring-the-teamcity-server.md) the TeamCity software as usual: download the distribution package and follow the installation wizard. Please note: when installing a secondary node using the installation wizard, it is important not to start the TeamCity Server service until the environment variables in step 2 and 3 are configured.
2. Provide the path to the [shared Data Directory](#Shared+Data+Directory) via the `TEAMCITY_DATA_PATH` [environment variable](configuring-teamcity-server-startup-properties.md#Standard+TeamCity+Startup+Scripts).
3. Add additional arguments to the `TEAMCITY_SERVER_OPTS` environment variable:
    ```Plain Text
    TEAMCITY_SERVER_OPTS = -Dteamcity.server.nodeId=<node_ID> -Dteamcity.node.data.path=<path_to_node_data_directory>  -Dteamcity.server.rootURL=<node_URL>
    
    ```
   where
    * `<node_ID>` is the ID of the node that will be displayed on the __Administration | Nodes Configuration__ page.
    * `<path_to_node_data_directory>` is the path to the node Data Directory (see [Node-Specific Data Directory](#Node-specific+Data+Directory)).
    * `<node_URL>` is the secondary node root URL. It should be accessible from the main server and agents.

## Nodes Configuration and Usage

### Authentication and License

The main and secondary nodes operate under the same license.

Secondary nodes use the same authentication settings as the main node. However, users might be asked to relog in after they are routed to a secondary node. This happens in two cases: (1) a user did not select the "Remember Me" option on the login screen and (2) [SSO authentication](configuring-authentication-settings.md) is not configured.

### Global Settings

Secondary notes use the same global settings (path to artifacts, version control settings, and so on) as the main node.

### Responsibilities

By default, a newly started secondary node provides a read-only user interface and does not perform any background activity. You can assign the following additional responsibilities to each secondary node in __Administration | Server Administration | Nodes Configuration__:

* [Processing data produced by builds](#Processing+Data+Produced+by+Builds+on+Secondary+Node)
* [VCS repositories polling](#VCS+Repositories+Polling+on+Secondary+Node)
* [Processing build triggers](#Processing+Triggers+on+Secondary+Node)
* [Processing user requests to modify data](#Processing+User+Requests+to+Modify+Data+on+Secondary+Node)
* [Main TeamCity node](#Main+Node+Responsibility)

A node assigned to any responsibility will allow users to perform the [most common actions](#User-level+Actions+on+Secondary+Node) on builds.

You can enable and disable responsibilities for nodes at any moment.

#### Processing Data Produced by Builds on Secondary Node

It is possible to use one or more secondary nodes to process traffic from the TeamCity agents. This allows moving the related load to a separate machine from the main TeamCity server which will improve the TeamCity performance when handling hundreds of concurrent and actively logging builds.

In general, you do not need a separate node for running builds unless you have more than 400 agents connected to a single server. Using a secondary node allows you to significantly increase the number of agents which the setup can handle.

Once you assign a secondary node to the _Processing data produced by builds_ responsibility for the first time, all\* newly started builds will be routed to this node. The existing running builds will continue being executed on the main server. When you disable the responsibility, only the newly started builds will be switched to the main server. The builds that were already running on the secondary node will continue running there.  
If you assign more than one secondary nodes to this responsibility, builds will be distributed equally between these nodes.

\* You can control how many builds can be run by each node.  
To do this, find the required node in the list of available nodes and click __Edit__  next to its _Processing data produced by running builds_ responsibility. The _Limit builds_ dialog will open. Here, you can enter a relative limit of builds allowed to run on this node. We suggest that you choose this limit depending on the node's hardware capabilities.  
If the maximum limit of allowed running builds is reached on all secondary nodes, TeamCity will be running new builds on the main node until some secondary node finishes its build.

#### VCS Repositories Polling on Secondary Node

Usually, the main TeamCity server polls the VCS repositories for changes to detect new commits. You can delegate VCS polling to the secondary node thus improving the performance of the main server. Only one secondary node can be assigned to this responsibility.

Once you assign the _VCS repositories polling_ responsibility to a node, it may take some time for the main server to finish the polling activities in progress, and then the secondary node will pick up this task. When you disable the responsibility, the main server will start polling VCS repositories.

If you have commit hooks configured on the main server, no changes in hooks are required: the hooks will continue working if the VCS polling is delegated to the secondary node.

#### Processing Triggers on Secondary Node

In setups with many build agents, a significant amount of the main server's CPU is allocated to constant processing of build triggers. By enabling the _Processing build trigger_ responsibility for one or more secondary nodes, you can distribute the trigger processing tasks and CPU load between the main node and the responsible secondary ones. TeamCity distributes the triggers automatically but you can see what triggers are currently assigned to each node.

#### Processing User Requests to Modify Data on Secondary Node

This responsibility is responsible for allowing [user actions on a secondary node](#User-level+Actions+on+Secondary+Node). It is especially useful when the main server is down or goes through maintenance.

#### Main Node Responsibility

You can assign a secondary node to the _Main TeamCity node_ responsibility. This responsibility by default belongs to the current main server, but gets vacant if this server becomes unavailable. After you assign any secondary server to this responsibility, it becomes the main node and receives all its other responsibilities (processing builds, managing agents, and so on). All the running builds will continue their operations without interruption. If a [proxy is configured](#Proxy+Configuration) in your setup, build agents will seamlessly reconnect to the new main node.  
When the previous main server starts again, it becomes a secondary node, as the _Main TeamCity node_ responsibility is already occupied by another server. If necessary, you can repeat the procedure above to switch roles between these servers.

### Internal Properties

All TeamCity nodes rely both on common internal properties, stored in the [shared data directory](#Shared+Data+Directory), and specific properties, stored in the node-specific data directory. The node-specific properties have a higher priority and overwrite the common values in case of a conflict.

To disable any common property on a given secondary node, pass it using the following syntax: `-<property_name>`.

### Secondary Node Memory Settings

The secondary node requires the same memory settings as the main node. If you have already configured the `TEAMCITY_SERVER_MEM_OPTS` [environment variable](configuring-teamcity-server-startup-properties.md) for the main node, make sure to use the same variable for the secondary node. If your main node uses a 64-bit JVM, the secondary node must use a 64-bit JVM as well.

### Project Import

You can import projects to the main node only. Secondary nodes will detect the imported data in the runtime, without restarting.

### Using Plugins

A secondary node has access to all plugins enabled on the main node. It also watches for the newly uploaded plugins. If a secondary node detects that a plugin has been uploaded to the main node, it will show the respective notification on its __Administration | Plugins__ page. If the plugin supports it, it can be reloaded in runtime with no need to restart the secondary node — the respective hint will be displayed in the UI.

A number of bundled plugins can be used on the main node only:
* Jira Cloud integration
* Kubernetes support
* Install Agent on a remote host (agent push)
* WebHooks support

>Secondary nodes can load an external/non-bundled plugin if this plugin supports secondary nodes. See the [Plugins F.A.Q.](https://plugins.jetbrains.com/docs/teamcity/plugin-development-faq.html#How+to+adapt+plugin+for+secondary+node) for details.
>
{type="note"}

### Clean-up

The TeamCity clean-up task runs on the main node only. In a multinode configuration, as well as in a single node configuration, the task can run while the secondary nodes are handling their operations.

### Upgrade/Downgrade

It is recommended that the main TeamCity server and all secondary nodes have the same version. In certain cases, the main server and secondary nodes can be running different versions for a short period, for example, during the minor upgrade of the main server. When the versions of a secondary node and the main server are different, the corresponding health report will be displayed on both nodes.

When __upgrading to a minor version__ (a bugfix release), the main and the secondary nodes should be running without issues as the TeamCity data format stays the same. You can upgrade the main TeamCity server and then the secondary servers [manually](upgrade.md), or with the [automatic update](upgrade.md#Automatic+Update).

When __upgrading the main server to a major version__, its TeamCity data format will change. We recommend stopping all the secondary nodes before starting the upgrade of the main server to avoid any possible data format errors.   
To be able to process tasks, all secondary nodes must be upgraded after the main server major upgrade.

To __upgrade__ nodes in a multinode setup to a major version of TeamCity, follow these steps:
1. Stop all secondary nodes.
2. Start the [upgrade](upgrade.md) on the main TeamCity server as usual.
3. Proceed with the upgrade.
4. Verify that everything works properly and agents are connecting to the main server (the agents will reroute the data that was supposed to be routed to the secondary nodes to the main server).
5. Upgrade TeamCity on the secondary nodes to the same version.
6. Start the secondary nodes and verify that they are connected on the __Administration | Server Administration | Nodes Configuration__ page on the main server.

To __downgrade__ nodes in a multinode setup, follow these steps:
1. Shutdown the main server and the secondary nodes.
2. [Restore the data](restoring-teamcity-data-from-backup.md) from backup (only if the data format has been changed during the upgrade).
3. Downgrade the TeamCity software on the main server.
4. Start the main TeamCity server and verify that everything works properly.
5. Downgrade the TeamCity software on the secondary nodes to the same version as the main server.
6. Start the secondary nodes.

TeamCity agents will perform upgrade/downgrade automatically.

### Start/Stop

Any TeamCity node can be started/stopped using regular TeamCity scripts (`teamcity-server.bat` or `teamcity-server.sh`) or Windows services. The environment variables [`TEAMCITY_DATA_PATH`], [`TEAMCITY_SERVER_MEM_OPTS`], and [`TEAMCITY_SERVER_OPTS`] are supported by all types of nodes.

All nodes use the same approach to logging events. You can check the state of the startup in the `<TeamCity Home Directory>/logs/teamcity-server.log` file. Or, you can open `<node root URL>` in your browser to see the TeamCity startup screens.

A secondary node, as well as the main node, can be stopped or restarted while the builds are running. They will continue running on agents and either will be reassigned to another node by the proxy, or will wait until their appointed node starts again.

### Backup/Restore

You can back up the main node right [from the TeamCity UI](creating-backup-from-teamcity-web-ui.md). [From a command line](creating-backup-via-maintaindb-command-line-tool.md), you can back up both the main node and secondary nodes.

The restore operation can be done on either of the nodes, but only if all servers using the TeamCity database and data directory are stopped.

>Currently, the contents of the `<Node-specific data directory>` are not included in the backup.
>
{type="note"}

#### User-level Actions on Secondary Node

If the "_[Processing user requests to modify data](#Processing+User+Requests+to+Modify+Data+on+Secondary+Node)_" responsibility is enabled on a secondary node, it will allow performing the most common user-level actions:
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
* Editing projects and build configurations

See the related tasks in our issue tracker for the full list of available actions: [TW-62749](https://youtrack.jetbrains.com/issue/TW-62749), [TW-63346](https://youtrack.jetbrains.com/issue/TW-63346).

Administrator-level actions are not yet available on secondary nodes. Use the main server if you need to change the server configuration.

## Multinode Setup Health Reports

See the related reports in the [dedicated article](server-health.md#Multinode+Setup+Misconfiguration).

## Common Issues

### Trouble Accessing Data Directory from Windows Service

Note that if TeamCity is running as a service on Windows, it might not be able to access the TeamCity Data Directory via a mapped network drive. This happens because Windows services cannot work with mapped network drives, and TeamCity does not support the UNC format (`\\host\directory`) for the Data Directory path. To work around this problem, you can use [`mklink`](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/mklink) as it allows making a symbolic link on a network drive:

```Console

mklink /d "C:\<path to mount point>" "\\<host>\<shared directory name>\"

```

Make sure remote-to-local symbolic link evaluations are enabled in your OS:

```Console

fsutil behavior query SymlinkEvaluation

```

To enable them, use the following command:

```Console

fsutil behavior set SymlinkEvaluation R2L:1

```