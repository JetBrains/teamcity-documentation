[//]: # (title: Tracking User Actions)
[//]: # (auxiliary-id: Tracking User Actions)
TeamCity logs user actions into the Audit log, which is available on the __Administration | Audit__ page. Here you can find out who deleted a build configuration or project, assigned a role to a user, added a user to a group, modified a build configuration, and much more. To find the required information faster, filter the log by the activity type, projects/build configurations, and/or particular users.

If settings of a project or build configuration were modified, you can see the name of user who made the modification and view the change itself by clicking the corresponding link. Project and build configuration settings are stored on the disk in plain xml, so the link will open the usual TeamCity diff window showing changes in these xml files.

You can also view the latest modifications made to a project or build configuration on the project/build configuration settings page by clicking the __view history__ link.

The audit log also can be retrieved in a text form, see the [TeamCity Server Logs](teamcity-server-logs.md) file.

### Audit storage period

By default, the audit log keeps records for one year (set to 365 by default) only if [clean-up](clean-up.md) is enabled. Without clean\-up, the records are intended to be kept forever. 

To modify the audit storage period, specify the number of days for the following [internal property](configuring-teamcity-server-startup-properties.md#TeamCity+internal+properties): `teamcity.audit.cleanupPeriod`.
