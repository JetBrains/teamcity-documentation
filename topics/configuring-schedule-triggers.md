[//]: # (title: Configuring Schedule Triggers)
[//]: # (auxiliary-id: Configuring Schedule Triggers)

<tag-list of="chapter" mode="tree" depth="4"/>


The _Schedule Trigger_ allows you to set the time when a build of the configuration will be run. The __[Builds Schedule](builds-schedule.md)__ page of the current project settings displays the configured build times. More than one schedule trigger can be added to a build configuration.

## Triggering Conditions

The settings in this section define time and other conditions for automatic build triggering. You can schedule a recurring build or set a specific date and time for it.

### Date and Time

In addition to triggering builds __daily__ or __weekly__ at a specified time for a particular time zone, you can specify advanced time settings using [cron](http://en.wikipedia.org/wiki/Cron#Operators)\-like expressions. This format provides more flexible scheduling options.

TeamCity uses [Quartz](http://www.quartz-scheduler.org/) for working with cron expressions. See the examples below or consider using the [CronMaker](http://www.cronmaker.com/) utility  to generate expressions based on the Quartz cron format. 

#### Examples

<table><tr>

<td>


</td>

<td>

Each 2 hours at :30


</td>

<td>

Every day at 11:45PM


</td>

<td>

Every Sunday at 1:00AM


</td>

<td>

Every last day of month at 10:00AM and 10:00PM


</td></tr><tr>

<td>

Seconds


</td>

<td>

0


</td>

<td>

0


</td>

<td>

0


</td>

<td>

0


</td></tr><tr>

<td>

Minutes


</td>

<td>

30


</td>

<td>

45


</td>

<td>

0


</td>

<td>

0


</td></tr><tr>

<td>

Hours


</td>

<td>

0/2


</td>

<td>

23


</td>

<td>

1


</td>

<td>

10,22


</td></tr><tr>

<td>

Day\-of\-month


</td>

<td>

\*


</td>

<td>

\*


</td>

<td>

?


</td>

<td>

L


</td></tr><tr>

<td>

Month


</td>

<td>

\*


</td>

<td>

\*


</td>

<td>

\*


</td>

<td>

\*


</td></tr><tr>

<td>

Day\-of\-week


</td>

<td>

?


</td>

<td>

?


</td>

<td>

1


</td>

<td>

?


</td></tr><tr>

<td>

Year(Optional)


</td>

<td>

\*


</td>

<td>

\*


</td>

<td>

\*


</td>

<td>

\*


</td></tr></table>

See also [other examples](http://www.quartz-scheduler.org/documentation/quartz-2.2.x/tutorials/tutorial-lesson-06.html).

#### Brief description of the cron format used

Cron expressions are comprised of six fields and one optional field separated with a white space. The fields are respectively described as follows:

<table><tr>

<td>

Field Name


</td>

<td>

Values


</td>

<td>

Special Characters


</td></tr><tr>

<td>

Seconds


</td>

<td>

0\-59


</td>

<td>

, \- \* /


</td></tr><tr>

<td>

Minutes


</td>

<td>

0\-59


</td>

<td>

, \- \* /


</td></tr><tr>

<td>

Hours


</td>

<td>

0\-23


</td>

<td>

, \- \* /


</td></tr><tr>

<td>

Day\-of\-month


</td>

<td>

1\-31


</td>

<td>

, \- \* ? / L W


</td></tr><tr>

<td>

Month


</td>

<td>

1\-12 of JAN\-DEC


</td>

<td>

, \- \* /


</td></tr><tr>

<td>

Day\-of\-week


</td>

<td>

1\-7 or SUN\-SAT


</td>

<td>

, \- \* ? / L #


</td></tr><tr>

<td>

Year (Optional)


</td>

<td>

empty, 1970\-2099


</td>

<td>

, \- \* /


</td></tr></table>

For the description of the special characters, please refer to [Quartz CronTrigger Tutorial](http://www.quartz-scheduler.org/documentation/quartz-2.2.x/tutorials/crontrigger.html#special-characters).

### VCS Changes

You can restrict a schedule trigger to start builds only if there are pending changes in your version control by enabling the __Trigger only if there are pending changes__ option. This option considers only newly detected pending changes: if there were pending changes before the trigger was created, the build is not triggered. 

### VCS Trigger Rules

<include src="configuring-vcs-triggers.md" include-id="vcs-trigger-rules"/>

#### General Syntax

<include src="configuring-vcs-triggers.md" include-id="general-syntax"/>

#### Trigger Rules Examples

<include src="configuring-vcs-triggers.md" include-id="trigger-rules-examples"/>

### Watching Different Build

A schedule trigger can watch a build in a different build configuration and run a build in the trigger's configuration if a watched build configuration changes. You can select one of the following changes as the condition for triggering:
* Last finished build
* Last successful build
* Last [pinned build](pinned-build.md)
* Last finished build with a specified [build tag](build-tag.md)

For example, build configuration A has a schedule trigger that starts each 5 minutes and watches the _last successful build_ in configuration B. If the trigger detects a new build B that has finished successfully in the last 5 minutes, it runs build A.

<img src="schedule-trigger-watch.png" width="500" alt="Triggered on a watched build"/>

In the __Watching Different Build__ section, enable the _Trigger only if the watched build changes_ option and specify the build configuration and the type of build to watch.

If the triggered build depends on the watched build via a [snapshot](dependent-build.md#Snapshot+Dependency) or [artifact](dependent-build.md#Artifact+Dependency) dependency, select the _Promote the watched build_ option so TeamCity can automatically [promote](triggering-a-custom-build.md#Promoting+Build) the watched build to the triggered build.

## Additional Options

### Enforce Clean Checkout 

Enable the _Delete all files in the checkout directory before the build_ option to force TeamCity to clean all files in the checkout directory before running a build.   
This option can also be applied to snapshot dependencies. In this case, all the builds of the build chain will be forced to use [clean checkout](clean-checkout.md). The option also enables rebuilding all dependencies (unless custom dependencies are provided via the custom build dialog or the schedule trigger promotes a build).

### Trigger Build on All Enabled and Compatible Agents

Use this option to run a build simultaneously on all agents that are enabled and compatible with the build configuration. This option may be useful in the following cases:

* run a build for agent maintenance purposes (for example, you can create a configuration to check whether agents function properly after an environment upgrade/update)
* run a build on different platforms (for example, you can set up a configuration and specify for it a number of compatible build agents with different environments installed)

<chunk include-id="queue-optimization">

### Build Queue Optimization Settings

By default, TeamCity [optimizes the build queue](build-queue.md): already queued build can be replaced with an already started build or a more recent queued build. You can disable this default behavior by unchecking the corresponding box.
</chunk>

### Branch Filter

By default, the schedule trigger works for all branches.

Read more in [Branch Filter](branch-filter.md).

### Trigger Rules and Branch Filter Combined

Trigger rules and branch filter are combined by __AND__, which means that the build is triggered only __when both conditions are satisfied__.

For example, if you specify a comment text in the trigger rules field and provide the branch specification, the build will be triggered only if a commit has the specified text and is also in a branch matched by the branch filter. 