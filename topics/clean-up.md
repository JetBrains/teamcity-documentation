[//]: # (title: Clean-Up)
[//]: # (auxiliary-id: Clean-Up)

TeamCity clean-up functionality allows an automatic deletion of old and no longer necessary build data.

The server clean-up configuration is available in __Administration | Server Administration | Clean-up Settings__. It allows setting clean-up schedule and shows general clean-up information.

Clean-up rules, related to specific projects, are configured in __Project Settings | Clean-up Rules__. These rules define what data to clean and what to preserve. They can be assigned to a project or build configuration.   
It is recommended to configure clean-up rules to remove obsolete builds and their artifacts, purge unnecessary data from the database and caches in order to free disk space, remove builds from the TeamCity UI and reduce the TeamCity workload.

Clean-up deletes the data stored under `<`[`TeamCity Data Directory`](teamcity-data-directory.md)`>/system` and in the database. Also, during the clean-up, the server performs various maintenance tasks (for example, resets VCS full patch caches).

## Server Clean-up Settings

The _server clean-up settings_ are configured in __Administration | Server Administration | Clean-up Settings__.

The build history clean-up is run as a background process, which means that there is no server maintenance downtime.   

<note>

If you use the default HSQL database, there is a short period of server unavailability when the HSQL database is being compacted.
</note>

Depending on the amount of data to clean up, the process may take significant time, during which the server might be less performant. Therefore, it is recommended to schedule clean-up for off-peak hours. By default, TeamCity will start cleaning up daily at 3:00 AM. It is also possible to [start clean-up manually](#Manual+Clean-up+Launch).

You can also specify the time limit for the clean-up process. In case not all the data is purged within the specified time frame, the remaining data will be removed during the next clean-up process.

With clean-up enabled, TeamCity will keep the server [audit records](tracking-user-actions.md) for a year (365 days) by default.

### Manual Clean-up Launch

The __Previous clean-up__ section of the server clean-up settings enables you to:
* Review the information on the previous server clean-up date and duration helping you decide whether to launch the clean-up process at a given moment.
* Run clean-up manually using the __Start clean-up now__ button.

During clean-up, TeamCity reports the progress. If you need, you can stop the clean-up process, and the remaining data will be removed during the next clean-up.

## Clean-up Rules

The _clean-up rules_ define how to clean data in the current project, its subprojects and build configurations.

The __Project Settings | Clean-up Rules__ page of the project settings allows viewing and managing the clean-up rules of the current project and its build configurations. If this project has subprojects, you can optionally display their rules as well.

There are two types of project clean-up rules:

* _Base rules_ define what data to delete and when. They are easy to configure and cover most common clean-up cases. One base rule is assigned to each project and build configuration. [Read how configure a base rule](#Base+Rule).
* _Keep rules_ define what data to preserve during the clean-up. They are very flexible but take more effort to configure than base rules. Multiple keep rules can be assigned to a project or build configuration. [Read how to configure a keep rule](#Keep+Rule).

Keep rules are more fine-grained and can cover cases like keeping all the builds with a certain tag (for example, `release`) or in a certain branch. While using the keep rules requires a better understanding of different kinds of builds and their data, it also offers greater flexibility.   
You can set a base rule to configure the common clean-up scenario and add multiple keep rules to tune what exact data to preserve. Or, you can rely exclusively on the keep rules to configure the clean-up logic, but make sure they preserve all valuable data before you disable a base rule.

During the clean-up, TeamCity analyzes and combines the base and keep rules to determine the scopes of data to preserve and to delete.

The clean-up rules assigned to a project are inherited by all its subprojects and build configurations but can be overridden by their own rules. The following diagram shows how rules propagate throughout an example project A:

<img src="clean-up-inheritance.png" width="700" alt="Inheritance of clean-up rules"/>

You can always reset the overridden rule to its original definition in a parent project, closest in the hierarchy.
  
### Base Rule

A single base rule is assigned to each project or build configuration. With a base rule, you can define a number of successful builds to preserve, and/or the time period to keep builds in the history.

The following clean-up levels are available in a base rule:
* _Artifacts_: all data including build logs is preserved; [hidden artifacts](build-artifact.md#Hidden+Artifacts) are also preserved.
* _History_: all the build data is deleted except for builds statistics values that are visible in the [statistics charts](statistic-charts.md).
* _Everything_: no build data remains in TeamCity.

Each level includes those listed above it.

By default, everything is kept forever. When you select custom settings, for each of the levels above you can specify:
* _Number of days_:   
  Builds older than the number of days specified will be cleaned with the specified level. The starting point is the date of the last successful build build, not the current date. A day is equivalent to a 24-hour period, not a calendar day.
* _Number of successful builds_:   
  Only builds older than the last matching successful build will be cleaned with the level specified (all the failed builds between the preserved successful ones are kept).

When both conditions are specified, only the builds, which must be cleaned according to all applied rules and not preserved by keep rules, will be actually removed: TeamCity finds the oldest build to preserve according to each of the rules and then cleans all builds older than the oldest one of the two found.   
Note that if the _Number of successful builds_ limit is specified for the level but there are no successful builds in the history, TeamCity will not clean up any data on this level.

For the _Artifacts_ level you can also specify the patterns for the artifact names: the artifacts matching the specified pattern will be in- or excluded from the clean-up. Use newline-delimited rules following [Ant-like pattern](http://ant.apache.org/manual/dirtasks.html#patterns). For example:
* To clean-up artifacts with `file` as a part of the name, use the following syntax: `+:**/file*.*`.
* To exclude `*.jar` artifacts with `file` as a part of the name from clean-up, use `-:**/file*.jar`.

#### Base Rule Behavior for Dependency Builds

In the _Dependencies_ block of a base rule, you can also choose the clean-up behavior option for build artifacts in dependency build configurations. TeamCity always preserves builds that are used as [snapshot dependencies](dependent-build.md#Snapshot+Dependency) in other builds. These builds are not deleted from build history by the clean-up procedure until dependent builds are deleted. Artifacts of these builds can be deleted based on the option below.   
TeamCity can optionally preserve builds and their artifacts which are used in other builds by [artifact dependencies](dependent-build.md#Artifact+Dependency). The following options are available:
* _Use default_ uses the option configured in the default clean-up rule.
* _Prevent clean-up_ protects builds (and their artifacts) which were used as a source of artifact or snapshot dependencies for the builds of the current build configuration.
* _Do not prevent clean-up_ (default) makes cleanup-related processing of the dependency builds disregard the fact that they are used by the builds of the current build configuration.

For example, a dependent build configuration A has an artifact dependency on B. If the _Prevent clean-up_ option is used for A, the builds of B that provide artifacts for the builds of A will not be processed while cleaning the builds, so the builds and their artifacts will be preserved.

#### Base Rule Behavior for Build Configurations with Feature Branches

If a build configuration has builds from several [branches](working-with-feature-branches.md), before applying a base clean-up rule, TeamCity splits the build history of this configuration into several groups. TeamCity creates one group per each [active branch](working-with-feature-branches.md#Active+branches), and a single group for all builds from inactive branches. Then the base clean-up rule is applied to each group independently.

#### Base Rule Behavior for Personal Builds

Base clean-up rules are applied separately for the non-personal builds and then for the personal builds. That is, if you have a rule to preserve 3 successful builds, 3 non-personal builds and 3 personal builds are preserved (in each branch group as per description above).

#### Base Rule for Build Configuration Template

Before TeamCity 2019.2, it was possible to assign a base rule to a build configuration template. For compatibility, all existing clean-up rules for build configuration templates will stay and can be accessed on the __Clean-up Rules__ page.

### Keep Rule

A keep rule defines what particular data to preserve during the clean-up. Multiple keep rules can be assigned to a project or build configuration.

In each keep rule, you can configure the following settings:
* __Build data to preserve__: history, artifacts, logs, statistics, or everything.
* To __keep artifacts in dependencies__ or not. This option controls if the builds of the dependency build configurations are also cleaned up. With this option enabled, if some build is preserved by this rule, all artifacts of its dependency builds will also be preserved. The option works similarly to the _[Dependencies](#Base+Rule+Behavior+for+Dependency+Builds)_ option of a base rule.
* __Builds range__: what time interval or what number of last builds will be affected by the rule.
* Optionally, you can __filter__ the preserved builds by their __status__, __tags__, and __branches__ (a [branch filter](branch-filter.md) with pattern matching is supported). You can also restrict the rule only to personal or non-personal builds.

<anchor name="applies-to"/>

* Choose if the limits must be applied to each matching branch individually or to all the builds in the selected branches as a single set.   
This condition applies once per each affected build configuration. If no branches are specified in the filter, it applies as follows: either to each branch in a build configuration or to all the build configuration branches as a set.   
Example: if you select to keep statistics of 10 last builds and choose two branches for this rule, you can either choose "_All selected_" to preserve 10 last builds among both selected branches or choose "_Each selected branch_" to preserve 20 last builds – 10 for each of the branches.

TeamCity processes a keep rule in the following order:
1. __Filters__ the builds of the affected project or build configuration.
2. Determines if the rule will apply to all selected branches or to builds in each selected branch individually (see [description](#applies-to)).
3. Keeps builds only in a specified __range__.
4. Determines what types of __build data to preserve__.
5. Determines if the artifacts for the dependency builds must be preserved.

Notes on the keep rule behavior:
* If several tags are entered, the rule will apply to all builds marked with any of these tags.
* In all "_Days since..._" range options, TeamCity doesn't consider fractional hours of a certain date and applies a rule only to builds that started in a time interval from the selected day (from midnight until 11:59 PM) and back to the starting day of the affected range (the selected day minus the configured limit of days).

## Deleted Build Configurations Clean-up

When a project or a build configuration is deleted, the corresponding build data is removed during the clean-up, but only if 5 days (432, 000 seconds) have passed since the deletion. To change the timeout, set the `teamcity.deletedBuildTypes.cleanupTimeout` [internal property](configuring-teamcity-server-startup-properties.md#TeamCity+internal+properties) to the required number of seconds to protect the data from deletion.

There are builds that preserve all their data and are not affected during clean-up. These are:
* [pinned builds](pinned-build.md)
* builds used as an [artifact of snapshot dependency](configuring-dependencies.md) in other builds when the "_[Prevent clean-up](#Base+Rule+Behavior+for+Dependency+Builds)_" option for dependencies is enabled in the build configuration. Such builds are marked with the ![link.png](link.png) icon in the build history list
* builds of build configurations that were deleted less than one day ago

[//]: # (Internal note. Do not delete. "Clean-Upd55e230.txt")    

<seealso>
        <category ref="concepts">
            <a href="dependent-build.md">Dependent Build</a>
        </category>
</seealso>