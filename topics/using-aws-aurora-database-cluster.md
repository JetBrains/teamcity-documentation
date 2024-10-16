[//]: # (title: Using AWS Aurora Database Cluster)
[//]: # (auxiliary-id: Using AWS Aurora Database Cluster)

This page provides details on using an [Amazon Aurora](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Aurora.Overview.html) cluster as the TeamCity database server.

When using an AWS Aurora cluster with TeamCity pointing to the [cluster end-point](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_Aurora.html#Aurora.Overview.Endpoints) as the database server, it is important to understand what happens when an AWS Aurora cluster fails over.

Both AWS Aurora DB instances are rebooted (so for a short period of time TeamCity entirely loses connection to the cluster) and
* the original DB instance is started in read-only mode (the new reader instance);
* the former failover instance is the new writer, and the cluster endpoint DNS record is changed to point to the new writer instance.

By default, TeamCity JVM caches DNS name lookups, which essentially means that TeamCity will stay connected to the original DB instance until DNS cache expires. This in turn leads to the database connection pool on the TeamCity side to be populated with the connections to the new reader.

It will take some time for the JVM-specific cache in TeamCity to expire and for the invalid connections to be evicted from the pool.

## General Recommendations 

When working with a failover cluster, it is recommended to decrease the JVM-specific DNS caching on TeamCity [by setting the TTL to 60](https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/java-dg-jvm-ttl.html):
1. Add the `-Dsun.net.inetaddr.ttl=60` JVM option to the [environment variable](server-startup-properties.md#JVM+Options).
2. Restart TeamCity for the changes to take effect.

<note>

You may also wish to manually flush your OS-specific DNS cache, but this is the action to be taken each time Aurora fails over.
</note>

## Forcing TeamCity to Connect to New Writer

You can force TeamCity to connect to the new writer either manually or automatically.

To force the connection __manually__ :
* Reboot the new DB reader instance again: the reboot will take up to 2 minutes which is sufficient for the DNS cache to expire as well as for the invalid connections to be evicted from the pool.
* Alternatively, restart TeamCity manually and all the connections will be created anew.

To force TeamCity to __automatically__ connect to the new primary instance as soon as it's up and running:

<note>

The following options may affect your TeamCity server performance.
</note>

1. Configure the database connection pool to use a special validation query, so that the connections to the DB instance are tested before and/or after use and if a connection to the read-only database is detected, they are evicted from the pool.   
For this, add the following lines to the `<TeamCity Data Directory>/config/`[database.properties](set-up-external-database.md#Database+Configuration+Properties):   
For Aurora MySQL:
    ```Shell
    testOnBorrow=true
    testOnReturn=true
    testWhileIdle=true
    timeBetweenEvictionRunsMillis=60000
    validationQuery=select case when @@read_only + @@innodb_read_only \= 0 then 1 else (select table_name from information_schema.tables) end as `1`
    
    ```
   
    For Aurora PostgreSQL:
    ```Shell
    testOnBorrow=true
    testOnReturn=true
    testWhileIdle=true
    timeBetweenEvictionRunsMillis=60000
    validationQuery=select case when not pg_is_in_recovery() then 1 else random() / 0 end
    
    ```  
2. Restart the TeamCity server. Once you do that, the specified validation query will be executed for all connections whenever they are borrowed from or returned to the pool, and also every 1 minute (60000 milliseconds) for idle connections, raising an error for each connection to the read-only database.

## Using SSL Connection to Aurora MySQL Cluster

Since Amazon Aurora MySQL version 2 (compatible with MySQL 5.7 and higher), you can determine if TeamCity uses a secure SSL connection to your DB cluster by running the [respective query](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/ssl-certificate-rotation-aurora-mysql.html#ssl-certificate-rotation-aurora-mysql.determining-server). If necessary, you can change the [connection mode](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/AuroraMySQL.Security.html#AuroraMySQL.Security.SSL) on the Amazon server side.

If you are using Amazon Aurora MySQL version 1 (compatible with MySQL 5.6 and lower), you can check if the connection between TeamCity and the DB cluster is secure by inspecting its traffic with a packet analyzer like `tcpdump`.   
To change the mode of the SSL connection (for example, to enforce or disable it), pass the respective `sslMode` parameter to the MySQL Connector/J JDBC driver by adding it to the `<TeamCity data directory>/config/database.properties` file:
* Either add a separate line:   
    ``` 
    connectionProperties.sslMode=<value>
    ```
    or
* Append the `?sslMode=<value>` argument to the connection string. For example:   
    ```
    connectionUrl=jdbc:mysql://<hostname>:3306/<dbname>?sslMode=<value>
  ``` 
See the list of supported connection mode values in [Connector/J configuration properties](https://dev.mysql.com/doc/connector-j/8.0/en/connector-j-reference-configuration-properties.html).