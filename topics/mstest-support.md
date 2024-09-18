[//]: # (title: MSTest Support)
[//]: # (auxiliary-id: MSTest Support)

TeamCity can parse the MSTest results file (`.trx` file) and show test reports in the build overview. It supports the MSTest 2005-2015 framework and requires the respective Microsoft Visual Studio edition to be installed on the build agent.

>Due to specifics of the MSTest tool, TeamCity does not support on-the-fly test reporting for MSTest. All test results are imported after the tests are finished.
> 
{style="note"}

## Reporting MSTest Results

There are two ways to report MSTest results in TeamCity.

The easiest way is to add the [Visual Studio Tests](visual-studio-tests.md) runner as one of your build steps and specify all the required parameters there.

If tests are already run within your build script and MSTest generates the `.trx` reports, you can configure XML Report Processing via the [build feature](xml-report-processing.md) or via [service messages](service-messages.md) to parse these reports.

## Autodetection of MSTest

The MSTest location is reported as [configuration parameters](configuring-build-parameters.md) in the `%\teamcity.dotnet.mstest.xx.yy%` format.

If configuration parameters are required for the build, the [mstest-legacy-provider](https://youtrack.jetbrains.com/issue/TW-41845) plugin can be used.

TeamCity autodetects MSTest based on the registry values that describe the Visual Studio installation path. If (a) Visual Studio is installed in a non-standard location, (b) the registry key is corrupted, or (c) the TeamCity agent has no access to the VisualStudio directory, TeamCity may not be able to detect MSTest. In this case, the corresponding configuration parameter of the `%\teamcity.dotnet.mstest.xx.yy%` format must be added to the build manually. It should contain the full path including the `MSTest.exe` executable: for example, the default path for MSTest 2013 is `C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE\MSTest.exe`.

<seealso>
        <category ref="admin-guide">
            <a href="testing-frameworks.md">Supported Testing Frameworks</a>
            <a href="net-testing-frameworks-support.md">.NET Testing Frameworks Support</a>
        </category>
</seealso>