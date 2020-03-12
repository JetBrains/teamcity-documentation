[//]: # (title: Multinode Setup)
[//]: # (auxiliary-id: Multinode Setup)

TeamCity allows configuring and starting one or more secondary server instances (nodes) in addition to the main one. The main and secondary nodes operate under the same license and share the TeamCity Data Directory and the database.

Using the multinode setup, you can:
* Set up a high-availability TeamCity installation that will have zero read downtime. A secondary node will allow users read operations during the downtime of the main server (for example, during the upgrade).
* Improve the performance of the main server by delegating tasks to the secondary nodes. A secondary node can detect new commits and process data produced by running builds (build logs, artifacts, statistic values).

<anchor name="user-actions"/>

After installation, each secondary node runs as a read-only copy of the main server. To extend its functionality, you can assign it to a certain [responsibility](configuring-secondary-node.md#Assigning+Responsibilities). In this case, the secondary node will allow users to perform the most common actions on builds:
* Triggering a build, including a custom or personal one
* Stopping/deleting and pinning/tagging/commenting builds
* Assigning investigations and muting build problems and tests
* Marking a build as successful/failed
* Merging sources and labeling sources actions
* and more (see the full list in our [issue tracker](https://youtrack.jetbrains.com/issue/TW-62749))

The following diagram shows an example of a TeamCity installation with one main node and two secondary nodes, where each secondary node has a certain responsibility:

<img src="multinode-setup.png" width="800" alt="TeamCity setup with two nodes"/>


<anchor name="running-builds-node-discontinued"/>

<note>

__Since TeamCity 2019.2__, the [Running Builds Node](https://confluence.jetbrains.com/display/TCD18/Configuring+Running+Builds+Node) is discontinued in favor of extended functionality of the secondary node.   
If you used the Running Builds Node in your setup, note that it will not be able to start after upgrading to 2019.2. Please remove the `teamcity.server.role` parameter from its [startup properties](configuring-teamcity-server-startup-properties.md), so it starts as a regular secondary node. To continue processing traffic from agents on this node, assign the "[Processing data produced by running builds](configuring-secondary-node.md#Processing+Data+Produced+by+Builds+on+Secondary+Node)" responsibility to it.

Note that the secondary node offers more features than the Running Builds Node and thus might require as many hardware resources as a regular TeamCity server. Refer to [Estimate Hardware Requirements for TeamCity](how-to.md#Estimate+Hardware+Requirements+for+TeamCity) for notes on the recommended hardware.

</note>

## Prerequisites for Multinode Setup

This section describes configuration requirements for setting up multiple TeamCity nodes.

### Shared Data Directory

The main TeamCity server and secondary nodes require access to the same [TeamCity Data Directory](teamcity-data-directory.md) (which must be shared) and to the same database.

For a high availability setup, we recommend storing the TeamCity Data Directory on a separate machine. In this case, even if the main server goes down, the secondary nodes will be able to connect to the shared Data Directory.

Ensure that all machines where TeamCity server nodes will be installed can access the Data Directory in the read\/write mode. Using a dedicated network file storage with good performance is recommended.

The typical Data Directory mounting options are SMB and NFS. TeamCity uses the Data Directory as a regular file system so all basic file system operations should be supported. The backing storage and way of mounting should not impose strict I\/O operations count or I\/O volume limits.

We recommend tuning storage and network performance: make sure to review performance guidelines for your storage solution. For example, increasing MTU for the network connection between the server and the storage usually increases the artifacts transferring speed.

Note that on Windows, a node might not be able to access the TeamCity Data Directory via a mapped network drive if TeamCity is running as a service. This happens because Windows services cannot work with mapped network drives, and TeamCity does not support the UNC format (`\\host\directory`) for the Data Directory path. To workaround this problem, you can use `mklink` as it allows making a symbolic link on a network drive:

```Plain Text
mklink /d "C:\<path to mount point>" "\\<host>\<shared directory name>\"
```


<note>
__Important!__

It is important that all the nodes "see" the current state of the Data Directory without delay. If this is not the case, it is likely to manifest in various unstable behavior and frequent build logs corruption.
For example if TeamCity nodes run on Windows with Data Directory shared via SMB protocol, make sure that all the registry keys mentioned in the [related article](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-7/ff686200(v=ws.10)) are set to 0 on all of the TeamCity nodes.
</note>


#### Node-Specific Data Directory

Besides the Data Directory shared with the main server, a secondary node requires a _local_ Data Directory where it stores some caches, unpacked external plugins, and other configuration.

On the first start of the node, the local Data Directory is automatically created as `<TeamCity Data Directory>/nodes/<node_ID>`. This is usually the location of the shared Data Directory, the directory used by all nodes.

To reduce the load caused by extra IO requests from all nodes to the shared TeamCity Data Directory and to speed up the nodes' access to data, we highly recommend redefining the location of the node-specific Data Directory to use the node\'s local disk.

To define a new path to a local directory, use the `-Dteamcity.node.data.path` property in the TeamCity [start-up scripts](configuring-teamcity-server-startup-properties.md#Standard+TeamCity+Startup+Scripts). Read more in [Configuring Secondary Node](configuring-secondary-node.md#Installing+Secondary+Node).

### Proxy Configuration

To set up a high-availability TeamCity installation, you need to install both the main server and the secondary node behind a reverse proxy and configure it to route requests to the main server while it is available and to the secondary one in other cases. If you are about to set up the TeamCity server behind a reverse proxy for the first time, make sure to review our [notes](how-to.md#Proxy+Server+Setup) on this topic.

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

Note that [NGINX Open Source](http://nginx.org/en/) does not support active [health checks](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-health-check/) (which is a better way to configure High Availability installation) and may experience DNS resolution issues. Consider using [NGINX Plus](https://www.nginx.com/products/nginx/) or [HA Proxy](http://www.haproxy.org/).

The HA Proxy configuration example:

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

### Firewall Settings

Firewall settings should allow accessing secondary nodes from the agents and from the main TeamCity server (the main server also communicates with the nodes by HTTP).

## Upgrade & Downgrade

It is recommended that the main TeamCity server and all secondary nodes have the same version. However, the main server and the secondary nodes can be running different versions, for example, when the main server is being upgraded. When the versions of the secondary node and the main server are different, the corresponding health report will be displayed on both nodes.

When __upgrading to a minor version__ (a bugfix release), the main and the secondary nodes should be running without issues as the TeamCity data has the same format.

When __upgrading__ the main server __to the major version__, its TeamCity data format will change. The data will be upgraded while the secondary nodes are running provided they are not performing any write operations. If writing operations are detected on a secondary node, a warning will be displayed, and the main server upgrade will be suspended until the secondary node is stopped.

After the main server is upgraded to a new major version, the secondary nodes will detect that the data has been upgraded, show health reports, and stop processing tasks according to their responsibilities. All secondary nodes must be upgraded after the main server upgrade to be able to process tasks.

To upgrade nodes in a multinode setup, follow these steps:

1. Stop all secondary nodes (if you forget to do that, you will be warned during the upgrade).
2. Start the [upgrade](upgrade.md) on the main TeamCity server as usual.
3. Proceed with the upgrade.
4. Check that everything works properly and agents are connecting (the agents will reroute the data, that was supposed to be routed to the secondary nodes, to the main server).
5. Upgrade the TeamCity installation on the secondary nodes to the same version.
6. Start the secondary nodes and check that they are connected on the __Administration | Server Administration | Nodes Configuration__ page on the main server.

      
To __downgrade__ nodes in a multinode setup, follow these steps:

1. Shutdown the main server and the secondary nodes.
2. [Restore the data](restoring-teamcity-data-from-backup.md) from backup (only if the data format has been changed during the upgrade).
3. Downgrade the TeamCity software on the main server.
4. Start the main TeamCity server and check that everything works properly.
5. Downgrade the TeamCity software on the secondary nodes to the same version as the main server.
6. Start the secondary nodes. 

TeamCity agents will perform upgrade\/downgrade automatically.

## Secondary Nodes Limitations

A secondary server has a few limitations compared to the main server:

* A secondary node does not allow changing the server configuration and state. The nodes without responsibilities are served in the read-only mode; the nodes with responsibilities provide user-level actions. In both cases, not all administration pages and actions are available.
* Currently, only bundled plugins and a limited set of some other plugins can be loaded by a secondary server. Some functionality provided by external plugins can be missing. Read more in [Configuring Secondary Node](configuring-secondary-node.md#Using+Plugins).
* Users may need to relogin when they are routed to a secondary node if they did not select the _Remember Me_ option.

__ __
