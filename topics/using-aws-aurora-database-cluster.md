[//]: # (title: Using AWS Aurora Database Cluster)
[//]: # (auxiliary-id: Using AWS Aurora Database Cluster)
This page provides details on using an [Amazon Aurora](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Aurora.Overview.html) cluster as the TeamCity database server.

<tag-list of="chapter" mode="tree" depth="4"/>

## Overview

When using an AWS Aurora cluster with TeamCity pointing to the [cluster end-point](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_Aurora.html#Aurora.Overview.Endpoints) as the database server, it is important to understand what happens when an AWS Aurora cluster fails over.

Both AWS Aurora DB instances are rebooted (so for a short period of time TeamCity entirely loses connection to the cluster) and
* the original DB instance is started with [innodb_read_only flag](https://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.html#sysvar_innodb_read_only) set (the new reader instance);
* the former failover instance is the new writer and the cluster endpoint DNS record is changed to point to the new writer instance.
By default, TeamCity JVM caches DNS name lookups, which essentially means that TeamCity will stay connected to the original DB instance until DNS cache expires. This in turn leads to the database connection pool on the TeamCity side to be populated with the connections to the new reader.

It will take some time for the JVM\-specific cache in TeamCity to expire and for the invalid connections to be evicted from the pool.

## General Recommendations 

When working with a failover cluster, it is recommended you decrease the JVM\-specific DNS caching on TeamCity [by setting the TTL to 60](http://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/java-dg-jvm-ttl.html):
1. Add the `-Dsun.net.inetaddr.ttl=60`JVM option to [the environment variable](configuring-teamcity-server-startup-properties.md#JVM+Options).

2. Restart TeamCity for the changes to take effect.

<note>

You may also wish to manually flush your OS\-specific DNS cache, but this is the action to be taken each time Aurora fails over.
</note>

## Forcing TeamCity to Connect to New Writer

You can force TeamCity to connect to the new writer either manually or automatically.

To force the connection __manually__ :
* reboot the new DB reader instance again: the reboot will take up to 2 minutes which is sufficient for the DNS cache to expire as well as for the invalid connections to be evicted from the pool
* alternatively, restart TeamCity manually and all the connections will be created anew.
 

To force TeamCity to __automatically__ connect to the new primary instance as soon as it's up and running:

<note>

The following options may affect your TeamCity server performance.
</note>

Configure the database connection pool to use a special validation query, so that the connections to the DB instance are tested before and/or after use and if a connection to the read\-only database is detected, they are evicted from the pool.
1. Add the following lines to the `<TeamCity Data Directory>/config/` [file](https://confluence.jetbrains.com/display/TCD10/Setting+up+an+External+Database#SettingupanExternalDatabase-DatabaseConfigurationProperties):


    ```Shell
    testOnBorrow=true
    testOnReturn=true
    testWhileIdle=true
    timeBetweenEvictionRunsMillis=60000
    validationQuery=select case when @@read_only + @@innodb_read_only \= 0 then 1 else (select table_name from information_schema.tables) end as `1`
    
    ```



2. Restart TeamCity. Once you do that, the following SQL query:


    ```Shell
    
    select case when @@read_only + @@innodb_read_only = 0 then 1 else (select table_name from information_schema.tables) end as `1`
    ```
will be executed for all connections whenever they are borrowed from or returned to the pool, and also every 1 minute (60000 milliseconds) for idle connections, raising [error 1242 (ER_SUBSELECT_NO_1_ROW )](https://dev.mysql.com/doc/refman/5.6/en/subquery-errors.html) for each connection to the read\-only database.
