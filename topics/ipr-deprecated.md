[//]: # (title: Ipr (deprecated))
[//]: # (auxiliary-id: Ipr (deprecated))
This runner provides ability to build [IntelliJ IDEA](http://www.jetbrains.com/idea/) projects in TeamCity.   
It is superseded by [IntelliJ IDEA Project](intellij-idea-project.md) runner. 

This page contains reference information about the __IPR__ build runner fields.

## Ipr Runner Deprecation

Since TeamCity 6.0 Ipr runner is deprecated in favor of [IntelliJ IDEA project](intellij-idea-project.md) runner which uses another implementation approach. In one of the following major TeamCity releases all build configurations with Ipr runner will be automatically converted to IntelliJ IDEA project runner. Since the runners may function differently in specific configurations it is highly recommended to change your current Ipr runner\-based configurations to the new runner and check your settings before the final Ipr runner disabling. Please also use the IntelliJ IDEA project runner for all newly created projects and let us know if you have any issues with it.

Apart from differences in the scope of supported IntelliJ IDEA project features, the runners are also different in approach to tests running and coverage.   
Namely:
	
* EMMA coverage is not supported by IntelliJ IDEA project runner. We recommend migrating to IntelliJ IDEA coverage engine if you used EMMA
	
* in IntelliJ IDEA project runner JUnit tests are launched via IntelliJ IDEA shared run configurations as opposed to Ant's `<junit>` task in Ipr runner.


Here are the recommended steps to perform the migration from Ipr to IntelliJ IDEA project runner:
	
1. If your existing Ipr runner has [JUnit Test Runner Settings](#Additional+Pre%2FPost+Processing+%28Ant%29) configured, backup all the settings of the section, for example, into a text file.
2. If you have [code coverage settings](#JUnit+Test+Runner+Settings) configured, save these settings also. (See also related [issue](http://youtrack.jetbrains.net/issue/TW-14921))
3. Change the runner type to IntelliJ IDEA Project. All your settings will be migrated except for JUnit and code coverage options.
4. To restore JUnit tests you will need to create a shared run configuration in IntelliJ IDEA and commit the corresponding file into the version control. The name of the run configuration can then be specified in the __Run configurations to execute__ area.
5. For coverage, configure code coverage options anew using your saved settings.

## IntelliJ IDEA Project Settings

<include src="intellij-idea-project.md" include-id="idea-project-settings"/>

## Additional Pre/Post Processing (Ant)



<table>
<tr>


<td>

 Option 


</td>


<td>

Description 


</td>
</tr>
<tr>


<td>

Run before build 


</td>


<td>

In the appropriate fields, enter the Ant scripts and targets (optional) that you want to run prior to starting the build. The path to the Ant file should be relative to the project root directory. 


</td>
</tr>
<tr>


<td>

Run after build 


</td>


<td>

 In the appropriate fields, enter the Ant scripts and targets (optional) that you want to run after the build is completed. The path to the Ant file should be relative to the project root directory. 


</td>
</tr>
</table>




## JUnit Test Runner Settings



<tip>

JUnit test settings map to the attributes of JUnit task. For details, refer to [Apache documentation](https://ant.apache.org/manual/Tasks/junit.html).
</tip>

<table>
<tr>


<td>

Option 


</td>


<td>

Description 


</td>
</tr>
<tr>


<td>

Test patterns 


</td>


<td>

Click the __Type test patterns__ link, and specify the required test patterns in a text area.   
These patterns are used to generate parameters of the `batchtest` JUnit task section. Each pattern generates either `include` or `exclude` section. These patterns are also used to compose classpath for the test run. Each module mentioned in the patterns adds its classpath to the whole classpath.   
Each pattern should be placed on a separate line and has the following format:



```Plain Text
[-]moduleName:[testFileNamePattern]

```


where:
* `[-]`: If a pattern starts with a minus character, the corresponding files will be excluded from the build process.
* `moduleName`: this name can contain wildcards.
* `[testFileNamePattern]`: Default value for `testFileNamePattern` is `**/*Test.java`, i.e. all files ending with Test.java in all directories. You can use [Ant syntax](http://ant.apache.org/manual/CoreTypes/patternset.html) for file patterns. The sample below includes all test files from modules ending with "test" and excludes all files from packages containing the "ui" subpackage:

```Plain Text
*test:**/*Test.java
-*:**/ui/**/*.java

```

</td>
</tr>
<tr>


<td>

Search for tests 


</td>


<td>

In IDEA project, a user can mark a source code folder as either _`sources`_ or _`test`_ root. This drop\-down list allows you to specify directories to look for tests: 


* __Throughout all project sources__: look for tests in both `sources` and `test` folders of your IDEA project.
* __In test sources only__: look through the folders marked as tests root only.




</td>
</tr>
<tr>


<td>

 Classpath in Tests 


</td>


<td>

By default the whole classpath is composed of all classpaths of the modules used to get tests from. The following two options define whether you will use the default classpath, or take it from the specified module.  


</td>
</tr>
<tr>


<td>

Override classpath in tests 


</td>


<td>

If this option is checked, you can define test classpath from a single, explicitly specified module. 


</td>
</tr>
<tr>


<td>

Module name to use JDK and classpath from 


</td>


<td>

If the option __Override classpath in tests__ is checked, you have to specify the module, where the classpath to be used for tests is specified.  


</td>
</tr>
<tr>


<td>

JUnit Fork mode  


</td>


<td>

Select the desired fork mode from the combo box:

* __Do not fork__: fork is disabled.
* __Fork per test__: fork is enabled, and each test class runs in a separate JVM
* __Fork once__: fork is enabled, and all test classes run in a single JVM


</td>
</tr>
<tr>


<td>

New classloader instance for each test 


</td>


<td>

Check this option, if you want a new classloader to be instantiated for each test case. This option is available only if __Do not fork__ option is selected. 


</td>
</tr>
<tr>


<td>

Include Ant runtime 


</td>


<td>

Check this option to add Ant classes, required to run JUnit tests. This option is available if fork mode is enabled (__Fork per test__ or __Fork once__). 


</td>
</tr>
<tr>


<td>

JVM executable 


</td>


<td>

Specify the command that will be used to invoke JVM. This option is available if fork mode is enabled (__Fork per test__ or __Fork once__). 


</td>
</tr>
<tr>


<td>

Stop build on error 


</td>


<td>

Check this option, if you want the build to stop if an error occurs during test run. 


</td>
</tr>
<tr>


<td>

JVM command line parameters for JUnit 


</td>


<td>

Specify JVM parameters to be passed to JUnit task. 


</td>
</tr>
<tr>


<td>

Tests working directory 


</td>


<td>

Specify the path to the working directory for tests. 


</td>
</tr>
<tr>


<td>

Tests timeout 


</td>


<td>

Specify the lapse of time in milliseconds, after which test will be canceled. This value is ignored, if __Do not fork__ option is selected. 


</td>
</tr>
<tr>


<td>

Reduce test failure feedback time 


</td>


<td>

Use following two options to instruct TeamCity to run some tests before others. 


<tip>

Tests reordering works the following way: TeamCity provides tests that should be run first (test classes), after that when a JUnit task starts, it checks whether it includes these tests. If at least one test is included, TeamCity generates a new fileset containing included tests only and processes it before all other filesets. It also patches other filesets to exclude tests added to the automatically generated fileset. After that JUnit starts and runs as usual.
</tip>



</td>
</tr>
<tr>


<td>

Run recently failed tests first 


</td>


<td>

If checked, in the first place TeamCity will run tests failed in previous finished or running builds as well as tests having high failure rate (a so called _blinking_ tests) 


</td>
</tr>
<tr>


<td>

Run new and modified tests first 


</td>


<td>

If checked, before any other test, TeamCity will run tests added or modified in change lists included in the running build. 


<note>

If both options are enabled at the same time, tests of the __new and modified tests__ group will have higher priority, and will be executed in the first place.
</note>
 

</td>
</tr>
<tr>


<td>

 Verbose Ant 


</td>


<td>

Check this option, if the generated JUnit task has to produce verbose output in ant terms. 


</td>
</tr>
</table>

## Code Coverage


To learn about configuring code coverage options, refer to the [Configuring Java Code Coverage](configuring-java-code-coverage.md) page.

__ __