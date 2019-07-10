[//]: # (title: TeamCity Maintenance Mode)
[//]: # (auxiliary-id: TeamCity Maintenance Mode)
If you see the TeamCity Maintenance page on the TeamCity startup, that means this TeamCity instance requires technical maintenance. For security reasons it is to be performed by a __system administrator__ who has access to the computer where the TeamCity server is installed. In most cases this page appears if the data format doesn't correspond to the required format; for example, during [Upgrade](upgrade.md) TeamCity will display this page before converting the data to a newer format. If you do __not__ have access to the computer where TeamCity is installed, inform your system administrator that TeamCity requires technical maintenance.

If you are a TeamCity system administrator, to confirm that enter the _Token_ into the corresponding field on this page. This token can be found in the `teamcity-server.log` file under `<`[TeamCity home>/logs`](teamcity-server-logs.md).

 After you have provided this token, you can review the details on what kind of maintenance is required. The need in technical maintenance may be caused, for example, by one of the following:
* [TeamCity Data Upgrade](#TeamCity+Data+Upgrade)

[//]: # (Internal note. Do not delete. "TeamCity Maintenance Moded316e49.txt")    

* [TeamCity Data Format is Newer](#TeamCity+Data+Format+is+Newer)
* [TeamCity Startup Error](#TeamCity+Startup+Error)
* [TeamCity Database Creation](#TeamCity+Database+Creation)

## TeamCity Data Upgrade

When upgrading your TeamCity instance, the newly installed version of TeamCity checks if the TeamCity data directory and database use the old data format when it is started for the first time. If the newer version requires data conversion, this page is displayed with data format details. Please, review them carefully.

 If you haven't backed up your data, do it at this point. Once TeamCity converts the data, downgrade won't be possible. If you need to return to an earlier TeamCity version, you will be able to do that only by restoring the data from a corresponding backup. Please refer to the [TeamCity Data Backup](teamcity-data-backup.md) section for instructions. When you are sure you have backed up your data, click __Upgrade__. When upgrading from TeamCity 6.0 (or higher), this page contains an option to perform backup automatically.





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
