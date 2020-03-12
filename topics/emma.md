[//]: # (title: EMMA)
[//]: # (auxiliary-id: EMMA)

## EMMA Integration Notes

The following steps are performed when collecting coverage with EMMA:
	
1\. After each compilation step (with javac/javac2), the build agent invokes EMMA to instrument the compiled classes and to store the location of the source files. As a result, the `coverage.em` file containing the classes metadata is created in the build checkout directory. The collected source paths of the Java files are used to generate the final HTML report.   

<note>

All `coverage.*` files are removed in the beginning of the build, so you have to ensure that full recompilation of sources is performed in the build to have the actual `coverage.em` file.
</note>

2\. Test run. At this stage, the actual runtime coverage information is collected. This process results in creation of the `coverage.ec` file. If there are several test tasks, data is appended to `coverage.ec`.

3\. Report generation. When the build ends, TeamCity generates an HTML coverage report, creates the `coverage.zip` file with the report and uploads it to the server. It also generates and uploads the summary report in the `coverage.txt` file, and the original `coverage.e(c|m)` files to allow viewing coverage from the TeamCity plugin for IntelliJ IDEA.


## Configuring Coverage with EMMA

To configure code coverage by means of EMMA engine, follow these steps:
	
1. While creating/editing Build Configuration, go to the __Build Step__ page.
2. Select the [Ant](ant.md) build runner.
3. In the __Code Coverage__ section, choose __EMMA__ as a coverage tool in the drop\-down.
4. Set up the coverage options \- refer to the description of the available options below.

<table><tr>

<td>

Option 


</td>

<td>

Description  


</td></tr><tr>

<td>

Include Source Files in the Coverage Data 


</td>

<td>

Check this option to include source files into the code coverage report (you'll be able to see sources on the Web).

<warning>

Enabling this option can increase the report size and may slow down the creation of your builds. To avoid this situation, specify some [EMMA properties](http://emma.sourceforge.net/reference_single/reference.html#prop-ref.tables).
</warning>
 

</td></tr><tr>

<td>

Coverage Instrumentation Parameters 


</td>

<td>

Use this field to specify the filters to be used for creating the code coverage report. These filters define classes to be exempted from instrumentation. For detailed description of filters, refer to [EMMA documentation](http://emma.sourceforge.net/reference_single/reference.html#prop-ref.tables). 


</td></tr></table>


## Troubleshooting

#### No coverage, there is a message: EMMA: no output created: metadata is empty

Please make sure that all your classes (whose coverage is to be evaluated) are recompiled during the build. Usually this requires adding a "clean" task at the beginning of the build.



#### java.lang.NoClassDefFoundError: com/vladium/emma/rt/RT

This message appears when your build loads EMMA\-instrumented class files in runtime, and it cannot find emma.jar file in classpath. For test tasks, like `junit` or `testng`, TeamCity adds `emma.jar` to classpath automatically. But for other tasks, this is not the case and you might need to modify your build script or to exclude some classes from instrumentation.


If your build runs a java task which uses your own compiled classes, you'll have to either add `emma.jar` to the classpath of the java task,  or to ensure that classes used in your java task are not instrumented. Besides, you should run your java task with the `fork=true` attribute.


The corresponding `emma.jar` file can be taken from `buildAgent/plugins/coveragePlugin/lib/emma.jar`. For a typical build, the corresponding include path would be `../../plugins/coveragePlugin/lib/emma.jar`.



To exclude classes from compilation, use settings for EMMA instrumentation task. TeamCity UI has a field to pass these parameters to EMMA, labeled "Coverage instrumentation parameters". To exclude some package from instrumenting, use the following syntax: `-ix -com.foo.task.*,+com.foo.*,-*Test*`, where the package `com.foo.task.*` contains files for your custom task.

#### EMMA coverage results are unstable

Please make sure that your junit task has the `fork=true` attribute. The recommended combination of attributes is "`fork=true forkmode=once`".

 __  __
 
__See also:__


__Concepts__: [Build Runner](build-runner.md) | [Code Coverage](code-coverage.md)   
__Administrator's Guide__: [Configuring Java Code Coverage](configuring-java-code-coverage.md) | [IntelliJ IDEA](intellij-idea.md)

__ __