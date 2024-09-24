[//]: # (title: TeamCity Maintenance Mode)
[//]: # (auxiliary-id: TeamCity Maintenance Mode)

If you see the TeamCity Maintenance page on the TeamCity startup, it means that this TeamCity instance requires technical maintenance before it can start. In most cases, this page appears if the data format expected by the TeamCity installation does not correspond to the data format in the [Data Directory](teamcity-data-directory.md) or the database. For example, during [upgrade](upgrading-teamcity-server-and-agents.md), TeamCity will display this page before converting the data to a newer format.

For security reasons, the maintenance has to be performed by a __system administrator__ who has administrative access to the environment where the TeamCity server is installed. If you __do not__ have access to the computer where TeamCity is installed, inform your system administrator that TeamCity requires technical maintenance.

If you are a TeamCity system administrator, confirm it by entering the _authentication token_ into the corresponding field on this page. This token can be found in the `teamcity-server.log` file under [`<TeamCity Home Directory>/logs`](teamcity-server-logs.md).

After you have provided this token, you can review the details on what kind of maintenance is required. It can be caused by one of the following factors:
* [TeamCity data upgrade](#TeamCity+Data+Upgrade)
* [New TeamCity data format](#New+TeamCity+Data+Format)
* [TeamCity startup error](#TeamCity+Startup+Error)
* [TeamCity database creation](#TeamCity+Database+Creation)

<!--[//]: # (Internal note. Do not delete. "TeamCity Maintenance Moded316e49.txt")-->

## TeamCity Data Upgrade

When updating your TeamCity instance, the installed version of TeamCity checks if the [TeamCity Data Directory](teamcity-data-directory.md) and database use the same data format as required by the new TeamCity version. If the newer version requires data conversion, this page is displayed with data format details. Review them carefully to ensure that the Data Directory and the database locations are indeed those meant to be used by the server.

If you haven't backed up your data before, do it at this point by using the provided option. Note that the backup process can take time, especially with large TeamCity installations. Other ways to back up are detailed in [this article](teamcity-data-backup.md).

Once TeamCity converts the data, downgrade will not be possible. If you need to return to an earlier TeamCity version, you will be able to do that only by restoring the data from the corresponding backup.

After the data is backed up, click __Upgrade__.

<!--[//]: # (Internal note. Do not delete. "TeamCity Maintenance Moded316e91.txt")-->

## New TeamCity Data Format

TeamCity has detected that the data format corresponds to a later TeamCity version than you try to run. Since downgrade is not supported, TeamCity cannot start until you provide the data that matches the format of the TeamCity version you want to run. To do so, restore the required data from backup. Refer to [this article](restoring-teamcity-data-from-backup.md) for instructions.

## TeamCity Startup Error

If a critical error was encountered on TeamCity startup, this page will display the error message. To proceed, you need to fix the error cause and restart the TeamCity server.

## TeamCity Database Creation

If you see this screen when [setting up TeamCity with an external database](set-up-external-database.md) or [after migrating to an external database](migrating-to-external-database.md), click __Proceed__ to create a new database for TeamCity.
 
<seealso>
        <category ref="installation">
            <a href="upgrading-teamcity-server-and-agents.md">Upgrade</a>
        </category>
        <category ref="admin-guide">
            <a href="teamcity-data-backup.md">TeamCity Data Backup</a>
        </category>
</seealso>