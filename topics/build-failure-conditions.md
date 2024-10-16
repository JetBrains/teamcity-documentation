[//]: # (title: Build Failure Conditions)
[//]: # (auxiliary-id: Build Failure Conditions)

TeamCity allows changing the conditions under which a build is marked as _failed_. You can adjust these _build failure conditions_ in __[Build Configuration Settings](creating-and-editing-build-configurations.md) | Failure Conditions__

>To fail a build if sufficient disk space cannot be freed for the build, see the [Free disk space](free-disk-space.md) build feature.
>
{style="tip"}

## Common Build Failure Conditions

In the __Common Failure Conditions__ block, you can specify how exactly TeamCity will fail builds.

### Set Custom Build Execution Timeout

The _if it runs longer than ... minutes_ condition allows defining an execution timeout for a build, in minutes. If the specified amount of time is exceeded, the build is automatically canceled. This option helps deal with hanging builds and maintains agent efficiency.

If the configuration's timeout is 0 (the default value), the global [server-wide](teamcity-configuration-and-maintenance.md#Build+Settings) timeout applies. Otherwise, the timeout specified in this configuration overrides the global value specified in __Administration | Global Settings__.

### Fail Build if Exit Code is not Zero

The _if the build process exit code is not zero_ condition allows failing a build if the build process doesn't exit successfully.

### Fail Build on Test Fail

The _if at least one test failed_ condition marks the build as failed if at least one its test fails. If this option is disabled, the build can be marked successful even if it fails to pass a number of tests. Regardless of this option, TeamCity will run all build steps.

<anchor name="test-retry"/>
If the _support test retry: successful test run mutes previous test failure_ option is enabled, TeamCity will mute a test if it fails and then succeeds within the same build. Such tests will not affect the build status. This is convenient for configurations with flaky tests that alternately fail and succeed when applied to the same source revision. 

### Fail Build on Error Message

The _if an error message is logged by build runner_ condition marks the build as failed if the build runner reports an error while building.

### Fail Build on Out-of-Memory Crash

The _if an out-of-memory problem or crash is detected (Java only)_ condition marks the build as failed if a crash of the JVM is detected, or Java has out of memory problems. If possible, TeamCity will upload crash logs and memory dumps as artifacts for such builds.

<anchor name="BuildFailureConditions-AdditionalFailureConditions"/>

## Additional Failure Conditions

You can instruct TeamCity to mark a build as failed if some of its metrics (for example, code coverage or artifacts size) have changed compared to another build. For instance, you can mark a build as failed if the code duplicates number is higher than in the previous build.

Another build failure condition causes TeamCity to mark build as failed when a certain text is present in the build log.

To add such failure condition, click __Add build failure condition__ and select from the list:
* [Fail build on metric change](#Fail+Build+on+Metric+Change)
* [Fail build on specific text in build log](#Fail+Build+on+Specific+Text+in+Build+Log)

>You can disable a build failure condition temporarily or permanently at any time, even if it is inherited from a build configuration template.
>
{style="tip"}

<anchor name="BuildFailureConditions-Failbuildonmetricchange"/>

### Fail Build on Metric Change

When your build uses code examining tools like code coverage, duplicates finders, or inspections, it generates various numeric metrics. For these metrics, you can specify a threshold which, when exceeded, will fail a build.

In general, there are two ways to configure this build failure condition:
* A _build metric_ exceeds or is less than the specified constant value (threshold).   
For example, _Fail build if_ its "_build duration (secs)_", compared to the constant value, is "_more_" than "_300_".   
In this case, a build will fail if it runs more than 300 seconds. 
* A _build metric_ has changed comparing to a specific build by a specified value.   
For example, _Fail build if_ its "_build duration (secs)_" compared to a value from another build is "_more_" by at least "_300_" default units for this metric than the value in the "_Last successful build_".   
In this case, a build will fail if it runs 300 seconds longer than the last successful build. If a [branch specification](working-with-feature-branches.md) is configured, the [following logic](working-with-feature-branches.md#Failure+Conditions) is applied.

Values from the following builds can be used as the basis for comparing build metrics:
* last successful build
* last pinned build
* last finished build
* build with specified build number
* last finished build with specified tag.

By default, TeamCity provides the wide range of _build metrics_:
* artifacts size(bytes) — size of artifacts, excluding [internal artifacts](build-artifact.md#Hidden+Artifacts) under the `.teamcity` directory
* build duration (secs) — build duration, excluding time to check out sources
* number of classes
* number of code duplicated
* number of covered classes
* number of covered lines
* number of covered methods
* number of failed tests
* number of ignored tests
* number of inspection errors
* number of inspection warnings
* number of lines of code
* number of methods
* number of passed tests
* number of tests
* percentage of block coverage
* percentage of class coverage
* percentage of line coverage
* percentage of method coverage
* percentage of statement coverage
* test duration (secs)
* total artifacts size (bytes) — size of all artifacts including [internal ones](build-artifact.md#Hidden+Artifacts)

### Adding Custom Build Metric

You can add your own build metric. There are two ways to do it:

- Use Kotlin DSL to configure a build failure condition [on a custom statistic value](teamcity-info-xml.md#Reporting+Custom+Statistics)
reported by the build.

   The sample Kotlin DSL code is as follows:

   ```Kotlin
     failureConditions {
     failOnMetricChange {
       param("metricKey", "myReportedCustomStatisticValue")
       ....
     }
   }
   ```

- Modify the TeamCity configuration file `<[TeamCity_Data_Directory](teamcity-data-directory.md)>/config/main-config.xml`, 
which requires system administrator privileges. 
   Add the following section to your `main-config.xml` under the `server` node:

   ```XML
   <build-metrics>
     <statisticValue key="myMetric" description="build metric for number of files"/>
   </build-metrics>

   ```

If your build publishes the `myMetric` value, you can use it as a criterion for a build failure.

<anchor name="BuildFailureConditions-Failbuildonspecifictextinbuildlog"/>

### Fail Build on Specific Text in Build Log

TeamCity can inspect all lines in a build log for some particular text occurrence that indicates a build failure. When matching lines, the time and block name prefixes preceding each log message are ignored.

To configure this build failure condition, specify:
* a string or a [Java Regular Expression](https://java.sun.com/javase/6/docs/api/java/util/regex/Pattern.html) whose presence/absence in the build log is an indicator of a build failure,
* a failure message to be displayed on the __Build Results__ page when a build fails due to this condition.

When using Regex, you can refer to the regular expression or its parts from the "_Failure message_" field (for example, refer to specific [capturing groups](https://www.regular-expressions.info/brackets.html) with `$N` where `N` is the group number). This allows including the matching error text into the failure message displayed in the build results.

TeamCity reports a build problem only for the first text occurrence found in a build log. To report a problem on each found match instead, disable the option _Create build problem only on the first match_. This might be helpful if a build tool sends multiple different errors matching the searched pattern and you want to get a problem report for each of them.

### Stopping Build Immediately

You can stop a build immediately on encountering a specified text in the build log or when a certain build metric, specified using the _Fail build on metric change_ condition, is exceeded.

<seealso>
    <category ref="external">
        <a href="https://youtu.be/fttWwJG7C38">Video tutorial: Improving your first build configuration</a>
    </category>
        
</seealso>
