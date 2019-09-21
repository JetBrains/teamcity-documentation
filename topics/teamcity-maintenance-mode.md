[//]: # (title: TeamCity Maintenance Mode)
[//]: # (auxiliary-id: TeamCity Maintenance Mode)
If you see the TeamCity Maintenance page on the TeamCity startup, that means this TeamCity instance requires technical maintenance before TeamCity can start. For security reasons it is to be performed by a __system administrator__ who has administrative access to the environment where the TeamCity server is installed. In most cases this page appears if the data format expected by the TeamCity installation doesn't correspond to the data format in the Data Directory or the database; for example, during [Upgrade](upgrade.md) TeamCity will display this page before converting the data to a newer format. If you do __not__ have access to the computer where TeamCity is installed, inform your system administrator that TeamCity requires technical maintenance.

If you are a TeamCity system administrator, to confirm that enter the _Token_ into the corresponding field on this page. This token can be found in the `teamcity-server.log` file under `<`[`TeamCity home>/logs`](teamcity-server-logs.md).

After you have provided this token, you can review the details on what kind of maintenance is required. The need in technical maintenance may be caused, for example, by one of the following:
* [TeamCity Data Upgrade](#TeamCity+Data+Upgrade)

[//]: # (Internal note. Do not delete. "TeamCity Maintenance Moded316e49.txt")    

* [TeamCity Data Format is Newer](#TeamCity+Data+Format+is+Newer)
* [TeamCity Startup Error](#TeamCity+Startup+Error)
* [TeamCity Database Creation](#TeamCity+Database+Creation)

## TeamCity Data Upgrade

When updating your TeamCity instance to a new feature release, the newly installed version of TeamCity checks if the TeamCity Data Directory and database use the same data format as required by the TeamCity version. If the newer version requires data conversion, this page is displayed with data format details. Please, review them carefully to ensure that the Data Directory and the database locations are indeed those meant to be used by the server.

If you haven't backed up your data before, do it at this point by using the option on the page (available when upgrading from TeamCity 6.0 or higher). Note that the backup process can take time, especially with large TeamCity installations. Other ways to create the backup are detailed in the [TeamCity Data Backup](teamcity-data-backup.md) section.

Once TeamCity converts the data, downgrade won't be possible. If you need to return to an earlier TeamCity version, you will be able to do that only by restoring the data from a corresponding backup.

When you are sure you have backed up your data, click __Upgrade__.



[//]: # (Internal note. Do not delete. "TeamCity Maintenance Moded316e91.txt")    




## TeamCity Data Format is Newer

TeamCity has detected that the data format corresponds to more recent TeamCity version than you try to run.  Since downgrade is not supported, TeamCity cannot start until you provide the data that matches the format of the TeamCity version you want to run. To do so, please restore the required data from backup. Refer to the [Restoring TeamCity Data from Backup](restoring-teamcity-data-from-backup.md) page for the instructions.

## TeamCity Startup Error

If on TeamCity startup a critical error was encountered, this page will display the error message.  Please, fix the error cause and restart the TeamCity server.

## TeamCity Database Creation

If you see this screen when [settings up TeamCity with an external database](setting-up-an-external-database.md) or [after migrating to an external database](migrating-to-an-external-database.md), click __Proceed__ to create a new database for TeamCity.

 
 __  __

__See also:__


__Installation and Upgrade__: [Upgrade](upgrade.md)   
__Administrator's Guide__: [TeamCity Data Backup](teamcity-data-backup.md)
