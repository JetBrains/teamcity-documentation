[//]: # (title: Upgrading TeamCity Server and Agents)
[//]: # (auxiliary-id: Upgrading TeamCity Server and Agents;Upgrade)

Your TeamCity Cloud instance is kept to date automatically. We upgrade all Cloud instances during the week following each major and minor release. The upgrade occurs in the least loaded server time. The duration of upgrade depends on the size of your instance.
{product="tcc"}

Refer to [TeamCity Release Cycle](teamcity-release-cycle.md) for information on expected release updates.
{product="tcc"}

>Unless specifically noted, TeamCity does not support downgrade between major releases (changes in the first two numbers of the version). It is strongly recommended [backing up your data](teamcity-data-backup.md) before any upgrade.
>
{type="note" product="tc"}

TeamCity supports upgrades from any of the previous versions to the later ones. All the settings and data are preserved unless noted in the [Upgrade Notes](upgrade-notes.md).
{product="tc"}

It is recommended to plan for regular upgrades to run the latest TeamCity version at least after several bugfix updates are released. This way, you run a fully [supported version](teamcity-release-cycle.md) with the latest fixes and security patches.
{product="tc"}

## Security Patches

TeamCity is a web application that heavily relies on communications among the server, build agents, VCS providers, and other external services (such as [AWS S3](storing-build-artifacts-in-amazon-s3.md) buckets or [Nexus LFS](git.md#LFS+and+Submodules+Support) repositories). As such, when deployed in non-closed environments, TeamCity is vulnerable to attacks exploiting common vulnerabilities and exposures (CVEs), including those inherited from third-party solutions and libraries it employs.

To reduce the risks, it is highly advised to promptly install any updates containing security fixes. Fixes that eliminate non-critical security vulnerabilities are addressed in regular [bug-fix updates](teamcity-release-cycle.md). Refer to the [Release Notes](what-s-new-in-teamcity.md#Fixed+Issues) and check out our [Security Bulletin](https://www.jetbrains.com/privacy-security/issues-fixed/?product=TeamCity) for the list of fixed issues.

Critical vulnerabilities that present major security threats are fixed in separate unscheduled security patches. Once a solution is found, these patches are released immediately. If such an update is available, TeamCity automatically downloads it and prompts administrators for installation.

> TeamCity notifies users about critical security patches via emails and health reports. These notifications are visible to users with the "Manage server installation: view logs, restart, etc." [permission](managing-roles-and-permissions.md). By default, this permission is included only in the "System administrator" role.
> 
> Make sure that users who should receive email notifications about available security patches have email addresses specified in their [user profiles](configuring-your-user-profile.md) and [email notifications are enabled](configuring-notifications.md).
> 
{style="tip"}

To stop TeamCity from automatically downloading these patches, uncheck the "Automatically download available security patches and notify administrators when they are ready to be installed" option on the **Administration | Updates** page. However, we strongly advise against disabling this feature, as it guarantees rapid responses to emerging security threats and ensures swift updates.

## Before Upgrade
{product="tc"}

Before upgrading TeamCity:

* For a major upgrade, review what you will be getting in [What's New](what-s-new-in-teamcity.md).
* [Check your license keys](#Licensing) unless you are upgrading within bugfix releases of the same major `YYYY.MM` version.
* [Download](https://www.jetbrains.com/teamcity/download/) the new TeamCity version (see [links to all released versions](previous-releases-downloads.md)).
* Carefully review the __[Upgrade Notes](upgrade-notes.md)__.
* Consider probing the upgrade on a [test server](how-to.md#Test-drive+Newer+TeamCity+Version+before+Upgrade).
* If you have non-bundled plugins installed, check plugin pages for compatibility with the new version and upgrade/uninstall the plugins if necessary.
* Make sure that TeamCity is running under a user account that has read and write permissions for both the installation and [TeamCity data](teamcity-data-directory.md) directories.

To upgrade the server:
1. [Back up the current TeamCity data](teamcity-data-backup.md) including settings, database, and supplementary data. You will need the backup to roll back to the previous version in the unlikely event of the upgrade failure.
2. [Perform the upgrade steps](#Upgrading+TeamCity+Server):
    * [Upgrading Using Windows Installer](#Using+Windows+Installer)
    * [Manual Upgrading on Linux](#Using+.tar.gz+Distributions)

### Licensing

Before upgrading, make sure the maintenance period of your licenses is not yet elapsed (use the __Administration | Licenses__ page to see your license keys). The licenses are valid only for the versions of TeamCity with the effective release date within the maintenance period. Check the effective release date on the [release list](previous-releases-downloads.md).   
Typically all the minor updates (indicated by changes in the `B` part of the `YYYY.MM.B` TeamCity version) use the same effective release date (that of the major release).   
If not all the licenses cover the target version release date, consider [renewing the licenses](https://www.jetbrains.com/teamcity/buy/#license-type=renewal) before the upgrade (you can replace the old license keys with the renewed ones even before the upgrade).

If you are only evaluating a newer version, you can get a trial license on the [download page](https://www.jetbrains.com/teamcity/download/). Note that each TeamCity version can be evaluated only once. To extend the trial period, [contact](https://www.jetbrains.com/company/contacts/#contactSales) the JetBrains sales department.

When upgrading from TeamCity 4.x or earlier, note that the licensing policy in TeamCity versions 5.0 and later are different from that of the previous TeamCity versions. Review the [Licensing Policy](licensing-policy.md) page and the [Licensing and Upgrade](https://www.jetbrains.com/teamcity/buy#upgradeuser) section on the official site.

<anchor name="Upgrade-UpgradingTeamCityServer"/>

## Upgrading TeamCity Server
{product="tc"}

TeamCity supports upgrades from any of the previous versions to the current one.   
Unless specifically noted, downgrades with preserving the data are not possible with changing the major version and are possible within bugfix releases.

The general policy is that bugfix updates (indicated by changes in the `B` part of the `YYYY.MM.B` TeamCity version) do not change data format, so you can freely upgrade/downgrade within the bugfix versions. However, when upgrading to the next major version (changed `YYYY.N`), you will not be able to downgrade with the data preservation: you will need to [restore a backup](restoring-teamcity-data-from-backup.md) of the appropriate version. [Read more](teamcity-release-cycle.md#Version+Numbers) about the release numbering.

On upgrade, all the TeamCity configuration settings and other data are preserved unless noted in [Upgrade Notes](upgrade-notes.md). If you have customized TeamCity installation (like Tomcat server settings change), you will need to repeat the customization after the upgrade.

The general approach to upgrade is to remove all the files of the previous installation in the TeamCity Server Home and place the new files into the same location. Make sure to preserve the [TeamCity Data Directory](teamcity-data-directory.md) and the database intact (making a backup beforehand), backing up and restoring previously customized settings (for example, in `...\conf\server.xml`, `...\conf\web.xml` files) is also necessary. The logs directory (`...\logs`) can be left with the old installation files.

Agents connected to the server are upgraded [automatically](#Automatic+Build+Agents+Update).

>__Important note on TeamCity data structure upgrade__  
>TeamCity server stores its data in the database and in [TeamCity Data Directory](teamcity-data-directory.md) on the file system. Different TeamCity versions use different data structure of the database and Data Directory. Upon starting a newer version of TeamCity, the data is kept in the old format until you confirm the upgrade and data conversion on the Maintenance page in the web UI. Until you do so, you can back up the old data; however, once the upgrade is complete, the data is converted to the new format.  
>__Once the data is converted, downgrade to the previous TeamCity versions which uses different data format is not possible!__
{style="warning"}

There are several important issues with data format upgrade:

* Data structure downgrade is not possible. Once newer TeamCity version changes the data format of database and Data Directory, you cannot use this data to run an older TeamCity version. Please ensure you [backup](teamcity-data-backup.md) the data before upgrading TeamCity.
* Both the database and the Data Directory should be upgraded simultaneously. Ensure that during the first start of the newer server it uses the correct [TeamCity Data Directory](teamcity-data-directory.md) that in its turn has the correct database [configured](set-up-external-database.md) in the `<[TeamCity Data Directory](teamcity-data-directory.md)>\config\database.properties` file. Also make sure the Data Directory is complete (for example, all the build logs and artifacts are in place), no Data Directory content supports copying from the Data Directory of the older server versions.

If you accidentally performed an inconsistent upgrade, check the [recovery instructions](how-to.md#Recover+from+%22Data+format+of+the+Data+Directory+%28NNN%29+and+the+database+%28MMM%29+do+not+match%22+error).

<anchor name="Upgrade-AutomaticUpdate"/>

### Automatic Update

> Automatic update is available only for `.tar.gz` and `.exe` installations.
> In particular, automatic update cannot be used with a Docker installation (see [](#manual-update-of-docker-image)) and the [AWS CloudFormation template](https://github.com/JetBrains/teamcity-cloudformation-template#readme) is no longer supported.
{style="note"}

To be able to update automatically, the TeamCity server should be able to contact [jetbrains.com](https://www.jetbrains.com/), `download.jetbrains.com` and `download-cdn.jetbrains.com`.

When a new version of TeamCity is detected, the server displays the corresponding health item for system administrators. The item points to the server's __Administration | Updates__ page, where all the versions available for the update are listed. The page contains notes about licenses compatibility, the new version description, and controls to perform the automatic update if you want to use that instead of performing the manual updating procedure.

The automatic update procedure is as follows:
1. The TeamCity server is stopped.
2. The update script is run to do the following:
    1. Create a backup of the current installation in the `<[TeamCity Home Directory](teamcity-home-directory.md)>/.old` directory.
    2. Update the stopped server to the new version.
3. Next, the updated server starts.  
   The update progress is logged to the `<[TeamCity Home Directory](teamcity-home-directory.md)>/logs/teamcity-update.log` file.

In case of an automatic update failure, perform the following to restore your TeamCity to the state prior to the update:

1. Stop your TeamCity server if it is running.
2. For each folder that exists in the `<[TeamCity Home Directory](teamcity-home-directory.md)>/.old` directory, remove the corresponding folder in the `<[TeamCity Home Directory](teamcity-home-directory.md)>`. Do not remove the "bin" folder as some of its files are not backed up to the `TC_home/.old` directory.
3. Copy everything from the `<[TeamCity Home Directory](teamcity-home-directory.md)>/.old` folder and paste it to `<TeamCity Home Directory>`, replacing all duplicate files.
4. Start the TeamCity server.

>Note that the bundled version of Java is not updated automatically. Learn [how to install the required Java version manually](how-to.md#Install+Non-Bundled+Version+of+Java).  
>If switching from 32- to 64-bit Java, you will also need to adjust the heap space (`Xmx`) value as described [here](configure-server-installation.md#Configure+Memory+Settings+for+TeamCity+Server).
>
> See also: [supported Java versions for TeamCity Server](supported-platforms-and-environments.md#Supported+Java+Versions+for+TeamCity+Server).
> 
{style="warning"}

Other limitations of the automatic update:
* Some files like `TeamCityService.exe` and `teamcity-server.bat` are not included into the scope of the autoupdate.
* Some customizations, for example, installations with [changed server context](configure-server-installation.md#Changing+Server+Context), are not supported by automatic update.
* The Windows uninstaller is not updated during the upgrade, so after several updates, old TeamCity version will still be noted in Windows lists. During the uninstallation, not all the TeamCity installation files might be deleted.

### Manual Update

<anchor name="winUpgrading"/>

#### Using Windows Installer

>The main server configuration file `<[TeamCity Home Directory](teamcity-home-directory.md)>/conf/server.xml` is updated automatically when there have been no changes to it since the last installation. If modification were made, the installer will detect them and backup the old `server.xml` file displaying a warning about the overwrite and the backup file location. Other files under `conf` can be overwitten to their default content as well, so if you have made manual modifications in those, check them after the upgrade.

1. [Create a backup](teamcity-data-backup.md). You can create a backup with the "basic" profile on the [TeamCity Maintenance Mode](teamcity-maintenance-mode.md) page on the updated TeamCity start.
2. Note the username used to run the TeamCity server. You will need it during the new version installation.
3. If you have any of the Windows service settings customized, store them to repeat the customizations later.
4. If you are using 64-bit Java to run the service (for example, check for "64" in "Java VM info" on the server's __Administration | Diagnostics__ or in a thread dump), consider backing up the `<[TeamCity Home Directory](teamcity-home-directory.md)>\jre` directory.
5. (optional as these will not be overwritten by the upgrade) Back up customizations of the bundled Tomcat server (like port, HTTPS protocol, and so on) or JRE if any.
6. Check if you a have local agent installed (though it is [not recommended](security-notes.md) to have a local agent), so that you can later select this option in the installer.
7. Run the new installer and point it to the same place TeamCity is installed to (the location used for installation is remembered automatically). Confirm uninstalling the previous installation. The TeamCity uninstaller ensures proper uninstallation, but you might want to make sure the [TeamCity server installation directory](teamcity-home-directory.md) does not contain any non-customized files after uninstallation finishes. If there are any, backup/remove them before proceeding with the installation.
8. If prompted, specify the `<[TeamCity Data Directory](teamcity-data-directory.md)>/` used by the previous installation.
9. (optional as these will not be overwritten by the upgrade) Make sure you have the external database driver [installed](set-up-external-database.md#General+Steps) (this applies only if you use an external database).
10. Check and restore any customizations of Windows services and Tomcat configuration that you need. When upgrading from TeamCity versions 7.1 and earlier, make sure to transfer the [server memory setting](server-startup-properties.md) to the [environment variables](server-startup-properties.md#JVM+Options).
11. If you were using 64-bit Java to run the server, restore the `<[TeamCity Home Directory](teamcity-home-directory.md)>\jre` directory previously backed up or repeat the 64-bit Java [installation steps](how-to.md#Update+from+32-bit+to+64-bit+Java).
12. If you use a customized Log4j configuration in the `conf\teamcity-server-log4j.xml` file and want to preserve it (note that it is recommended using [logging presets](teamcity-server-logs.md#Logging-related+Diagnostics+UI) instead), compare and merge `conf\teamcity-server-log4j.xml.backup` created by the installer from the existing copy with the default file saved with the default name. Compare the `conf\teamcity-*-log4j.xml.dist` file with the corresponding `conf\teamcity-*-log4j.xml` file and make sure that` .xml` file contains all the `.dist` file defaults. It is recommended to copy the `.dist` file over to the corresponding `.xml` file until you really need the changed logging configuration.
13. Start up the TeamCity server (and agent, if it was installed together with the installer).
14. Review the [TeamCity Maintenance Mode](teamcity-maintenance-mode.md) page to make sure there are no problems encountered, and confirm the upgrade by clicking the corresponding button. Only after that all data will be converted to the newer format.

If you encounter errors which cannot be resolved, make sure old TeamCity is not running/does not start on boot, restart the machine, and repeat the installation procedure.

#### Using .tar.gz Distributions

1. [Create a backup](teamcity-data-backup.md).
2. Back up files customized since the previous installation (most probably `[TOMCAT_HOME]/conf/server.xml`)
3. Remove old installation files (the entire `<TeamCity Home Directory>`). It's advised to back up the directory beforehand.
4. Unpack the new archive to the location where TeamCity was previously installed.
5. If you use a Tomcat server (your own or bundled in `.tar.gz` TeamCity distribution), it is recommended to delete the content of the `work` directory. Note that this may affect other web applications deployed into the same web server.
6. Restore customized settings backed up in step 2 above. If you have the customized `[TOMCAT_HOME]/conf/server.xml` file, apply your changes into the appropriate sections of the default file.
7. Make sure the previously configured [TeamCity server startup properties](server-startup-properties.md) (if any) are still actual.
8. Start up the TeamCity server.
9. Review the [TeamCity Maintenance Mode](teamcity-maintenance-mode.md) page to make sure there are no problems encountered, and confirm the upgrade by clicking the corresponding button. Only after that, all the configuration data and database scheme are updated by TeamCity converters.

#### Using Docker images
{id="manual-update-of-docker-image" auxilary-id="Manual Update of Docker Image"}

> Manual update is the only option for Docker containers.
{style="note"}

1. [Create a backup](teamcity-data-backup.md).
2. Back up files customized since the previous installation (most probably `[TOMCAT_HOME]/conf/server.xml`)
3. If you have built your own Docker image, [rebuild your Docker image](https://github.com/JetBrains/teamcity-docker-images#readme) based on the new version of the TeamCity server base image.
4. Invoke [`docker run`](https://hub.docker.com/r/jetbrains/teamcity-server) to start the new container on the target host (either the [official TeamCity](https://hub.docker.com/r/jetbrains/teamcity-server/) image or your rebuilt image).
5. Review the [TeamCity Maintenance Mode](teamcity-maintenance-mode.md) page to make sure there are no problems encountered, and confirm the upgrade by clicking the corresponding button.

### Upgrading Multinode Setups
{product="tc"}

The [Multinode Setup](multinode-setup.md) distributes the tasks performed by your TeamCity server between multiple individual nodes, which significantly boosts the system stability and performance. In this setup, each node needs an individual upgrade. See this section for more information: [](multinode-setup.md#Upgrade%2FDowngrade).

## IDE Plugins
{product="tc"}

It is recommended for all users to regularly update their IDE plugins to the latest version compatible with the TeamCity server version in use — at least to the version available from the TeamCity server's **Tools** section in the user profile.   
Generally, the versions of the IntelliJ IDEA TeamCity plugin and Visual Studio TeamCity add-in have to be the same as the TeamCity server version. Users with non-matching plugin versions get a message on an attempt to log in to the TeamCity server with a non-matching version.

## Server Rollback
{product="tc"}

In a rare scenario of an automatic or manual server upgrade failing, you can restore TeamCity to its previous state. See the following articles for more information:

* [](restoring-teamcity-data-from-backup.md)
* [](manual-backup-and-restore.md)

## Upgrading Build Agents
{product="tc"}

* [Automatic Build Agent Upgrading](#Automatic+Build+Agents+Update)
* [Upgrading Build Agents Manually](#Manual+Build+Agents+Update)
    * [Upgrading the Build Agent Windows Service Wrapper](#Upgrading+the+Build+Agent+Windows+Service+Wrapper)

### Automatic Build Agents Update

On starting the TeamCity server (and updating agent distributions or plugins on the server), TeamCity agents connected to the server and [correctly installed](system-requirements.md#Common+Requirements) are automatically updated to the version corresponding to the server. This occurs for both server upgrades and downgrades. If there is a running build on the agent, the build finishes. No new builds are started on the agent unless the agent is up to date with the server.

Before starting the agent upgrade, the agent is checked for free disk space, 3 GB by default. To modify the value required for the upgrade, configure the `teamcity.agent.upgrade.ensure.free.space` [agent property](configure-agent-installation.md).

The agent update procedure is as follows: The agent (`agent.bat`, `agent.sh`, or agent service) will download the current agent package from the TeamCity server. When the download is complete and the agent is idle, it will start the upgrade process (the agent is stopped, the agent files are updated, and the agent is restarted). This process may take several minutes depending on the agent hardware and network bandwidth. __Do not interrupt the upgrade process__, as doing so may cause the upgrade to fail and you will need to manually reinstall the agent.

If you see that an agent is identified as "Agent disconnected (Will upgrade)" in the TeamCity UI, do not close the agent console or restart the agent process, but wait for several minutes.

Various console windows can open and close during the agent upgrade. Please be patient and do not interrupt the process until the agent upgrade is finished.

### Manual Build Agents Update

All connected agents upgrade automatically, provided they are correctly [installed](install-and-start-teamcity-agents.md), so manual upgrade is not necessary.

If you need to upgrade agent manually, you can follow the steps below.

As TeamCity agent does not hold any unique information, the easiest way to upgrade an agent is to:
1. Back up the `<Agent Home>/conf/buildAgent.properties` file.
2. Uninstall/delete existing agent.
3. Install the new agent version.
4. Restore the previously saved `buildAgent.properties` file to the same location.
5. Start the agent.

If you need to preserve all the agent data (for example, to eliminate clean checkouts after the upgrade), you can:
1. Stop the agent.
2. Delete all the directories in the agent installation present in the agent `.zip` distribution except `conf`.
3. Unpack the `.zip` distribution to the agent installation directory, skipping the "conf" directory.
4. Start the agent.

In the latter case, if you run the agent under Windows using a service, you may also need to upgrade the Windows service as described [below](#Upgrading+the+Build+Agent+Windows+Service+Wrapper).

### Upgrading the Build Agent Windows Service Wrapper

If the service wrapper needs an update, the new version is downloaded into the `<agent>/launcher.latest` directory, however the changes are not applied automatically.

To upgrade the service wrapper manually, do the following:
1. Ensure the `<agent>/launcher.latest` folder exists.
2. Stop the service using `<agent>\bin\service.stop.bat`.
3. Uninstall the service using `service.uninstall.bat`.
4. Backup the `<agent>/launcher/conf/wrapper.conf` file.
5. Delete `<agent>/launcher`.
6. Rename `<agent>/launcher.latest` to `<agent>/launcher`.
7. Edit the `<agent>/launcher/conf/wrapper.conf` file. Check that the `wrapper.java.command` property points to the `java.exe` file. Leave it blank to use the registry to look up java. Leave `java.exe` to look up `java.exe` in `PATH`. For a standalone agent, the service value should be `../jre/bin/java`, for an agent installation on the server the value should be `../../jre/bin/java`. The backup version of the `wrapper.conf` file can be used.
8. Install the service using `<agent>\bin\service.install.bat`.
9. Make sure the service is running under the proper user account. Please note that using SYSTEM can result in failing builds which use MSBuild/Sln2005 configurations.
10. Start the service using `<agent>\bin\service.start.bat`.

<note>

This procedure is applicable ONLY to an agent running with _new_ service wrapper. Make sure you are not running the __agentd__ service.

</note>

<seealso product="tc">
        <category ref="concepts">
            <a href="teamcity-data-directory.md">TeamCity Data Directory</a>
        </category>
        <category ref="admin-guide">
            <a href="teamcity-maintenance-mode.md">TeamCity Maintenance Mode</a>
            <a href="teamcity-data-backup.md">TeamCity Data Backup</a>
        </category>
</seealso>
