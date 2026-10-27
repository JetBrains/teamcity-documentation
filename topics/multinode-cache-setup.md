[//]: # (title: Set up Distributed Cache)
[//]: # (help-id: Set up Distributed Cache;Distributed Cache;Multinode Cache Setup)

<show-structure for="chapter" depth="2"/>

In a [multinode setup](multinode-setup.md), TeamCity nodes can share cached data through an external Redis-compatible server. With a distributed cache, a value that is expensive to compute or to read from the database is calculated once by one node and then reused by all other nodes. This lowers the load on the database and lets nodes learn about changed data faster.

Currently, a distributed cache is optional. Without it, each node maintains its own local caches.

<snippet id="2027-2-cache-required">

<warning>

Starting with version 2027.2, a distributed cache will be required for a [multinode setup](multinode-setup.md). If you run TeamCity with several nodes, we recommend [setting up the distributed cache](multinode-cache-setup.md) in advance to prepare for upgrading to TeamCity 2027.2.

</warning>

</snippet>

For the list of supported cache servers, see [](supported-platforms-and-environments.md#Distributed+Cache).

## How the Distributed Cache Works

Each TeamCity node uses two cache levels:

* a local in-memory cache on the node;
* a shared cache on the Redis server, available to all nodes.

When a node needs a value, it checks its local cache first, then the shared cache. Only if both miss does the node calculate the value or read it from the database, and then stores the result in the shared cache for the other nodes.

When cached data changes, the Redis server notifies all connected nodes, and each node drops its outdated local copy. Without a distributed cache, nodes learn about such changes by periodically polling the database, which takes longer. Note that invalidation is fast, but not instant: for a short time, different nodes may see slightly different cached values.

Key principles to keep in mind when planning the setup:

* __The database remains the source of truth.__ The cache only stores copies of data that TeamCity can always recalculate. Entries may be evicted at any moment, and losing the whole cache content does not lose any TeamCity data. Redis persistence (RDB or AOF) is therefore not required.
* __Cache errors do not break the server.__ If the Redis server becomes unavailable while TeamCity is running, nodes log the errors and fall back to calculating values themselves. The server keeps working, but with a higher load on the database and nodes.
* __Cached values are not encrypted.__ TeamCity stores them as JSON: small values as plain text, large values compressed. Compression is not a protection, as anyone with access to the Redis server can read both. TeamCity does not put secrets into the cache, but cached data can still contain information about your projects and builds. Restrict network access to the Redis server, enable authentication, and connect [over TLS](#TLS+Connection).

## Recommended Setup

* __Use a dedicated Redis server or cluster for each TeamCity installation.__ All TeamCity keys share the same prefix, so two installations connected to the same Redis server would overwrite each other's data.
* __Use a Redis cluster in production.__ A cluster keeps the cache available when one of its nodes fails. A standalone Redis server is suitable for evaluation and small installations.
* __Place the Redis server close to the TeamCity nodes__, in the same network and data center, as every cache miss on a node results in a network round trip to Redis.

## Configure the Cache Connection

The connection to the cache server is configured in the same way as the [database connection](set-up-external-database.md#Configure+the+Database+Connection): in the `database.properties` file or via environment variables. The same settings must be used on all nodes.

### Properties File

Add the following properties to the [`<TeamCity Data Directory>`](teamcity-data-directory.md)`/config/database.properties` file:

<table>
<tr>
<th>
Property
</th>
<th>
Description
</th>
</tr>

<tr>
<td>
<code>redisUrl</code>
</td>
<td>
Address of the Redis server in the <code>host:port</code> format. If the port is omitted, the default <code>6379</code> port is used. The format of the value defines the client type:
<ul>
<li>
A single <code>host:port</code> pair connects to a standalone server, for example: <code>redis.example.com:6379</code>
</li>
<li>
A comma-separated list of <code>host:port</code> pairs connects to a cluster, for example: <code>redis-1.example.com:6379,redis-2.example.com:6379,redis-3.example.com:6379</code>
</li>
<li>
A single <code>host:port</code> pair with the <code>cluster@</code> prefix also connects to a cluster. Use it when the cluster is available via a single address, for example: <code>cluster@redis-cluster.example.com:6379</code>
</li>
</ul>
An optional scheme at the start of the value defines whether the connection is encrypted:
<ul>
<li>
<code>rediss://</code> connects over TLS. See <a href="#TLS+Connection">TLS Connection</a>.
</li>
<li>
<code>redis://</code> or no scheme connects without encryption.
</li>
</ul>
The scheme goes before the <code>cluster@</code> prefix, for example: <code>rediss://cluster@redis-cluster.example.com:6379</code>
<p>
The value must not contain anything else. Specify credentials in the <code>redisProperties.user</code> and <code>redisProperties.password</code> properties: a URL with a <code>user:password@</code> part or a database index such as <code>/0</code> is rejected as a failed connection (see <a href="#Startup+Behavior">Startup Behavior</a>).
</p>
</td>
</tr>

<tr>
<td>
<code>redisProperties.user</code>
</td>
<td>
Username for connecting to the Redis server. Omit if no authentication is required.
</td>
</tr>

<tr>
<td>
<code>redisProperties.password</code>
</td>
<td>
Password for connecting to the Redis server. Omit if no authentication is required.
</td>
</tr>
</table>

For example:

```Text
redisUrl=rediss://cluster@redis-cluster.example.com:6379
redisProperties.user=teamcity
redisProperties.password=<password>
```

Restart all TeamCity nodes to apply the changes.

### Environment Variables

The cache connection settings can also be defined by setting environment variables in the environment of every TeamCity node. This is useful, for example, to avoid storing the Redis password in the `database.properties` file.

<table>
<tr>
<th>
Variable
</th>
<th>
Description
</th>
</tr>

<tr>
<td>
<code>TEAMCITY_REDIS_URL</code>
</td>
<td>
Address of the Redis server. Overrides <code>redisUrl</code>.
</td>
</tr>

<tr>
<td>
<code>TEAMCITY_REDIS_USER</code>
</td>
<td>
Username for connecting to the Redis server. Overrides <code>redisProperties.user</code>.
</td>
</tr>

<tr>
<td>
<code>TEAMCITY_REDIS_PASSWORD</code>
</td>
<td>
Password for connecting to the Redis server. Overrides <code>redisProperties.password</code>.
</td>
</tr>
</table>

### TLS Connection

We recommend encrypting the connection to the Redis server, especially if it carries the Redis password or crosses networks you do not fully control. To connect over TLS:

1. Enable TLS on the Redis server. On Amazon ElastiCache, this option is called _encryption in transit_.  
   For a self-hosted cluster, also set `cluster-announce-hostname` and `cluster-preferred-endpoint-type hostname` on every cluster node, so that the nodes announce host names matching their certificates instead of IP addresses. Amazon ElastiCache already does this.
2. Make sure the JVM running each TeamCity node trusts the Redis server certificate. TeamCity verifies the certificate against the default trust store of the JVM and checks that the host name of each Redis node matches its certificate. This check cannot be turned off.
   * Certificates issued by a well-known certificate authority are usually trusted by default.
   * For a self-signed certificate or a certificate issued by your organization's certificate authority, import the CA certificate into the JVM trust store on every node, as described in [](using-https-to-access-teamcity-server.md#Configuring+client+JVM+for+trusting+server+certificate). Certificates [uploaded to TeamCity](uploading-ssl-certificates.md) are not used for this connection.
3. Add the `rediss://` scheme to `redisUrl` (or `TEAMCITY_REDIS_URL`) on all nodes, for example: `rediss://redis.example.com:6379`.
4. Restart all TeamCity nodes.

If the certificate is not trusted or the host name does not match, the node cannot connect to the Redis server, and the [startup behavior](#Startup+Behavior) described below applies.

## Startup Behavior

If the cache connection is configured but the node cannot connect to the Redis server on startup, the node does not start. This prevents nodes from silently running without the shared cache because of a configuration mistake.

To let a node start without the cache in this case, set the `teamcity.redis.ignoreBootstrapError=true` [internal property](server-startup-properties.md#TeamCity+Internal+Properties). The node then runs with local caches only.

>This property does not cover a Redis server whose version is too old: such a server accepts the connection, but the node fails later during startup when it uploads its function library. Make sure the server version is [supported](supported-platforms-and-environments.md#Distributed+Cache).
>
{style="note"}

## Monitor the Cache

Cache statistics, such as hits, misses, errors, and load times, are reported as server [metrics](teamcity-monitoring-and-diagnostics.md#Metrics). A growing number of errors usually indicates connection problems with the Redis server.

<seealso>
        <category ref="installation">
            <a href="multinode-setup.md">Multinode Setup for High Availability</a>
            <a href="set-up-external-database.md">Set up External Database</a>
        </category>
        <category ref="admin-guide">
            <a href="teamcity-monitoring-and-diagnostics.md">TeamCity Monitoring and Diagnostics</a>
        </category>
</seealso>
