[//]: # (title: Simple Build Tool \(Scala\))
[//]: # (auxiliary-id: viewpage.actionpageId113084133;Simple Build Tool \(Scala\))

The _Simple Build Tool (Scala)_ build runner natively supports [SBT](http://www.scala-sbt.org/) builds: you can build your code, run tests, and see the results in a handy way in TeamCity.

TeamCity supports SBT versions up to 1.4.x.

## SBT runner settings

### SBT parameters

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

SBT commands 

</td>

<td>

Commands to execute, for example, `clean "set scalaVersion:="2.11.6"" compile test` or `;clean;set scalaVersion:="2.11.6";compile;test`. 

</td>
</tr>
<tr>


<td>

SBT installation mode 

</td>


<td>

When the default `<Auto>` option is selected, the bundled SBT version will be installed on every TeamCity agent your build will be running. To specify an existing installation, use the `<Custom>` mode. The `sbt-launch.jar` from the `\bin` directory of the SBT home will be launched.

</td>
</tr>
<tr>


<td>

SBT home path

</td>

<td>

_Available if `<Custom>` is selected in the option above._ The path to the existing SBT installation directory.

</td>
</tr>
<tr>


<td>

Working directory 

</td>


<td>

Optional. Specify the [Build Working Directory](build-working-directory.md) if it differs from the [Build Checkout Directory](build-checkout-directory.md).

</td>
</tr>
</table>

### Java Parameters

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

JDK

</td>

<td>

When `<Default>` is selected, the JDK specified in the `JAVA_HOME` environment variable on the agent or the agent's own Java is used to run the build process. Set to `<Custom>` to use a custom JDK.

</td>
</tr>
<tr>

<td>

JDK home path 

</td>


<td>

_Available if `<Custom>` is selected in the option above._ Specify the path to your custom JDK which will be used to run the build.

</td>
</tr>
<tr>

<td>

JVM command line parameters

</td>


<td>

Specify the desired Java Virtual Machine parameters, such as maximum heap size or parameters that enable remote debugging. These settings are passed to the JVM used to run your build.   
 Example:

```Java
-Xmx512m -Xms256m

```

</td>
</tr>
</table>
