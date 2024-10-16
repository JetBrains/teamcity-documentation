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

<snippet id="java-param">

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

Select a JDK. [This section](predefined-build-parameters.md#Defining+Java-Related+Environment+Variables) details the available options. The default is `JAVA_HOME` environment variable or the agent's own Java.

</td></tr><tr>

<td>

JDK home path

</td>

<td>

_The option is available when &lt;Custom&gt; is selected above._ Use this field to specify the path to your custom JDK used to run the build. If the field is left blank, the path to JDK Home is read either from the `JAVA_HOME` environment variable on the agent machine, or from the `env.JAVA_HOME` property specified in the [build agent configuration](configure-agent-installation.md) file (`buildAgent.properties`). If these values are not specified, TeamCity uses the Java home of the build agent process itself.

</td></tr><tr>

<td>

JVM command line parameters

</td>

<td>

Additional JVM command line parameters allow you to set initial and maximum heap sizes, enable additional logging, select the required bytecode verifier mode, and more.

You can specify both standard (begin with `-`, for instance `-verbose:[class|module|gc|jni]` or `--dry-run`) and non-standard (begin with `-X`, for instance `-Xmx<size>` or `-XstartOnFirstThread`) JVM options.

To specify multiple command line parameters, use space as a separator. For example:

```Shell
-verbose:gc -Xdiag -Xcomp -Xmx512m -Xms256m
```

</td></tr></table>

</chunk>