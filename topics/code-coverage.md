[//]: # (title: Code Coverage)
[//]: # (auxiliary-id: Code Coverage)

Code coverage is a number of metrics that measure how your code is covered by unit tests. TeamCity supports the following coverage engines out of the box:


	
* __Java,__ see [Configuring Java Code Coverage](configuring-java-code-coverage.md)			
 * [IntelliJ IDEA](intellij-idea.md) coverage (bundled)		
 * [EMMA](http://emma.sourceforge.net/) open\-source toolkit (bundled)		
 * [JaCoCo](http://www.eclemma.org/jacoco/) open\-source (bundled)		
	
* __.NET__: see [Configuring .NET Code Coverage](configuring-.net-code-coverage.md)			
 * [JetBrains dotCover](jetbrains-dotcover.md) (bundled)		
 * [NCover](ncover.md)		
 * [PartCover](partcover.md)		




For importing reports from other coverage tools, see the related [notes](how-to.md#Integrate+with+Build+and+Reporting+Tools)



For importing coverage results in TeamCity, see [this section](how-to.md#Import+coverage+results+in+TeamCity).


To get the code coverage information displayed in TeamCity for the supported tools, you need to configure it in the dedicated section of a [Build Runner](build-runner.md) settings page. The following build runners include code coverage support:


	
* [Ant](ant.md)
	
* [IntelliJ IDEA Project](intellij-idea-project.md)
	
* [Maven](maven.md)
	
* [MSBuild](msbuild.md)
	
* [NAnt](nant.md)
	
* [NUnit](nunit.md)
	
* [MSpec](mspec.md)
	
* [.NET Process Runner](net-process-runner.md)
	
* [Ipr (deprecated)](ipr-deprecated.md)



Note that currently the Maven2 runner supports [IntelliJ IDEA](intellij-idea.md) and [JaCoCo](jacoco.md) coverage engines.   
The code coverage results can be viewed on the __Overview__ tab of the [Working with Build Results](working-with-build-results.md); detailed report is displayed on the dedicated [Working with Build Results](working-with-build-results.md) tab.

The chart for code coverage is also available on the [Statistic Charts](statistic-charts.md) tab of the build configuration.

For the details on configuring code coverage, refer to the dedicated pages: [Configuring Java Code Coverage](configuring-java-code-coverage.md), [Configuring .NET Code Coverage](configuring-.net-code-coverage.md).




 __  __

__See also:__


__Concepts__: [Build Runner](build-runner.md)

__Administrator's Guide__: [Configuring Java Code Coverage](configuring-java-code-coverage.md) | [Configuring .NET Code Coverage](configuring-.net-code-coverage.md) | [How To...](how-to.md)
