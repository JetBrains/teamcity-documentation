[//]: # (title: Gradle)
[//]: # (help-id: Gradle)

<primary-label ref="primary-step-pipeline"/>

<show-structure for="chapter" depth="2"/>


This build step is tailored to build [Gradle](https://www.gradle.org) projects and supports all Gradle build configurations, including `build.gradle` and `build.gradle.kts`.

> TeamCity can also serve as an external dependency repository for Gradle builds. Try the external [TeamCity-Gradle plugin](https://github.com/jk1/TeamCity-dependencies-gradle-plugin) to enable this functionality.


## Prerequisites


To run builds with Gradle, Gradle 0.9-rc-1 or later must be installed on all the agent machines. Alternatively, if you use the [Gradle wrapper](https://docs.gradle.org/3.3/userguide/gradle_wrapper.html), you need to have properly configured Gradle Wrapper scripts checked in to your Version Control.


## Step Settings

The list of Gradle step settings and their corresponding UI labels slightly differ depending on whether you configure a build configuration or a pipeline.

### Main Settings

<deflist type="medium">

<def title="Tasks">

The list of space-separated Gradle tasks this step should perform. For example, `:myproject:clean :myproject:build` or `clean build`. If this field is left blank, the `default` task is used. Note that TeamCity currently supports building Java projects with Gradle. Building Groovy, Scala, and other projects has not been tested.

Additional [task options](https://docs.gradle.org/current/userguide/command_line_interface.html#sec:disambiguate_task_options_from_built_in_options) should also be entered in this field. For example, `:myproject:run --args="foo --bar"` or `clean test --tests MyTestClass.myTestMethod`.

</def>

<def title="Working directory">

<include from="common-templates.md" element-id="step-settings-working-dir"/>

</def>

<def title="Use Gradle wrapper">

If enabled, TeamCity will look for Gradle Wrapper scripts in the checkout directory, and launch the appropriate script with Gradle tasks and additional command line parameters specified in corresponding step settings. In this case, Gradle specified in **Gradle home** path and Gradle installed on the agent are ignored.

</def>

</deflist>

### Additional Settings

<deflist type="medium">

<def title="Gradle home">

The path to the Gradle home directory (the parent of the `bin` directory). If not specified, TeamCity will use Gradle specified in the agent's `GRADLE_HOME` environment variable. If you don't have Gradle installed on agents, you can use a Gradle wrapper instead.

</def>

<def title="Build file">

A path to the [Gradle build file](https://docs.gradle.org/current/userguide/tutorial_using_tasks.html#sec:hello_world), relative to the working directory. If empty (default), Gradle uses own settings to determine it.

> To specify a build file for Gradle 9.0 and higher, add the `-p <path to build file relative to the checkout directory>` line to the **Additional Gradle command line parameters** field instead of using this setting.
>
{style="note"}

</def>

<def title="Additional Gradle command line parameters">

The optional space-separated list of [Gradle properties](https://docs.gradle.org/current/userguide/build_environment.html#sec:gradle_configuration_properties). For example, `-x test` (or `--exclude-task test`), `--configuration-cache`, or `-PmyProjectProperty=foo`.

</def>

<def title="Gradle wrapper path">

Optional path to the Gradle wrapper script relative to the working directory.

</def>

<def title="Incremental building">

<tip>Available only for classic build configuration steps.</tip>


TeamCity can make use of the Gradle `:buildDependents` [feature](https://www.gradle.org/docs/current/userguide/userguide_single.html#sec:multiproject_build_and_test). If the _Incremental building_ option is enabled, TeamCity will detect Gradle modules affected by changes in the build and start the `:buildDependents` command for them only. This will cause Gradle to fully build and test only the modules affected by changes.


</def>

</deflist>


### Run Parameters


<deflist type="medium">

<def title="Debug">

Adds the `-d` Gradle command-line parameter.

>Running Gradle with the `DEBUG` log level can potentially expose sensitive information in the build log (learn more in the [Gradle documentation](https://docs.gradle.org/current/userguide/logging.html#sec:debug_security)). Before enabling this mode, make sure that the log can be viewed only by trusted users.
>
{style="warning"}

</def>

<def title="Stacktrace">

Adds the `-s` Gradle command-line parameter.

</def>

</deflist>



### Run in Docker

<include from="common-templates.md" element-id="build-step-run-in-docker"/>


### Code Coverage

<secondary-label ref="secondary-config"/>

The Gradle build runner supports code coverage with based on the [IDEA code coverage engine](intellij-idea.md) and [JaCoCo](jacoco.md).


### Java Parameters

<include from="java-parameters.md" element-id="java-param"/>



## Build properties

In Gradle builds, TeamCity system properties are different from Java system properties.

* Regular Java system properties can be accessed globally. Use the `System.getProperty("my.property")` or `providers.systemProperty("my.property").get()` methods to obtain these properties' values.

* TeamCity [system properties](configuring-build-parameters.md#System+Properties) are written to the [Project](https://docs.gradle.org/current/dsl/org.gradle.api.Project.html) object when a build initializes. Therefore, TeamCity system properties can be accessed anywhere the `Project` is available (use `project.hasProperty("property.name")` to check whether the required property is available).

The recommended way to reference TeamCity system properties is as follows:

<tabs>

```Groovy
task printProperty {
    doLast {
        println "${teamcity['teamcity.build.id']}"
    }
}

```

```Kotlin

tasks.register("printProperty") {
    doLast {
        val teamcity: Map<*,*> by project
        println("${teamcity["teamcity.build.id"]}")
    }
}
```

</tabs>

or if the system property's name is a legal name identifier (for example, `system.myPropertyName = myPropertyValue`):

<tabs>

```Groovy
task printProperty {
     doLast {
          println "$myPropertyName"
     }
}

```

```Kotlin

tasks.register("printProperty") {
    doLast {
        val myPropertyName: String by project
        println("$myPropertyName")
    }
}
```

</tabs>





## Configuration Cache

Starting with version 2024.03, TeamCity Gradle runner supports [configuration cache](https://docs.gradle.org/current/userguide/configuration_cache.html). This feature significantly improves build performance by caching the result of the configuration phase and reusing this cache in subsequent builds.

Configuration cache is enabled if either of the following is true:

* The `--configuration-cache` parameter was added to the runner's **Additional Gradle command line parameters** field.
* A `gradle.properties` file includes the `org.gradle.configuration-cache=true` (for Gradle 8.1+) or `org.gradle.unsafe.configuration-cache=true` (for older Gradle versions) line. This applies to both the project's `gradle.properties` file and the one in the `GRADLE_USER_HOME` directory.

### Current Limitations and Known Issues

Gradle configuration caches may not work as expected in the following cases:

* if virtual builds (those spawned during [parallel testing](parallel-tests.md) or [Matrix Build](matrix-build.md) runs) run in a different order from when the caches were created. See this YouTrack ticket for more information: [TW-86556](https://youtrack.jetbrains.com/issue/TW-86556/Gradle-Configuration-cache-can-be-not-reused-in-virtual-builds-parallel-tests-or-matrix-builds).

* if the [](clean-checkout.md) is enabled;

* if a build step runs within a Docker or Podman container;

* if Gradle [ignores configuration cache problems](https://docs.gradle.org/current/userguide/configuration_cache.html#config_cache:usage:ignore_problems).

* if the list of additional command line arguments includes those unsupported by Gradle Tooling API (`--daemon`, `--stop`, and others).

[Build parameters](configuring-build-parameters.md) whose values always change from build to build (for example, `build.id` or `build.number`) will be loaded only on demand. You can still obtain values of these properties using direct references (for example, `project.teamcity["build.number"]`), but the `findProperty()` method (`project.findProperty("build.number")`) yields no results. If you need to call this method in your Gradle script, use the following workaround:

1. Create a new configuration parameter and map it to the affected parameter: `MyBuildNumber=%\build.number%`.
2. Create a new system property and map it to your new configuration parameter: `system.buildNumber = %\MyBuildNumber%`.
3. Use the `${findProperty}("buildNumber")}` syntax to obtain a required value in your Gradle script.

Note that this workaround prevents your build configuration from reusing the configuration cache, so you may also want to disable it.



<seealso>
        <category ref="admin-guide">
            <a href="intellij-idea.md">IntelliJ IDEA Code Coverage</a>
        </category>
</seealso>
