[//]: # (title: Gradle)
[//]: # (auxiliary-id: Gradle)

The _Gradle_ build runner runs [Gradle](https://www.gradle.org) projects.

To run builds with Gradle, Gradle 0.9-rc-1 or later must be installed on all the agent machines. Alternatively, if you use the [Gradle wrapper](https://docs.gradle.org/3.3/userguide/gradle_wrapper.html), you need to have properly configured Gradle Wrapper scripts checked in to your Version Control.

The runner supports all Gradle build configurations, including `build.gradle` and `build.gradle.kts`.

>To use the TeamCity server as an external dependency repository for Gradle builds, try the external [TeamCity-Gradle plugin](https://github.com/jk1/TeamCity-dependencies-gradle-plugin).

<anchor name="Gradle-GradleParameters"/>

## Gradle Parameters

<table><tr>

<td>

Option

</td>

<td>

Description

</td></tr><tr>

<td>

Gradle tasks

</td>

<td>

Specify Gradle task names separated by spaces. For example, `:myproject:clean :myproject:build` or `clean build`. If this field is left blank, the 'default' task is used. Note that TeamCity currently supports building Java projects with Gradle. Building Groovy, Scala, and other projects has not been tested.

</td></tr>

<tr>

<td>

Gradle build file

</td>

<td>

A path to the [Gradle build file](https://docs.gradle.org/current/userguide/tutorial_using_tasks.html#sec:hello_world), relative to the working directory. If empty (default), Gradle uses own settings to determine it.

</td></tr>

<tr>

<td>

Incremental building

</td>

<td>

TeamCity can make use of the Gradle `:buildDependents` [feature](https://www.gradle.org/docs/current/userguide/userguide_single.html#sec:multiproject_build_and_test). If the _Incremental building_ option is enabled, TeamCity will detect Gradle modules affected by changes in the build and start the `:buildDependents` command for them only. This will cause Gradle to fully build and test only the modules affected by changes.

</td></tr><tr>

<td>

Gradle home path

</td>

<td>

Specify here the path to the Gradle home directory (the parent of the `bin` directory). If not specified, TeamCity will use Gradle specified in the agent's `GRADLE_HOME` environment variable. If you don't have Gradle installed on agents, you can use a Gradle wrapper instead.

</td></tr><tr>

<td>

Additional Gradle command line parameters

</td>

<td>

Optionally, specify the space-separated list of command line parameters to be passed to Gradle.

</td></tr><tr>

<td>

Gradle Wrapper

</td>

<td>

If enabled, TeamCity will look for Gradle Wrapper scripts in the checkout directory, and launch the appropriate script with Gradle tasks and additional command line parameters specified in the fields above. In this case, Gradle specified in _Gradle home path_ and Gradle installed on the agent are ignored.

</td></tr></table>

<anchor name="LaunchingParameters"/>
<anchor name="Gradle-LaunchingParameters"/>

### Run Parameters
[//]: # (AltHead: LaunchingParameters cbr) 

<table><tr>

<td>

Option

</td>

<td>

Description

</td></tr><tr>

<td>

Debug

</td>

<td>

Selecting the _Log debug messages_ checkbox is equivalent to adding the `-d` Gradle command-line parameter.

>Running Gradle with the `DEBUG` log level can potentially expose sensitive information in the build log (learn more in the [Gradle documentation](https://docs.gradle.org/current/userguide/logging.html#sec:debug_security)). Before enabling this mode, make sure that the log can be viewed only by trusted users.
> 
{type="warning"}

</td></tr><tr>

<td>

Stacktrace

</td>

<td>

Selecting the _Print stacktrace_ checkbox is equivalent to adding the `-s` Gradle command-line parameter.

</td></tr></table>

### Java Parameters

<include src="java-parameters.md" include-id="java-param"/>

### Build properties

In Gradle builds, TeamCity system properties are different from Java system properties.

* Regular Java system properties can be accessed globally. Use the `System.getProperty("my.property")` or `providers.systemProperty("my.property").get()` methods to obtain these properties' values.

* TeamCity system properties are written to the [Project](https://docs.gradle.org/current/dsl/org.gradle.api.Project.html) object when a build initializes. Therefore, TeamCity system properties can be accessed anywhere the `Project` is available (use `project.hasProperty("property.name")` to check whether the required property is available).

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

### Docker Settings

In this section, you can specify a Docker image which will be [used to run the build step](container-wrapper.md).

<anchor name="coverage"/>

### Code Coverage
[//]: # (AltHead: coverage)

The Gradle build runner supports code coverage with based on the [IDEA code coverage engine](intellij-idea.md) and [JaCoCo](jacoco.md).

<seealso>
        <category ref="admin-guide">
            <a href="intellij-idea.md">IntelliJ IDEA Code Coverage</a>
        </category>
</seealso>
