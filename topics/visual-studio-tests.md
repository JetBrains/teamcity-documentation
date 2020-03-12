[//]: # (title: Visual Studio Tests)
[//]: # (auxiliary-id: Visual Studio Tests)

The Visual Studio Tests runner integrates functionality of the MSTest framework and [VSTest console runner](https://plugins.jetbrains.com/plugin/9056-vstest-console-runner). Support for both frameworks enables TeamCity to execute tests and automatically import their test results. 

The Visual Studio Test Runner requires Visual Studio Test Agent or Microsoft Visual Studio installed on the build agent.

## Visual Studio Tests runner settings

<table><tr>

<td>

Option


</td>

<td>

Description


</td></tr><tr>

<td>

Test engine type


</td>

<td>

Select the tool used to run tests: VSTest or MSTest.


</td></tr><tr>

<td>

Test engine version


</td>

<td>

Select the version of the tool from the drop\-down. By default, the available VSTest and MSTest installations are autodetected by TeamCity:

* MSTest versions 2005\-2017 are supported
* VSTest versions 2012\-2017 are supported

You can specify a custom path to the test runner here as well. TeamCity parameters are supported.

<tip>

You can specify the [agent property](configuring-build-parameters.md#Defining+Build+Parameters+in+Build+Configuration) `teamcity.dotnet.vstest.VS_VERSION.install.dir` pointing to a VSTest distribution path and use it here, where `VS_VERSION` can be 11.0, 12.0, 14.0, 15.0, or 16.0. The property will have priority over registry values.
</tip>


</td></tr><tr>

<td>

Test file names


</td>

<td>

This field is __mandatory for VSTest__ and optional for MSTest.   
Specify the new\-line separated list of paths to assemblies to run tests on in the included assemblies list. Exclude assemblies from the test run by specifying paths to them in the corresponding field. [Wildcards](wildcards.md) are supported.   
Paths to the assemblies must be relative to the [build checkout directory](build-checkout-directory.md).


</td></tr><tr>

<td>

Run configuration file


</td>

<td>

(Optional) Specify the typical `.runsettings` file for [VSTest](https://msdn.microsoft.com/library/jj155796.aspx) or `.testsettings` file for [MSTtest](https://msdn.microsoft.com/library/ms182489.aspx#testsettings). You can use the file browser ![VCS-browserIcon.png](VCS-browserIcon.png).


</td></tr><tr>

<td>

Additional command line parameters


</td>

<td>

Enter additional command line parameters for the selected test engine.   
Note that tests run with MSTest are __not__ reported on\-the\-fly.   
The Microsoft Developer Network lists the available options for [VSTest](https://msdn.microsoft.com/en-us/library/jj155796.aspx) and [MSTest](http://msdn.microsoft.com/en-us/library/ms182489(VS.80).aspx).


</td></tr></table>

The rest of settings will vary depending on the engine to run tests with:

* [VSTest Settings](#VSTest+Settings)
* [MSTest Settings](#MSTest+settings)
 
### VSTest Settings

 

<table><tr>

<td>

Option


</td>

<td>

Description


</td></tr><tr>

<td>

Target platform


</td>

<td>

Select the platform bitness. Note that specifying x64 target platform will force the `vstest.console` process to be run in the isolated mode


</td></tr><tr>

<td>

Framework


</td>

<td>

If the default is specified, vstest.console will select the target framework automatically. You can also choose the .NET platform manually using the drop\-down.


</td></tr><tr>

<td>

Test names


</td>

<td>

(optional) Of all tests discovered in the included assemblies, only the tests with the names matching the provided values will be run. For multiple values, use new line.

If the field is empty, all tests will be run. See details in the [Microsoft documentation](https://msdn.microsoft.com/en-us/library/jj155800.aspx). Cannot be used with the option below.


</td></tr><tr>

<td>

Test case filter


</td>

<td>

Run tests that match the given expression. See details in the [Microsoft documentation](https://msdn.microsoft.com/en-us/library/jj155800.aspx). Cannot be used with the option above.


</td></tr><tr>

<td>

Run in isolation


</td>

<td>

Run the tests in an isolated process


</td></tr><tr>

<td>

Use real\-time test reporting


</td>

<td>

* If this checkbox is selected, a custom TeamCity test logger will be used for real\-time reporting. See the [next section](#Custom+test+logger) for details.
* If not selected, VSTest will report tests to TeamCity after the run (the discovery of custom logger will not be attempted).

<note>

When using this option, it is recommended to check the number of tests in the produced `trx` file against the build log. In case of incosistencies, switch to the custom logger.
</note>

 



</td></tr></table>

#### Custom test logger

`VSTest.Console` supports custom loggers, i.e. libraries that can handle events that occur when tests are being executed.   
TeamCity 9.0\+ has a custom logger that provides real\-time test reporting.   
The logger must be installed manually on the agent machine, as it requires dlls to be copied to the `Extensions` folder of the VSTest.Console. No agent restart is needed when the custom logger is installed.

__To install the custom logger__:

1\. Download the [custom logger](http://teamcity.jetbrains.com/guestAuth/app/rest/builds/buildType:TeamCityPluginsByJetBrains_TeamCityVSTestTestAdapter_Build,pinned:true,status:SUCCESS,branch:master,tags:release/artifacts/content/TeamCity.VSTest.TestLogger.zip).

2\. Extract the contents of the downloaded archive on the agent machine to the following path:
* for VisualStudio 2019 update:
   
```Plain Text
PROGRAM_FILES(x86)\Microsoft Visual Studio\2019<Edition>\Common7\IDE\Extensions\TestPlatform\Extensions

```

* for VisualStudio 2017 update 5 onwards:
   
```Plain Text
PROGRAM_FILES(x86)\Microsoft Visual Studio\2017<Edition>\Common7\IDE\Extensions\TestPlatform\Extensions

```
* for VisualStudio 2017 up to update 4:
   
```Plain Text
PROGRAM_FILES(x86)\Microsoft Visual Studio\2017<Edition>\Common7\IDE\CommonExtensions\Microsoft\TestWindow\Extensions

```
* for VisualStudio 2015:

```Plain Text
PROGRAM_FILES\Microsoft Visual Studio 14.0\Common7\IDE\CommonExtensions\Microsoft\TestWindow\Extensions

```

* for VisualStudio 2013:

```Plain Text
PROGRAM_FILES\Microsoft Visual Studio 12.0\Common7\IDE\CommonExtensions\Microsoft\TestWindow\Extensions

```

* for VisualStudio 2012:

```Plain Text
PROGRAM_FILES\Microsoft Visual Studio 11.0\Common7\IDE\CommonExtensions\Microsoft\TestWindow\Extensions

```

3\. Check that the custom logger was installed correctly by executing `vstest.console.exe /ListLoggers` in the console on the agent machine. If the logger was installed correctly, you will see the logger with FriendlyName `TeamCity` listed:


```Shell
VSTest.TeamCityLogger.TeamCityLogger
Uri: logger://TeamCityLogger
FriendlyName: TeamCity{info}

```



### MSTest settings

 

<table><tr>

<td>

Option


</td>

<td>

Description


</td></tr><tr>

<td>

MSTest metadata


</td>

<td>

Enter a value for `/testmetadata:file` argument. See details in the [Microsoft documentation](https://msdn.microsoft.com/en-us/library/ms182489(VS.80).aspx#testmetadata)


</td></tr><tr>

<td>

Testlist from metadata to run


</td>

<td>

Edit the Testlist. Every line will be translated into `/testlist:line` argument. See details in the [Microsoft documentation](https://msdn.microsoft.com/en-us/library/ms182489(VS.80).aspx#testlist). 


</td></tr><tr>

<td>

Test


</td>

<td>

Names of individual tests to run. This option will be translated into the series of `/test:` arguments. See details in the [Microsoft documentation](https://msdn.microsoft.com/en-us/library/ms182489(VS.80).aspx#test). 


</td></tr><tr>

<td>

Unique


</td>

<td>

Run the test only if one unique match is found for any specified test in test section


</td></tr><tr>

<td>

Results file


</td>

<td>

Enter a value for the `/resultsfile:file_name` command\-line argument. Parameter references are supported. It __is recommended to leave the field blank__ to generate a temporary `\*.trx` file in the build temporary directory. Note that the directory may be cleaned between the builds.

To save the test run results to a named non\-default file, enter a value for the `/resultsfile:file_name` command\-line argument.

 * If the path specified is relative, it will be relative to the build checkout directory. If the specified file already exists in the checkout directory, the build agent will attempt to delete the file. If the agent fails to delete it, the build will fail.   
 To create a `*.trx` report in the build temporary directory, use `%system.teamcity.build.tempDir%.`

<note>

If you need to generate your results file in the checkout directory, consider adding the [Swabra](build-files-cleaner-swabra.md) build feature to your configuration.
</note>


 * If the path specified is absolute, it will be used "as is".



</td></tr></table>

__ __