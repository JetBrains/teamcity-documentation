[//]: # (title: MSTest Support)
[//]: # (auxiliary-id: MSTest Support)

<note>

The MSTest runner is merged into the [Visual Studio Tests](visual-studio-tests.md) runner.
</note>

TeamCity provides support for MSTest 2005\-2015 testing framework via parsing of the MSTest results file (`.trx` file). The appropriate Microsoft Visual Studio edition installed on the build agent is required.

Due to specifics of MSTest tool, TeamCity does __not__ support on\-the\-fly test reporting for MSTest. All test results are imported __after__ the tests run has finished.

There are two ways to report test results to TeamCity:
* Add the [Visual Studio Tests](visual-studio-tests.md) runner as one of your build steps.
* Configure [XML Report Processing](xml-report-processing.md) via build feature or via [service message](build-script-interaction-with-teamcity.md#Service+Messages) to parse the `.trx` reports that are produced by your build procedure.

The easiest way to set up MSTest tests reporting in TeamCity is to add the [Visual Studio Tests](visual-studio-tests.md) build runner as one of the steps to your build configuration and specify there all required parameters.

If the tests are already run within your build script and MSTest generates `.trx` reports, you can configure [service messages](build-script-interaction-with-teamcity.md#Service+Messages) to parse the reports.

## Autodetection of MSTest

__Prior to TeamCity 9.1__ the MSTest location was reported as system properties: `%system.MSTest.8.0%`, `%system.MSTest.9.0%`, `%system.MSTest.10.0%`, `%system.MSTest.11.0%`, `%system.MSTest.12.0%`, `%system.MSTest.14.0%` that referred to MSTest `2008`, `2010`, `2012`, `2013`, `2015` correspondingly. 

__Since TeamCity 9.1__ system parameters of the `%system.MSTest.xx.yy%` format were changed to configuration parameters of the `%teamcity.dotnet.mstest.xx.yy%` format.

If system properties are required for the build, the [mstest-legacy-provider](https://youtrack.jetbrains.com/issue/TW-41845) plugin can be used.

 TeamCity auto\-detects MSTest based on the registry values that describe the Visual Studio installation path. If Visual Studio is installed in a non\-standard location, or the registry key is corrupted, or the TeamCity agent has no access to the VisualStudio directory, TeamCity may not be able to detect MSTest. In this case, the corresponding configuration parameter of the `%teamcity.dotnet.mstest.xx.yy%` format must be added to the build manually. It should contain the full path including the `MSTest.exe` executable, for example, the default path for MSTest 2013 is `C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE\MSTest.exe`.

<seealso>
        <category ref="concepts">
            <a href="testing-frameworks.md">Testing Frameworks</a>
        </category>
        <category ref="admin-guide">
            <a href="nunit-support.md">NUnit Support</a>
        </category>
</seealso>