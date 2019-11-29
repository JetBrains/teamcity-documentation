[//]: # (title: Clean-Up)
[//]: # (auxiliary-id: Clean-Up)

On this page

<tag-list of="chapter" mode="tree" depth="4"/>


Clean-up in TeamCity is a feature allowing an automatic deletion of data belonging to old builds.

The server clean-up configuration is available in the server __Administration | Clean-up Settings__.

 Project-related clean-up settings are configured in the __Project Settings | Clean-up Rules__.

It is recommended to configure clean-up rules to remove obsolete builds and their artifacts, purge unnecessary data from database and caches in order to free disk space, remove builds from the TeamCity UI and reduce the TeamCity workload.

Clean-up deletes the data stored under `<`[`TeamCity Data Directory`](teamcity-data-directory.md)`>/system` and in the database. Also, during the clean-up time the server performs various maintenance tasks (for example, resets VCS full patch caches).


## Server Clean-up Settings

The server settings are configured on the __Administration | Server Administration | clean-up Settings__.

The build history clean-up is run as a background process, which means that there is no server maintenance down\-time.   

<note>

If you use the default HSQL database, there is a short period of server unavailability when the HSQL database is being compacted.
</note>

Depending on the amount of data to clean up, the process may take significant time, during which the server might be less performant. Therefore, it is recommended to schedule clean-up for off\-peak hours. By default, TeamCity will start cleaning up daily at 3.00 AM. It is also possible to [start clean-up manually](#Manual+Clean-up+Launch).

You can also specify the time limit for the clean-up process. In case not all the data is purged within the time\-frame specified, the remaining data will be removed during the next clean-up process.

With clean-up enabled, TeamCity will keep the server [audit records](tracking-user-actions.md) for a year (365 days) by default.

### Manual Clean-up Launch

The __Previous clean-up__ section of the server clean-up settings enables you to:
* review the information on the previous server clean-up date and duration helping you decide whether to launch the clean-up process at a given moment
* run clean-up manually using the __Start clean-up now__ button.
During clean-up, TeamCity reports the progress. If you need, you can stop the clean-up process and the remaining data will be removed during the next clean-up.


## Project Clean-up Rules

_clean-up_ rules are configured per project and define when and what data to clean.

To manage the rules, use the __Project-Settings | Clean-up Rules__ page where you can specify different rules for a project or a build configuration. You can also specify them for a template, and then they will be inherited by all build configurations using this template.

The following inheritance rules apply:
* if a clean-up rule is assigned to a project, it becomes default for all build configurations or subprojects in this project
* if a clean-up rule is assigned to a template, it becomes default for all configurations inherited from this template, but if clean-up rules are assigned to both a template and a project, the rule from the project will override the rule from the template
* if a clean-up rule is assigned to a build configuration, it will override the clean-up rule from the project or a template it is based on.   

In each rule, you can define a number of successful builds to preserve, and/or the period of time to keep builds in history (for example, keep builds for 7 days).

The following clean-up levels are available:
* __Artifacts__ (all other data including build logs is preserved. [Hidden Artifacts](build-artifact.md#Hidden+Artifacts) are also preserved);
* __History__ (all the build data is deleted except for builds statistics values that are visible in the [statistics charts](statistic-charts.md));
* __Everything__ (no build data remains in TeamCity).

Each level includes the one(s) listed above it.

By default, everything is kept forever. When you select custom settings, for each of the items above you can specify:
* the number of days. Builds older than the number of days specified will be cleaned with the specified level. The starting point is the date of the last build, not the current date. A day is equivalent to a 24-hour period, not a calendar day;
* the number of successful builds. Only builds older than the last matching successful build will be cleaned with the level specified (all the failed builds between the preserved successful ones are kept). This rule is only taken into account if there are successful builds in the build configuration.

When both conditions are specified, only the builds which must be cleaned according to all rules will be actually removed: TeamCity finds the oldest build to preserve according to each of the rules and then cleans all builds older than the oldest one of the two found.

For the __Artifacts__ level you can also specify the patterns for the artifact names: the artifacts matching the specified pattern will be in/excluded from the clean-up. Use newline\-delimited rules following [Ant-like pattern](http://ant.apache.org/manual/dirtasks.html#patterns). For example:
  * to clean-up artifacts with 'file' as a part of the name, use the following syntax: `+:**/file*.*`.
  * to exclude `*.jar` artifacts with `file` as a part of the name from clean-up, use `-:**/file*.jar`.

There are builds that preserve all their data and are not affected during cleanup. These are:
* [pinned builds](pinned-build.md);
* builds used as an [artifact of snapshot dependency](configuring-dependencies.md) in other builds when the "Prevent clean-up" option for dependencies is enabled in the build configuration. See [Clean-Up for Dependent Builds](#Clean-up+for+Dependent+Builds) below. Such builds are marked with the ![link.png](link.png) icon in the build history list;
* builds of build configurations that were deleted less than one day ago.

[//]: # (Internal note. Do not delete. "Clean-Upd55e230.txt")    


### Clean-up for Dependent Builds

The settings in the __Dependencies__ section of the __Edit Clean Up Rules__ dialog affect clean-up of artifacts in builds that the builds of the current build configuration depend on.

TeamCity always preserves builds which are used as [snapshot dependencies](dependent-build.md#Snapshot+Dependency) in other builds. These builds are not deleted from builds history by the clean-up procedure until dependent builds are deleted. Artifacts of these builds can be deleted based on the option below.TeamCity can optionally preserve builds and their artifacts which are used in other builds by [artifact dependencies](dependent-build.md#Artifact+Dependency).
* __Use default__ choice uses the option configured in the default cleanup rule.
* __Prevent clean-up__ choice protects builds (and their artifacts) which were used as a source of artifact or snapshot dependencies for the builds of the current build configuration.
* __Do not prevent clean-up__ (default) choice makes cleanup\-related processing of the dependency builds disregard the fact that they are used by the builds of the current build configuration.

Example:

Say, a dependent build configuration A has an artifact dependency on B. If the __Prevent clean-up__ option is used for A, the builds of B that provide artifacts for the builds of A will not be processed while cleaning the builds, so the builds and their artifacts will be preserved.


There is a [known issue](https://youtrack.jetbrains.com/issue/TW-59344): when a build is used via an artifact dependency by a build that has any snapshot dependency, then, even if the build configuration of the dependent build has the __Do not prevent clean-up__ option set, the original artifact dependency build and its artifacts will not be cleaned, as if the __Prevent clean-up__ option is set in the dependent build configuration.


### Clean-up in Build Configurations with Feature Branches

If a build configuration has builds from several [branches](working-with-feature-branches.md), before applying clean-up rules, TeamCity splits the build history of this configuration into several groups. TeamCity creates one group per each [active branch](working-with-feature-branches.md#Active+branches), and a single group for all builds from inactive branches. Then clean-up rules are applied to each group independently.

### Clean-up of Personal Builds

clean-up rules are applied separately for the non\-personal builds and then for the personal builds. That is, if you have a rule to preserve 3 successful builds, 3 non\-personal builds and 3 personal builds are preserved (in each branch group as per description above).

### Deleted Build Configurations Cleanup

When a project or a build configuration is deleted, the corresponding builds data is removed during the cleanup, but only if 5 days (432, 000 seconds) have passed since the deletion. To change the timeout, set the `teamcity.deletedBuildTypes.cleanupTimeout` [internal property](configuring-teamcity-server-startup-properties.md#TeamCity+internal+properties) to the required number of seconds to protect the data from deletion.

 __  __
 
__See also:__

__Concepts__: [Dependent Build](dependent-build.md)

__ __
