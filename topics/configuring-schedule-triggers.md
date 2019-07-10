[//]: # (title: Configuring Schedule Triggers)
[//]: # (auxiliary-id: Configuring Schedule Triggers)

<tag-list of="chapter" mode="tree" depth="4"/>


The _Schedule Trigger_ allows you to set the time when a build of the configuration will be run. The __[Builds Schedule](builds-schedule.md)__ page of the current project settings displays the configured build times. More than one Schedule trigger can be added to a build configuration.

## Triggering Conditions

The settings in this section define time and other conditions for automatic build triggering. You can schedule a recurring build or set a specific date and time for it.

### Date and Time

In addition to triggering builds __daily__ or __weekly__ at a specified time for a particular time zone, you can specify advanced time settings using [cron](http://en.wikipedia.org/wiki/Cron#Operators)\-like expressions. This format provides more flexible scheduling options.

TeamCity uses [Quartz](http://www.quartz-scheduler.org/) for working with cron expressions. See the examples below or consider using the [CronMaker ](http://www.cronmaker.com/)utility  to generate expressions based on Quartz cron format. 

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

Year(Optional)


</td>

<td>

empty, 1970\-2099


</td>

<td>

, \- \* /


</td></tr></table>

For the description of the special characters, please refer to [Quartz CronTrigger Tutorial](http://www.quartz-scheduler.org/documentation/quartz-2.2.x/tutorials/crontrigger.html#special-characters).

### VCS Changes

You can restrict schedule trigger to start builds only if there are pending changes in your version control by selecting the corresponding option. The __Trigger only if there are pending changes__ option considers newly detected pending changes only: if there were pending changes before the trigger was created, the build is not triggered. 

<include src="configuring-vcs-triggers.md" include-id="vcs-trigger-rules"/>

### Build Changes

The Schedule Trigger can watch a build in a different build configuration and trigger a build if the watched build changes.

In the __Build Changes__ section, select the corresponding box and specify the build configuration and the type of build to watch: last successful build, last [pinned build](pinned-build.md), last finished build, or the last finished build with a specified tag.

TeamCity can [promote](triggering-a-custom-build.md) the watched build if there is a dependency (snapshot or artifact) on its build configuration.

## Additional Options

### Enforce Clean Checkout 

It is possible to force TeamCity to clean all files in the checkout directory before a build. This option can also be applied to snapshot dependencies. In this case, all the builds of the build chain will be forced to use [clean checkout](clean-checkout.md). The option also enables rebuilding all dependencies (unless custom dependencies are provided via the custom build dialog or the schedule trigger promotes a build).

### Trigger Build on All Enabled and Compatible Agents

Use this option to run a build simultaneously on all agents that are enabled and compatible with the build configuration. This option may be useful in the following cases:

* run a build for agent maintenance purposes (e.g. you can create a configuration to check whether agents function properly after an environment upgrade/update).
* run a build on different platforms (for example, you can set up a configuration, and specify for it a number of compatible build agents with different environments installed).

<chunk include-id="queue-optimization">

### Build Queue Optimization Settings

By default, TeamCity [optimizes the build queue:](build-queue.md) already queued build can be replaced with an already started build or a more recent queued build. To disable the default behavior, uncheck the box.
</chunk>

### Branch Filter

Read more in [Branch Filter](branch-filter.md).

### Trigger Rules and Branch Filter Combined

Trigger rules and branch filter are combined by __AND__, which means that the build is triggered only __when both conditions are satisfied__.

For example, if you specify a comment text in the trigger rules field and provide the branch specification, the build will be triggered only if a commit has the special text and is also in a branch matched by branch filter. 