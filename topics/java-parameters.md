[//]: # (title: Java Parameters)
[//]: # (auxiliary-id: Java Parameters)

This page describes general parameters for Java-based runners:

* [Ant](ant.md)
* [Maven](maven.md)
* [Gradle](gradle.md)
* [Duplicates Finder (Java)](duplicates-finder-java.md)  
* [Inspections (IntelliJ IDEA)](inspections.md)
* [IntelliJ IDEA Project](intellij-idea-project.md)
* [Kotlin Script](kotlin-script.md)

<chunk include-id="java-param">

<table><tr>

<td>

Option

</td>

<td>

Description

</td></tr><tr>

<td>

JDK

</td>

<td>

Select a JDK. [This section](predefined-build-parameters.md#Defining+Java-related+Environment+Variables) details the available options. The default is `JAVA_HOME` environment variable or the agent's own Java.

</td></tr><tr>

<td>

JDK home path

</td>

<td>

_The option is available when &lt;Custom&gt; is selected above._ Use this field to specify the path to your custom JDK used to run the build. If the field is left blank, the path to JDK Home is read either from the `JAVA_HOME` environment variable on the agent machine, or from the `env.JAVA_HOME` property specified in the [build agent configuration](build-agent-configuration.md) file (`buildAgent.properties`). If these values are not specified, TeamCity uses the Java home of the build agent process itself.

</td></tr><tr>

<td>

JVM command line parameters

</td>

<td>

You can specify such JVM command line parameters, for example, _maximum heap size_ or parameters enabling _remote debugging_. These values are passed by the JVM used to run your build.

Example:

```Shell
-Xmx512m -Xms256m

```

</td></tr></table>

</chunk>