[//]: # (title: Ant)
[//]: # (auxiliary-id: Ant)

The _Ant_ build runner can process Ant `build.xml` files.

## Testing Frameworks Support

The TeamCity Ant runner supports the JUnit and TestNG frameworks. When tests are run by the `junit` and `testng` tasks directly within the script, TeamCity reports tests on the fly.

By using the `<parallel>` tag in your Ant script, it is possible to have the JUnit and TestNG tasks run in parallel. TeamCity supports this and should concurrently log the parallel processes correctly.

## Reporting and Logging

TeamCity collects detailed data from Ant as to the performed activities, provides structured error reporting, and reports tests. However, you can start a build with no specific reporting or to turn off TeamCity-specific logging:
* To disable TeamCity-specific reporting in Ant, use `teamcity.ant.listener.enabled=false` [build configuration parameter](configuring-build-parameters.md)
* To disable JUnit reporting, use the `teamcity.ant.junit-support.enabled=false` [system property](configuring-build-parameters.md)
* To disable TestNG reporting, use `teamcity.ant.testng-support.enabled=false` [system property](configuring-build-parameters.md)

<anchor name="antAntParamsOptionDescription"/>

<anchor name="Path to build.xml file"/>
<anchor name="Build file content"/>
<anchor name="Working directory"/>
<anchor name="Targets"/>
<anchor name="Ant home path"/>
<anchor name="Additional Ant command line parameters"/>

## Ant Runner Settings

<anchor name="Ant-AntParameters"/>

### Ant Parameters
[//]: # (AltHead: antAntParamsOptionDescription)

<table><tr>

<td>

Option

</td>

<td>

Description

</td></tr><tr>

<td>

Path to build.xml file

</td>

<td>

If you choose the option, you can type the path to an Ant build script file of the project. The path is relative to the project root directory. Alternatively, click ![VCS-browserIcon.png](VCS-browserIcon.png) to choose the file using the VCS repository browser.

</td></tr><tr>

<td>

Build file content

</td>

<td>

If you choose this option, click the __Type build file content__ link and type the source code of your custom build file in the text area. Note that the text area is resizeable. Use the __Hide__ link to close the text area.

</td></tr><tr>

<td>

Working directory

</td>

<td>

Specify the [build working directory](build-working-directory.md) if it differs from the checkout directory.

</td></tr><tr>

<td>

Targets


</td>

<td>

Use this text field to specify valid Ant targets as a list of space-separated strings. The available targets can be viewed in the web UI by clicking the icon next to the field and added by checking the appropriate boxes. If this field is left empty, the default target specified in the build script file will be run.

</td></tr><tr>

<td>

Ant home path


</td>

<td>

Specify the path to the distributive of your custom Ant. You do not need to specify this parameter if you are going to use the Ant distributive bundled with TeamCity.

<note>

Note that you should use Ant 1.7 if you want to run JUnit4 tests in your builds.
</note>

</td></tr><tr>

<td>

Additional Ant command line parameters

</td>

<td>

Optionally, specify additional command line parameters as a space-separated list. For example, you can specify the [ant-net-tasks Tool](#ant-net-tasks+Tool) (see below).

</td></tr></table>

#### ant-net-tasks Tool

The Ant build runner comes with a bundled tool, _ant-net-tasks_, which includes the jar files required for network tasks, such as FTP, sshexec, scp and mail.   
It also contains [missing link Ant task](https://code.google.com/p/missing-link/) which can be used for REST requests.

To use the tool, specify `-lib "%\teamcity.tool.ant-net-tasks%"` in [Additional Ant command line parameters](#Ant+Parameters) of the runner settings.

<anchor name="Ant-JavaParameters"/>

### Java Parameters

<include from="java-parameters.md" element-id="java-param"/>

### Test Parameters

Tests reordering works the following way: TeamCity provides tests that should be run first (test classes), after that, when a JUnit task starts, it checks whether it includes these tests. If at least one test is included, TeamCity generates a new fileset containing included tests only and processes it before all other filesets. It also patches other filesets to exclude tests added to the automatically generated fileset. After that JUnit starts and runs as usual.

<table><tr>

<td>

Option


</td>

<td>

Description

</td></tr><tr>

<td>

Reduce test failure feedback time:

</td>

<td>

Use the following two options to instruct TeamCity to run some tests before others.

* Run recently failed tests first — if checked,TeamCity will first run tests failed in the previous finished or running builds as well as tests having a high failure rate (so called _blinking_ tests).
* Run new and modified tests first — if checked, TeamCity will first run the tests added or modified in the change lists included in the running build.

</td></tr></table>

<note>

If both options are enabled at the same time, the tests of the __new and modified tests__ group will have higher priority, and will be executed first.
</note>

### Docker Settings

In this section, you can specify a Docker image which will be [used to run the build step](container-wrapper.md).

### Code Coverage

To learn about configuring code coverage options, refer to the [Configuring Java Code Coverage](configuring-java-code-coverage.md) page.
 


## JUnit 5 Support

TeamCity Ant runner does not currently display tests under the "Tests" build result tab if these tests are declared in `<junitlauncher>` tasks. Use `<junit>` tasks instead or migrate your project to Gradle (the TeamCity [Gradle runner](gradle.md) fully supports JUnit 5).

<seealso>
        <category ref="admin-guide">
            <a href="configuring-java-code-coverage.md">Configuring Java Code Coverage</a>
        </category>
</seealso>