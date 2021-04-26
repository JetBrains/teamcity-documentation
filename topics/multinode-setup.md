[//]: # (title: Multinode Setup)
[//]: # (auxiliary-id: Multinode Setup)

TeamCity allows configuring and starting one or more secondary server instances (nodes) in addition to the main one. The main and secondary nodes operate under the same license and share the TeamCity Data Directory and the database.

Using the multinode setup, you can:
* Set up a high-availability TeamCity installation that will have zero downtime for most operations. Secondary nodes will operate [as usual](#user-actions) during the downtime of the main server (for example, during the minor upgrades).
* Improve the performance of the main server by delegating tasks to the secondary nodes. A secondary node can detect new commits and process data produced by running builds (build logs, artifacts, statistic values).

<anchor name="user-actions"/>

After installation, each secondary node runs as a read-only copy of the main server. To extend its functionality, you can assign it to a certain [responsibility](configuring-secondary-node.md#Assigning+Responsibilities). In this case, the secondary node will allow users to perform the [most common actions](configuring-secondary-node.md#User-level+Actions+on+Secondary+Node) on builds.

The following diagram shows an example of a TeamCity installation with one main node and two secondary nodes, where each secondary node has a certain responsibility:

<img src="multinode-setup.png" width="702" alt="TeamCity setup with two nodes"/>


<anchor name="running-builds-node-discontinued"/>

<note>

__Since TeamCity 2019.2__, the [Running Builds Node](https://confluence.jetbrains.com/display/TCD18/Configuring+Running+Builds+Node) is discontinued in favor of extended functionality of the secondary node.   
If you used the Running Builds Node in your setup, note that it will not be able to start after upgrading to 2019.2. Please remove the `teamcity.server.role` parameter from its [startup properties](configuring-teamcity-server-startup-properties.md), so it starts as a regular secondary node. To continue processing traffic from agents on this node, assign the "[Processing data produced by running builds](configuring-secondary-node.md#Processing+Data+Produced+by+Builds+on+Secondary+Node)" responsibility to it.

Note that the secondary node offers more features than the Running Builds Node and thus might require as many hardware resources as a regular TeamCity server. Refer to [Estimate Hardware Requirements for TeamCity](how-to.md#Estimate+Hardware+Requirements+for+TeamCity) for notes on the recommended hardware.

</note>

## Prerequisites for Multinode Setup

This section describes configuration requirements for setting up multiple TeamCity nodes.

<tip>

Before switching to the multinode setup, we recommend that your read how to [configure the TeamCity server for better performance](how-to.md#Configuring+TeamCity+Server+for+Performance).

</tip>


### Shared Data Directory

The main TeamCity server and secondary nodes require access to the same [TeamCity Data Directory](teamcity-data-directory.md), which must be shared, and to the same external database.

For a high availability setup, we recommend storing the TeamCity Data Directory on a separate machine. In this case, even if the main server goes down, the secondary nodes will be able to connect to the shared Data Directory.

Ensure that all machines where TeamCity server nodes will be installed can access the Data Directory in the read\/write mode. Using a dedicated network file storage with good performance is recommended.

The typical Data Directory mounting options are SMB and NFS. TeamCity uses the Data Directory as a regular file system so all basic file system operations should be supported. The backing storage and way of mounting should not impose strict I\/O operations count or I\/O volume limits.

We recommend tuning storage and network performance: make sure to review performance guidelines for your storage solution. For example, increasing MTU for the network connection between the server and the storage usually increases the artifacts transferring speed.

Note that on Windows, a node might not be able to access the TeamCity Data Directory via a mapped network drive if TeamCity is running as a service. This happens because Windows services cannot work with mapped network drives, and TeamCity does not support the UNC format (`\\host\directory`) for the Data Directory path. To workaround this problem, you can use [`mklink`](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/mklink) as it allows making a symbolic link on a network drive:

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

<anchor name="MultinodeSetup-MultinodeSetupDisabling-networkclientcachesonDataDirectorymounts"/>

#### Disabling network client caches on Data Directory mounts

It is important that all the nodes "see" the current state of the shared Data Directory without delay. If this is not the case, it is likely to manifest in various unstable behavior and frequent build logs corruption.

If TeamCity nodes run on Windows with Data Directory shared via SMB protocol, make sure that all the registry keys mentioned in the [related article](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-7/ff686200(v=ws.10)) are set to 0 on all the TeamCity nodes.

If Data Directory is shared via NFS, make sure all nodes have the following option in their mount settings: `lookupcache=positive`.

#### Main Node Caches Directory

If main node accesses the data directory via a network location than it is highly recommended moving `system/caches` directory to a [local disk](teamcity-data-directory.md#caches_folder).

#### Node-Specific Data Directory

Besides the Data Directory shared with the main server, a secondary node requires a _local_ Data Directory where it stores some caches, unpacked external plugins, and other configuration.

On the first start of the node, the local Data Directory is automatically created as `<TeamCity Data Directory>/nodes/<node_ID>`. This is usually the location of the shared Data Directory, the directory used by all nodes.

To reduce the load caused by extra IO requests from all nodes to the shared TeamCity Data Directory and to speed up the nodes\' access to data, we highly recommend redefining the location of the node-specific Data Directory to use the node\'s local disk.

To define a new path to a local directory, use the `-Dteamcity.node.data.path` property in the TeamCity [start-up scripts](configuring-teamcity-server-startup-properties.md#Standard+TeamCity+Startup+Scripts). Read more in [Configuring Secondary Node](configuring-secondary-node.md#Installing+Secondary+Node).

### Proxy Configuration
{id="ProxyConfiguration" auxiliary-id="ProxyConfiguration"}

To set up a high-availability TeamCity installation, you need to install both the main server and the secondary node behind a reverse proxy and configure it to route requests to the main server while it is available and to the secondary one in other cases. If you are about to set up the TeamCity server behind a reverse proxy for the first time, make sure to review our [notes](how-to.md#Proxy+Server+Setup) on this topic.

#### Default Configuration

The following [NGINX](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-health-check) configuration will route requests to the secondary node only when the main server is unavailable or when the main server responds with the 503 status code (when starting or upgrading).

```Plain Text
http {
    upstream backend {
        server teamcity-main.local:8111 max_fails=0; # full internal address of the main server;
        server teamcity-ro.local:8111 backup; # full internal address of the secondary node;
    }
  
    server {
        location / {
            proxy_pass      	http://backend;
            proxy_next_upstream error timeout http_503 non_idempotent;
         }
    }
}

```

Note that [NGINX Open Source](http://nginx.org/en/) does not support active [health checks](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-health-check/) (which is a better way to configure High Availability installation) and may experience DNS resolution issues. Consider using [NGINX Plus](https://www.nginx.com/products/nginx/) or [HAProxy](http://www.haproxy.org/).

The HAProxy configuration example:

```Plain Text
resolvers dns
   nameserver dns %IP%:%PORT%
   resolve_retries       3
   timeout resolve   	1s
   timeout retry     	1s
defaults
   mode http
frontend http-in
   bind *:80
   default_backend tc-ha
backend tc-ha
   option httpchk GET /login.html
   default-server inter 2s fall 2 rise 2
   server tc_main_node %TC_MAIN_NODE_URL:8111% check resolvers dns
   server tc_ro_node %TC_RO_NODE_URL:8111% backup check resolvers dns

```

>After configuring the proxy, remember to update the _Server URL_ on the __Administration | Global Settings__ page.

#### Proxy as Load Balancer

>This functionality is provided in terms of TeamCity 2021.1 EAP.

In addition to its default role, your proxy can serve as a load balancer and manage communication between TeamCity agents and secondary nodes. By default, an agent sends all its requests to the main node first, and the main node redirects these requests to a suitable secondary node. If the main node becomes unavailable, the agent will not be able to communicate with its appointed secondary node, until the main node becomes available again.   
With the load balancer approach, you can make this communication less dependent on the main server by always routing agents to the proxy instead. The proxy will route each newly started agent to the main node first. If the main node assigns this agent to a secondary node, the proxy will route all the following agent's requests to this node, independently of the main node's status. Moreover, agents don't need to be able to connect to the TeamCity nodes directly, which allows for more convenient configuration of your setup. For example, you can configure HTTPS only on the proxy level instead of setting it up on each secondary node.

This approach optimizes the communication between agents and nodes which helps establish a high-availability setup. To use it instead of the default one, you need to configure the proxy to route the agent traffic to secondary nodes based on a special HTTP header and cookie.

The following HAProxy example shows what parameters are required to provide in the proxy configuration.

>This example can be used for a reference only and not intended for production purposes.
>
{type="warning"}

```Plain Text

defaults
    mode http
    timeout connect 60s
    timeout client 60s
    timeout server 60s

    frontend http-in
        bind *:80
        default_backend web_endpoint
        option httplog
        log stdout local0  info

        option http-buffer-request
        declare capture request len 40000000
        http-request capture req.body id 0
        capture request header user-agent len 150
        capture request header Host len 15
        
        # specify the cookie:
        capture cookie X-TeamCity-Node-Id-Cookie= len 100

        # specify the package header:
        http-request add-header X-TeamCity-Proxy "type=haproxy; version=2021.1"

        acl is_build_agent hdr_beg(User-Agent) -i "TeamCity Agent"

        # for web users' requests use the balanced endpoint:
        use_backend agents_endpoint if is_build_agent
        use_backend web_endpoint unless is_build_agent


    backend agents_endpoint
        acl cookie_found req.cook(X-TeamCity-Node-Id-Cookie) -m found
        # pass requests without the cookie to the main node
        # these are the commands and builds without the cookie:
        use-server MAIN_SERVER unless cookie_found
        # parses the X-TeamCity-Node-Id-Cookie cookie from the request. Does not add if absent.
        cookie X-TeamCity-Node-Id-Cookie
        # last argument is the cookie value that is the secondary node ID:
        server secondary_node <sec-host>:<sec-port> check cookie SecNodeID
        server MAIN_SERVER <main-host>:<port> check cookie MAIN_SERVER


    backend web_endpoint
        # choose the first available server:
        balance first
        option httpchk GET /login.html HTTP/1.1 # \r\nHost:localhost
        # set the main node as the default server:
        server web_main <sec-host>:<sec-port> check
        # set the secondary node as backup if the main node is down:
        server web_secondary <main-host>:<main-port> check
```

Here, `MAIN_SERVER` is the identifier of the main node; `secNodeID` must be set to the ID of a secondary node, as specified in __Administration | Nodes Configuration__. You can add as many `server secondary_node ...` lines as there are secondary nodes in your setup.

After configuring the proxy, remember to change the `serverURL` value to the proxy address in the agent's [`buildAgent.properties`](build-agent-configuration.md) file.

>If you leave `serverURL` set to the main server URL, the agent will connect to the main node for every operation, as in the default scenario. This way, you can combine two approaches and control which agents connect to the proxy, and which ones — directly to the main server.

#### Matching Proxy Version with Server
{id="MatchingProxyVersionwithServer" auxiliary-id="MatchingProxyVersionwithServer"}

When configuring a proxy as a [load balancer](#Proxy+as+Load+Balancer), you can optionally specify a minimal supported TeamCity version as the `version` parameter of the proxy package header. We always declare it in our configuration examples, as it helps ensure that each example template is compatible with the specified server version. Once we extend the proxying functionality in TeamCity, we will respectively update the proxy configuration templates and increase the recommended TeamCity version declared in them.

If TeamCity detects that the proxy version is later than the current version of the TeamCity server, it will display a related health report. In this case, the proxy configuration should be downgraded. To prevent such cases, we suggest that you always upgrade your TeamCity server before upgrading the proxy configuration and never use configuration templates with the later version than the version of your server.

### Firewall Settings

Firewall settings should allow accessing secondary nodes from the agents and from the main TeamCity server (the main server also communicates with the nodes by HTTP).

## Upgrade & Downgrade

It is recommended that the main TeamCity server and all secondary nodes have the same version. In certain cases, the main server and the secondary nodes can be running different versions for a short period, for example, during the minor upgrade of the main server. When the versions of the secondary node and the main server are different, the corresponding health report will be displayed on both nodes.

When __upgrading to a minor version__ (a bugfix release), the main and the secondary nodes should be running without issues as the TeamCity data has the same format. You can upgrade the main TeamCity server and then the secondary servers [manually](upgrade.md), or using the [automatic update](upgrade.md#Automatic+Update).

When __upgrading the main server to a major version__, its TeamCity data format will change. We recommend stopping all the secondary nodes before starting the upgrade of the main server to avoid any possible data format errors.   
All secondary nodes must be upgraded after the main server major upgrade to be able to process tasks.

To __upgrade__ nodes in a multinode setup to a major version of TeamCity, follow these steps:
1. Stop all secondary nodes.
2. Start the [upgrade](upgrade.md) on the main TeamCity server as usual.
3. Proceed with the upgrade.
4. Verify that everything works properly and agents are connecting (the agents will reroute the data, that was supposed to be routed to the secondary nodes, to the main server).
5. Upgrade the TeamCity installations on the secondary nodes to the same version.
6. Upgrade the [proxy configuration](#MatchingProxyVersionwithServer), if necessary.
7. Start the secondary nodes and verify that they are connected on the __Administration | Server Administration | Nodes Configuration__ page on the main server.

To __downgrade__ nodes in a multinode setup, follow these steps:
1. Shutdown the main server and the secondary nodes.
2. [Restore the data](restoring-teamcity-data-from-backup.md) from backup (only if the data format has been changed during the upgrade).
3. Downgrade the [proxy configuration](#MatchingProxyVersionwithServer), if necessary.
4. Downgrade the TeamCity software on the main server.
5. Start the main TeamCity server and verify that everything works properly.
6. Downgrade the TeamCity software on the secondary nodes to the same version as the main server.
7. Start the secondary nodes.

TeamCity agents will perform upgrade/downgrade automatically.

## Secondary Nodes Limitations

A secondary server has a few limitations compared to the main server:

* A secondary node does not allow changing the server configuration and state. The nodes without responsibilities are served in the read-only mode; the nodes with responsibilities provide user-level actions. In both cases, not all administration pages and actions are available.
* Currently, only bundled plugins and a limited set of some other plugins can be loaded by a secondary server. Some functionality provided by external plugins can be missing. Read more in [Configuring Secondary Node](configuring-secondary-node.md#Using+Plugins).
* Users may need to relogin when they are routed to a secondary node if they did not select the _Remember Me_ option.