[//]: # (title: Configuring Schedule Triggers)
[//]: # (auxiliary-id: Configuring Schedule Triggers)

The _schedule trigger_ allows you to set the time when a build of the configuration will be run. The __[Builds Schedule](builds-schedule.md)__ section of the current __Project Settings__ displays the configured build times. More than one schedule trigger can be added to a build configuration.

## Triggering Conditions

The settings in this section define time and other conditions for automatic build triggering. You can schedule a recurring build or set a specific date and time for it.

### Date and Time

In addition to triggering builds __daily__ or __weekly__ at a specified time for a particular time zone, you can specify advanced time settings using [cron](https://en.wikipedia.org/wiki/Cron#Operators)\-like expressions. This format provides more flexible scheduling options.

TeamCity uses [Quartz](https://www.quartz-scheduler.org/) for working with cron expressions. See the examples below or consider using the [CronMaker](http://www.cronmaker.com/) utility to generate expressions based on the Quartz cron format. 

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

See also [other examples](https://www.quartz-scheduler.org/documentation/quartz-2.3.0/tutorials/tutorial-lesson-06.html).

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

For the description of the special characters, please refer to [Quartz CronTrigger Tutorial](https://www.quartz-scheduler.org/documentation/quartz-2.3.0/tutorials/crontrigger.html#special-characters).

### VCS Changes

You can restrict a schedule trigger to start builds only if there are pending changes in your version control by enabling the __Trigger only if there are pending changes__ option. This option considers only newly detected pending changes: if there were pending changes before the Trigger was created, the build is not triggered. 

### VCS Trigger Rules

<include src="configuring-vcs-triggers.md" include-id="vcs-trigger-rules"/>

#### General Syntax
{id="general-syntax-1"}

<include src="configuring-vcs-triggers.md" include-id="general-syntax"/>

#### Trigger Rules Examples
{id="trigger-rules-examples-1"}

<include src="configuring-vcs-triggers.md" include-id="trigger-rules-examples"/>

<anchor name="ConfiguringScheduleTriggers-WatchedBuild"/>

### Build Changes

A schedule trigger can watch a build in any specified build configuration and trigger a build only if the watched build has changed since the previous triggering. You can select which build to watch:
* Last finished build
* Last successful build
* Last [pinned build](pinned-build.md)
* Last finished build with a specified [build tag](build-tag.md)

If the Trigger detects a new build that satisfies the selected characteristic in the watched configuration, it queues a new build in own configuration.
 
The Trigger watches only regular (not [personal](personal-build.md) or [history](history-build.md)) builds in the default branch.
 
If the triggered build depends on the watched build via a snapshot or artifact dependency, select the "_Promote watched build_" option so TeamCity can automatically [promote](triggering-a-custom-build.md#Promoting+Build) the detected build to the triggered build. Otherwise, the build will be triggered as usual and will have no relation to the detected build.

## Additional Options

### Enforce Clean Checkout 

Enable the _Delete all files in the checkout directory before the build_ option to force TeamCity to clean all files in the checkout directory before running a build.   
This option can also be applied to snapshot dependencies. In this case, all the builds of the build chain will be forced to use [clean checkout](clean-checkout.md). The option also enables rebuilding all dependencies (unless custom dependencies are provided via the custom build dialog or a schedule trigger promotes a build).

### Trigger Build on All Enabled and Compatible Agents

Use this option to run a build simultaneously on all agents that are enabled and compatible with the build configuration. This option may be useful in the following cases:

* run a build for agent maintenance purposes (for example, you can create a configuration to check whether agents function properly after an environment upgrade/update)
* run a build on different platforms (for example, you can set up a configuration and specify for it a number of compatible build agents with different environments installed)

<chunk include-id="queue-optimization">

### Build Queue Optimization Settings

By default, TeamCity [optimizes the build queue](build-queue.md#Build+Queue+Optimization+by+TeamCity): already queued build can be replaced with an already started build or a more recent queued build. You can disable this default behavior by unchecking the corresponding box.
</chunk>

### Branch Filter

By default, a schedule trigger works for all branches.

Read more in [Branch Filter](branch-filter.md).

### Trigger Rules and Branch Filter Combined

Trigger rules and branch filter are combined by __AND__, which means that the build is triggered only __when both conditions are satisfied__.

For example, if you specify a comment text in the trigger rules field and provide the branch specification, the build will be triggered only if a commit has the specified text and is also in a branch matched by the branch filter.

