[//]: # (title: NCover)
[//]: # (auxiliary-id: NCover)
TeamCity supports code coverage with NCover (1.x and 3.x) for NUnit tests run via TeamCity NUnit test runner, which can be configured in one of the following ways: web UI, [TeamCity NUnit Test Launcher](teamcity-nunit-test-launcher.md), [NUnit for MSBuild](nunit-for-msbuild.md), [NUnit for MSBuild](nunit-for-msbuild.md), [NUnit for NAnt Build Runner](nunit-for-nant-build-runner.md). 

__Important Notes__ 
	
* To launch coverage, NCover and NCoverExplorer should be installed on the agent where the coverage build will run.
	
* You don't need to make any modifications to your build script to enable coverage.
	
* You don't need to explicitly pass any of the NCover/NCoverExplorer arguments to the TeamCity NUnit test runner.
	
* NCover supports .NET Framework 2.0 and 3.5 started under x86 platform (NCover 3.x also supports x64 platform and works with .NET Framework 4.0). Make sure, you use have specified the same platform both for NCover and NUnit.


[//]: # (Internal note. Do not delete. "NCoverd221e55.txt")    

## Configuring NCover 1.x


Make sure your NUnit tests run under x86.

To configure NCover 1.x:

1. While creating/editing Build Configuration, go to the __Build Steps__ page.
2. Add one of the build steps that support NCover ([.NET Process Runner](net-process-runner.md), [MSBuild](msbuild.md), [MSpec](mspec.md), [NAnt](nant.md), [NUnit](nunit.md)), configure unit tests.
3. Select __NCover (1.x)__ in __.NET coverage tool__.
4. Set up the NCover options \- refer to the description of the available options below.


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

Path to NCover


</td>


<td>

Specify the path to NCover installed on the build agent, or use `%system.ncover.v1.path%` to refer to the auto\-detected NCover on the build agent.


</td>
</tr>
<tr>


<td>

Path to NCoverExplorer


</td>


<td>

Specify the path to NCoverExplorer on the build agent.


</td>
</tr>
<tr>


<td>

Additional NCover Arguments


</td>


<td>

Type additional arguments to be passed to NCover. 


<note>

* Do not enter the arguments that can be configured in the web UI.
* Do not specify the output path for the generated reports. It is configured automatically by TeamCity.
</note>


</td>
</tr>
<tr>


<td>

Assemblies to Profile


</td>


<td>

Specify new\-line separated assembly names (without paths and extensions), or leave this field blank to profile all assemblies.   
Equivalent to `//a` NCover.Console option.


</td>
</tr>
<tr>


<td>

 Exclude Attributes


</td>


<td>

Specify the classes and methods with defined .NET attribute(s) to be excluded from the coverage statistics.  
Equivalent to `//ea` NCover.Console option


</td>
</tr>
<tr>


<td>

 Report Type


</td>


<td>

Select the report type. For the details, refer to [NCoverExplorer documentation](http://docs.ncover.com/ref/2-0/ncoverexplorer-console/coverage-reporting#report).


</td>
</tr>
<tr>


<td>

 Sorting


</td>


<td>

Select the preferred sorting option. For the details, refer to [NCoverExplorer documentation](http://docs.ncover.com/ref/2-0/ncoverexplorer-console/coverage-reporting#sort).


</td>
</tr>
<tr>


<td>

Additional NCoverExplorer Arguments


</td>


<td>

Specify additional arguments to be passed to NCoverExplorer. Do not enter here the output path for the reports, nor specify arguments, for which there are corresponding options in the UI.


</td>
</tr>
</table>


## Configuring NCover 3.x


Make sure you use have specified the same platform both for NCover and NUnit.

To configure NCover 3.x:

1. While creating/editing Build Configuration, go to the Build Steps page.	
2. Add one of the build steps that support NCover ([.NET Process Runner](net-process-runner.md), [MSBuild](msbuild.md), [MSpec](mspec.md), [NAnt](nant.md), [NUnit](nunit.md)), configure unit tests.	
3. Select __NCover (3.x)__ in __.NET coverage tool__.
4. Set up the NCover options \- refer to the description of the available options below.




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

Path to NCover 3


</td>


<td>

Specify the path to NCover. Alternatively, use `%system.ncover.v3.x86.path%` or `%system.ncover.v3.x64.path%` to refer to the auto\-detected NCover 3 on the build agent.


</td>
</tr>
<tr>


<td>

Run NCover under


</td>


<td>

Select the preferred platform to run the coverage under \- x86 or x64. Make sure the selected platform agrees with the one used for NUnit tests. 


</td>
</tr>
<tr>


<td>

NCover Arguments


</td>


<td>

Specify NCover arguments, i.e. assemblies to profile and coverage tool specific arguments. Do not enter here arguments, which can be specified in the UI, nor enter here output path for generated reports and NCover process parameters. Use `//ias.*` to get coverage of all assemblies.


</td>
</tr>
<tr>


<td>

NCover Reporting Arguments


</td>


<td>

Specify additional NCover reporting arguments, except for the output path. Use `or FullCoverageReport:Html:{teamcity.report.path}` to get the report.


</td>
</tr>
</table>


## Reporting NCover Results Manually

If .NET code coverage is collected by the build script and needs to be reported inside TeamCity (for example, [Rake](rake.md), or if you run tests via a test launcher other than TeamCity NUnit Test Launcher), there is a way to let TeamCity know about the coverage data. Read more in [Manually Configuring Reporting Coverage](manually-configuring-reporting-coverage.md).

__ __