[//]: # (title: Tracking User Actions)
[//]: # (auxiliary-id: Tracking User Actions)

TeamCity logs user actions into the Audit log, which is available on the __Administration | Audit__ page.

## View User Audit Log

Here you can see who deleted a build configuration or project, assigned a role to a user, added a user to a group, modified a build configuration, and much more. To find the required information faster, filter the log by the activity type, projects/build configurations, and/or particular users.

If settings of a project or build configuration were modified, you can see the name of a user who made the modification and view the change itself by clicking the corresponding link. Project and build configuration settings are stored on the disk in plain XML, so the link will open a usual TeamCity diff window showing changes in these XML files.

You can also view the latest modifications made to a project or build configuration on its __Settings__ page by clicking __View history__.

The audit log can also be retrieved in a text form, see the [TeamCity server log](teamcity-server-logs.md) file.
{instance="tc"}

## Audit Storage Period
{instance="tc"}

TeamCity preserves and shows all audit logs indefinitely unless (a) respective projects or agents are deleted manually or (b) [server clean-up](teamcity-data-clean-up.md#Server+Clean-up+Settings) is enabled. In (b) case, nonessential audit records are cleaned up automatically after one year. If there are no audit entries to show for a project or configuration, the _View history_ link will not be available.

To modify this storage period, specify the number of days in the `teamcity.audit.cleanupPeriod` [internal property](server-startup-properties.md#TeamCity+Internal+Properties) (365 by default).