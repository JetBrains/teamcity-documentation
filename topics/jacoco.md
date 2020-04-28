[//]: # (title: JaCoCo)
[//]: # (auxiliary-id: JaCoCo)
TeamCity supports [JaCoCo](http://www.eclemma.org/jacoco), a Java Code Coverage tool allowing you to measure a wide set of coverage metrics and code complexity.

JaCoCo is available for the following build runners: [Ant](ant.md), [IntelliJ IDEA Project](intellij-idea-project.md), [Gradle](gradle.md), and [Maven](maven.md).

TeamCity 2019.2 offers three bundled versions of JaCoCo:
* 0.7.5 (default)
* 0.8.2
* 0.8.4

<note>

To ensure the coverage data is collected properly, make sure your tests run in (one or more) separate JVMs.
* Ant and Intellij Idea Project runners: this is the default setting for [TestNG](http://testng.org/doc/ant.html), for [Junit test task](http://ant.apache.org/manual/Tasks/junit.html), set `fork=true`.
* Maven runner: set `forkCount` to a value [higher than](http://maven.apache.org/surefire/maven-surefire-plugin/examples/fork-options-and-parallel-execution.html).
* Gradle runner: this is the default setting for [Gradle tests](https://gradle.org/docs/current/dsl/org.gradle.api.tasks.testing.Test.html).

</note>

## Enabling JaCoco coverage

TeamCity supports the java agent coverage mode allowing you to collect coverage without modifying build scripts or binaries. No additional build steps needed \- just choose JaCoCo coverage in a build step which runs tests:

1. In the __Code Coverage__ section, select __JaCoCo__ as a coverage tool in the __Choose coverage runner__ drop\-down.
2. Set up the coverage options \- refer to the description of the available options below.

<table><tr>

<td>


Option

</td>

<td>

Description


</td>

<td>


Example

</td></tr><tr>

<td>


__Classfile directories or jars__

</td>

<td>


Newline\-delimited set of path patterns in the form of `+|-:[path]` relative to the [checkout directory](build-checkout-directory.md) to scan for classfiles to be analyzed. Libraries and test classfiles don't have to be listed unless their coverage is wanted.

<include src="branch-filter.md" include-id="OR-syntax-tip"/>

</td>

<td>


`+:target/main/java/**`

</td></tr><tr>

<td>


__Classes to instrument__

</td>

<td>


Newline\-delimited set of classname patterns in the form of `+|-:[path]`. Allows filtering out unwanted classes listed in "__Classfile directories or jars__" field. Useful in case test classes are compiled.

</td>

<td>


`+:com.package.core.*`

`-:com.package.*Test*`

</td></tr></table>

<tip>

By default, in TeamCity the jacoco.sources property is set to "." , which means that TeamCity will scan whole checkout directory including all subdirectories for your sources.Check that your classfiles are compiled with debug information (including the source file info) to see with highlighted source code in the report.

</tip>

The code coverage results can be viewed on the __Overview__ tab of the Build Results page; detailed report is displayed on the dedicated __Code Coverage__ tab.

## Importing JaCoCo coverage data to TeamCity

TeamCity can parse JaCoCo coverage data and generate a report using a [service message](build-script-interaction-with-teamcity.md#Service+Messages) of the following format:


```Plain Text
##teamcity[jacocoReport dataPath='<path to jacoco.exec file>']

```

<table><tr>

<td>

Attribute name


</td>

<td>


Description

</td>

<td>


Default value

</td>

<td>


Example

</td></tr><tr>

<td>

dataPath


</td>

<td>


Space\-delimited set of paths relative to the checkout directory to read the jacoco data file

</td>

<td>


</td>

<td>


`jacocoResults/jacoco.exec jacocoResults/anotherJacocoRun.exec`

</td></tr><tr>

<td>

includes

</td>

<td>


Space\-delimited set of classname include patterns

</td>

<td>

`*`


</td>

<td>

`com.package.core.* com.package.api.*`


</td></tr><tr>

<td>

excludes


</td>

<td>

Space\-delimited set of classname exclude patterns


</td>

<td>

 


</td>

<td>

`com.package.test.* .*Test`

</td></tr><tr>

<td>

sources


</td>

<td>

Space\-delimited set of paths relative to the checkout directory to read sources from. Does not need to be listed by default.


</td>

<td>

`.`


</td>

<td>

`src`


</td></tr><tr>

<td>

classpath


</td>

<td>

Space\-delimited set of path patterns in the form of `+|-:[path]` to scan for classfiles to be analyzed. Libraries and test classfiles do not need to be listed unless their coverage is wanted.


</td>

<td>

`+:**/*`


</td>

<td>

`+:target/main/java/**`


</td></tr><tr>

<td>

reportDir


</td>

<td>

Path to the directory to store temporary files. The report will be generated as coverage `.zip` under this directory. Check that there is no existing directory with the same name.


</td>

<td>

A random directory under Agent's temp directory


</td>

<td>

jacocoReport


</td></tr></table>

An example of a complete service message:


```Plain Text
##teamcity[jacocoReport dataPath='jacoco.exec' includes='com.package.core.*' classpath='classes/lib/some.jar' reportDir='temp/jacocoReport']

```

__ __