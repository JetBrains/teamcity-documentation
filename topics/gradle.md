[//]: # (title: Gradle)
[//]: # (auxiliary-id: Gradle)

_The Gradle Build Runner_ runs [Gradle](http://www.gradle.org) projects.

<note>

To run builds with Gradle, you need to have Gradle 0.9\-rc\-1 or higher installed on all the agent machines. Alternatively, if you use the [Gradle wrapper](https://docs.gradle.org/3.3/userguide/gradle_wrapper.html), you need to have properly configured Gradle Wrapper scripts checked in to your Version Control.
</note>


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

Specify Gradle task names separated by spaces. For example: `:myproject:clean :myproject:build` or `clean build`. If this field is left blank, the 'default' task is used. Note that TeamCity currently supports building Java projects with Gradle. Building Groovy/Scala/etc. projects has not been tested.


</td></tr><tr>

<td>

Incremental building


</td>

<td>

TeamCity can make use of the Gradle `:buildDependents` [feature](http://www.gradle.org/docs/current/userguide/userguide_single.html#sec:multiproject_build_and_test). If the __Incremental building__ checkbox is enabled, TeamCity will detect Gradle modules affected by changes in the build, and start the `:buildDependents` command for them only. This will cause Gradle to fully build and test only the modules affected by changes.


</td></tr><tr>

<td>

Gradle home path


</td>

<td>

Specify here the path to the Gradle home directory (the parent of the `bin` directory). If not specified, TeamCity will use the Gradle from an agent's `GRADLE_HOME` environment variable. If you don't have Gradle installed on agents, you can use Gradle wrapper instead.


</td></tr><tr>

<td>

Additional Gradle command line parameters


</td>

<td>

Optionally, specify the space\-separated list of command line parameters to be passed to Gradle.


</td></tr><tr>

<td>

Gradle Wrapper


</td>

<td>

If this checkbox is selected, TeamCity will look for Gradle Wrapper scripts in the checkout directory, and launch the appropriate script with Gradle tasks and additional command line parameters specified in the fields above. In this case, the Gradle specified in __Gradle home path__ and the one installed on agent, are ignored.


</td></tr></table>

<anchor name="LaunchingParameters"/>

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

Selecting the __Log debug messages__ checkbox is equivalent to adding the `-d` Gradle command line parameter.


</td></tr><tr>

<td>

Stacktrace


</td>

<td>

Selecting the __Print stacktrace__ check box is equivalent to adding the `-s` Gradle command line parameter.


</td></tr></table>

<include src="ant.md" include-id="java-param"/>

### Build properties

The TeamCity system parameters can be accessed in Gradle build scripts in the [same way](upgrade-notes.md#Gradle%3A+Breaking+change+compared+to+9.1.2) as Gradle properties. The recommended way to reference properties is as follows:


```Shell
task printProperty << {
    println "${project.ext['teamcity.build.id']}"
}

```

or if the system property's name is a legal Groovy name identifier (for example, `system.myPropertyName = myPropertyValue`):


```Shell
task printProperty << {
     println "$myPropertyName"
}

```



### Docker Settings

In this section, you can specify a Docker image which will be [used to run the build step](docker-wrapper.md).

<anchor name="coverage"/>

### Code Coverage
[//]: # (AltHead: coverage)

Code coverage with [IDEA code coverage engine](intellij-idea.md) and [JaCoCo](jacoco.md) is supported.

__  __

__See also:__

__Administrator's Guide__: [IntelliJ IDEA Code Coverage](intellij-idea.md)

__ __