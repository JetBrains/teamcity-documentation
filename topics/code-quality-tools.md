
[//]: # (title: Code Quality Tools)
[//]: # (auxiliary-id: Code Quality Tools)

TeamCity comes bundled with a number of tools capable of analyzing the quality of your code and reporting the obtained data. If you are using the tools which are currently not supported, TeamCity can be configured to run them and display their report results.

## Bundled Tools

Generally, the tools are configured as [build runners](build-runner.md) and the results are displayed on the Build Results page as well as in the IDE for some tools.

You can also configure builds to fail based on the results and view the trends as statistics charts.

### Java Tools

#### IntelliJ IDEA-powered Code Analysis Tools

These are available when you have an IntelliJ IDEA project (.idea directory or `.ipr` file) or a Maven project file (pom.xml) checked into your version control.
* [Inspections (IntelliJ IDEA)](inspections.md) runs [IntelliJ IDEA inspections](http://www.jetbrains.com/idea/documentation/inspections.jsp) in TeamCity. These include more than 600 Java, HTML, CSS, JavaScript inspections.
* [Duplicates Finder (Java)](duplicates-finder-java.md) provides a report on the discovered repetitive blocks of code.

#### Code Coverage tools

These are configured in the dedicated sections of the build runners.
* [IntelliJ IDEA](intellij-idea.md) code coverage is supported for [Ant](ant.md), [IntelliJ IDEA Project](intellij-idea-project.md), [Gradle](gradle.md) or [Maven](maven.md) build runners.
* [EMMA](emma.md) coverage supports [Ant](ant.md) build runner.
* [JaCoCo](jacoco.md) coverage supports [Ant](ant.md), [IntelliJ IDEA Project](intellij-idea-project.md), [Gradle](gradle.md) and [Maven](maven.md) build runners.

### .NET Tools

#### ReSharper-powered Tools

These are available if you use Visual Studio.
* [Inspections (ReSharper)](inspections-resharper.md) gathers results of [JetBrains ReSharper Code Inspections](http://www.jetbrains.com/resharper/webhelp/Code_Analysis__Code_Inspections.html) in your C#, VB.NET, XAML, XML, ASP.NET, JavaScript, CSS and HTML code.
* [Duplicates Finder (ReSharper)](duplicates-finder-resharper.md) provides a report on the discovered repetitive blocks of C# and VB.NET code.
* [FxCop](fxcop.md) uses [Microsoft FxCop](http://msdn.microsoft.com/en-us/library/bb429476(VS.80).aspx) preinstalled on a build agent.

#### Code Coverage

The following code coverage tools are supported for [.NET Process Runner](net-process-runner.md), [MSBuild](msbuild.md), [NAnt](nant.md) and [NUnit](nunit.md) build runners:
* [JetBrains dotCover](jetbrains-dotcover.md)
* [NCover](ncover.md)
* [PartCover](partcover.md)

For the [.NET](net.md) runner and with NUnit version 3.x the only supported coverage tool is [JetBrains dotCover](jetbrains-dotcover.md).

## Reporting External Tools Results in TeamCity

If you need to use non-bundled tools, you can use TeamCity to import their results and display them in the TeamCity UI.

### Supported Report Formats

The external tool reports are supported via the [XML Report Processing](xml-report-processing.md) build feature. See the list of [supported reports](xml-report-processing.md).

### Including HTML Reports

If your reporting tool is not supported by TeamCity directly, you can make it produce reports in the HTML format via a build script and add a build results [report tab in TeamCity](including-third-party-reports-in-the-build-results.md).

### Importing Code Coverage Results

You can also [import code coverage results in TeamCity](how-to.md#Import+coverage+results+in+TeamCity).

## Integration with External Tools

TeamCity can also [be integrated](how-to.md#Integrate+with+Build+and+Reporting+Tools) with external build tools or tools generating some report/providing code metrics which are not yet supported by TeamCity. The integration tasks involved are collecting the data in the scope of a build and then reporting the data to TeamCity.