[//]: # (title: PartCover)
[//]: # (auxiliary-id: PartCover)

TeamCity supports code coverage with PartCover (2.2 and 2.3) for NUnit tests run via the TeamCity NUnit test runner, which can be configured in one of the following ways: via the web UI, [TeamCity NUnit Test Launcher](nunit-support.md#NUnit+Test+Launcher), [NUnit for MSBuild](nunit-support.md#Using+NUnit+for+MSBuild), [NUnit for MSBuild](nunit-support.md#Using+NUnit+for+MSBuild), [NUnit for NAnt Build Runner](nunit-support.md#NUnit+for+NAnt+Build+Runner).

__Important Notes__
	
* In order to launch coverage, PartCover should be installed on an agent where coverage builds will run.
* You don't need to make any modifications to your build script to enable coverage.
* You don't need to explicitly pass any of the PartCover arguments to the TeamCity NUnit test runner.

To configure PartCover:

1. While creating/editing Build Configuration, go to the Build Runner page.
2. Select __PartCover (2.2 or 2.3)__ as a .NET coverage tool.
3. Select the .Net runtime platform and version. 
4. Set up the PartCover options — find the description of the available options below.

>Some versions of PartCover support .NET Framework 2.0 and 3.5 and can be started under x86 platform only. Make sure you use the appropriate configuration options.

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

Path to PartCover

</td>

<td>

Specify the path to PartCover installed on a build agent, or the corresponding [system property](configuring-build-parameters.md), if configured.

</td>
</tr>
<tr>

<td>

Additional PartCover Arguments

</td>

<td>

Specify additional PartCover arguments, excluding  the ones that can be specified using the web UI. Do not specify here the output path for the generated reports, because TeamCity configures it automatically.

</td>
</tr>
<tr>

<td>

Include Assemblies

</td>

<td>

Explicitly specify the assemblies to profile, or use `[*]*` to include all assemblies.

</td>
</tr>
<tr>

<td>

Exclude Assemblies

</td>


<td>

Explicitly specify the assemblies to be excluded from coverage statistics. If you have specified `[*]*` to profile all assemblies, type `[JetBrains*]*` here to exclude TeamCity NUnit test runner sources.

</td>
</tr>
<tr>

<td>

Report XSLT

</td>

<td>

Write new-line delimited xslt transformation rules in the following format: `file.xslt=>generatedFileName.html`. You can use the default PartCover xslt as `file.xslt`, or your own. The Xslt files path is relative to the build checkout directory.


<tip>

Note that default xslt files bundled with PartCover 2.3 are broken, and you need to write your own xslt files to be able to generate reports.
</tip>

</td>
</tr>
</table>

## Reporting PartCover Results Manually

If .NET code coverage is collected by the build script and needs to be reported inside TeamCity (for example, [Rake](rake.md), or if you run tests via a test launcher other than TeamCity NUnit Test Launcher), there is a way to let TeamCity know about the coverage data. Read more in [Manually Configuring Reporting Coverage](manually-configuring-reporting-coverage.md).
