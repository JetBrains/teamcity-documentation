[//]: # (title: Setting up TeamCity with MS SQL Server)
[//]: # (auxiliary-id: Setting up TeamCity with MS SQL Server)

This article provides step-by-step instructions on integrating TeamCity with a MS SQL Server. For a quick reference, see [this section](set-up-external-database.md#Microsoft+SQL+Server).

## Prerequisites

* MS SQL server
* MS SQL Server Management Studio
* [Installed TeamCity Server](install-and-start-teamcity-server.md)

## Enable TCP/IP protocol for your SQL server 

Open SQL Server Configuration Manager and do the following:
1. Expand _SQL Server Network Configuration_, click _Protocols for MS SQL_.
2. In the right pane, right-click _TCP/IP_, then click _Enabled_ and select _Yes_. (MS SQL comes with TCP/IP disabled by default.) Click __OK__.![enableTCP.png](enableTCP.png)
3. You need to restart your MSSQL server for the changes to take effect: in the left pane, click _SQL Server Services_, in the right pane right-click _SQL Server_ and click __Restart__.
4. Check that SQL Server Browser is running.![SQLServerBrowser.png](SQLServerBrowser.png)

## Create New Database

In MS SQL Server Management Studio:
1. Connect to your database server, right-click the _Databases_ node in the Object Explorer, and select _New database_.
2. On the _General_ page, specify the database name ("TeamCity" on the screenshot below) and allocate sufficient [transaction log](https://msdn.microsoft.com/en-us/library/ms365418.aspx) space. The recommended minimum is 1 GB (1024 MB). The requirements vary depending on how intensively the server will be used.![createDB_new.png](createDB_new.png)
3. Specify the primary collation. Go to the _Options_ node in the left pane and select a collation on the right. We recommend a case-sensitive collation (with the collation name ending with `_CS_AS`) corresponding to your locale. Click __Ok__ to save the settings:![collation_new_new.png](collation_new_new.png)
4. Make sure the `no count` setting is disabled as follows: right-click the server instance in Object Explorer, go to _Properties | Connections_. In the _Default Connection Options_ frame, `no count` must be turned off. Save changes if any.![noCount.png](noCount.png)


## Set Up TeamCity Database User

SQL Server supports two ways of authentication: SQL Server authentication and Windows authentication mode.

* SQL Authentication requires specifying username and a password in the database settings. It is recommended to start with this authentication before you try to use Windows authentication.
* Windows authentication (MS SQL integrated security) allows the TeamCity server running under a specific Windows user connect to the SQL server as that user, without providing a username and a password. However, it requires [additional setup](#Additional+Settings+for+Windows+Authentication+%28MS+SQL+Integrated+Security%29)

### Create Dedicated User for TeamCity with SQL Server Authentication

1. Go to the _Security_ node, right-click _Logins_, select _New Login_, and in the _General_ window that opens provide the login name ("TeamCity" on the image below), select the SQL server authentication and provide the password for the user. Set the default database to TeamCity.![defaultDBTC.png](defaultDBTC.png)
2. Select _User Mapping_ in the left pane, in the upper right pane check the TeamCity database in the list and in the lower pane grant the user TeamCity database owner rights: check the `db_owner` box. Click __OK__.![dbowner_new1.png](dbowner_new1.png)

### Create SQL Database User with Windows Authentication

1. Go to the _Security_ node, right-click _Logins_, select _New Login_, and in the _General_ window that opens provide the login name, select Windows authentication, and click the search button.
2. In the _Select user or Group_ dialog, specify the user account which will be used to run TeamCity. Click __Check names__. Once the user is found, click __OK__.![6WinAuthNewSQLUser.png](6WinAuthNewSQLUser.png)
3. Grant this user the DB owner permissions: select _User Mapping_ in the left pane, in the upper right pane check the TeamCity database in the list and in the lower pane grant the user TeamCity database owner rights: check the `db_owner` box. Click __OK__.![7WinAuthNewSQLUser.png](7WinAuthNewSQLUser.png)

## Set Up JDBC Driver for SQL Server Database

1. Download a [Microsoft JDBC driver version 6.0 or later](https://docs.microsoft.com/en-us/sql/connect/jdbc/download-microsoft-jdbc-driver-for-sql-server) (pick `.exe` or `.tar.gz` depending on your TeamCity server platform) from the Microsoft Download Center.
2. Unpack the downloaded package into a temporary directory.
3. Copy the `sqljdbc42.jar` (or `mssql-jdbc-<version>.jre8.jar` in versions above 6.0) package from the just downloaded package into the [`<TeamCity Data Directory>`](teamcity-data-directory.md)`/lib/jdbc` directory.

<note>

Note that Microsoft JDBC driver v6.0\+ has compatibility issues with Microsoft SQL Server 2005. For MS SQL Server 2005, use [JDBC driver v4.x](https://docs.microsoft.com/en-us/sql/connect/jdbc/download-microsoft-jdbc-driver-for-sql-server) (`.exe` or `.tar.gz` depending on your TeamCity server platform).

</note>

<anchor name="integratedSecurityAuth"/>

### Additional Settings for Windows Authentication (MS SQL Integrated Security)
[//]: # (AltHead: integratedSecurityAuth)

For Windows authentication (MS SQL integrated security), in addition to the JDBC driver, it is necessary to install native driver library `sqljdbc_auth.dll` from the JDBC driver package. The required version of the library depends on the bitness of the Java version used by the server.

For the default 32-bit JVM bundled with the TeamCity server, copy the `<sql_jdbc_home>/enu/auth/x86/sqljdbc_auth.dll` file into [`<TeamCity Data Directory>`](teamcity-data-directory.md)`/lib/jdbc/native/windows-i386`.   
For the 64-bit JVM used to run the TeamCity server, use `<sql_jdbc_home>/enu/auth/x64/sqljdbc_auth.dll`, and place it into the [`<TeamCity Data Directory>`](teamcity-data-directory.md)`/lib/jdbc/native/windows-amd64` directory.

## Start TeamCity

1. Start the TeamCity server. For Windows authentication (MS SQL integrated security), make sure the server is running under the user [configured at this step](#Create+SQL+Database+User+with+Windows+Authentication).
2. Select MS SQL as the database.
3. Click the Refresh the JDBC drivers if asked.
4. Specify the connection settings:
    - Database host — your SQL server host.
    - Port — optional. By default, TeamCity will try to connect to the database on a default port (`1433`). If your database uses a different port (for example, it can be autoassigned by MS SQL Express or changed manually), remember to enter it in this field. You can check what port is used by your database in SQL Server Configuration Manager: __TCP/IP Properties | IP Addresses | TCP Dynamic Ports__.
    - Database instance name — leave blank for the default SQL server instance. If a named instance is used, provide its name here.
    - Database name — the name of the newly created database
5. Select the required authentication type, for SQL Server authentication provide the credentials of the dedicated SQL user [configured at this step](#Create+Dedicated+User+for+TeamCity+with+SQL+Server+Authentication).![DB_SQLAuth.png](DB_SQLAuth.png)
6. Continue with the setup. It'll take some time to initialize the database schema and the components.

>It is recommended monitoring the database performance regularly. For example, perform [reindexing](https://msdn.microsoft.com/en-us/library/ms189858.aspx#Fragmentation) every several months.
