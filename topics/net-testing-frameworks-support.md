[//]: # (title: .NET Testing Frameworks Support)
[//]: # (auxiliary-id: .NET Testing Frameworks Support)

To support the real-time reporting of test results, TeamCity should either run tests using its own test runner or be able to interact with the testing frameworks to receive notifications on test events.

The following .NET testing plugins are supported by TeamCity out of the box.

## NUnit

To report NUnit test results, use the [.NET](net.md) or [NUnit](nunit.md) build runner.

Note that the NUnit runner supports only [.NET Framework](https://docs.microsoft.com/en-us/dotnet/framework/get-started/overview). To run tests for [.NET Core](https://docs.microsoft.com/en-us/dotnet/framework/get-started/net-core-and-open-source) projects (and .NET Framework projects version 4.0 or later), use the .NET build runner with the [`test`](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-test) command instead.

The details of the NUnit support in TeamCity and alternative approaches are described in [this article](nunit-support.md).

## MSTest

Refer to the [MSTest Support](mstest-support.md) page for details. Note that due to specifics of the MSTest tool, TeamCity does not support on-the-fly test reporting for MSTest.

## MSPec

A dedicated test runner is available for MSPec support. Refer to the [MSpec](mspec.md) page for details.

<anchor name="GallioSupport"/>

## Gallio
[//]: # (AltHead: GallioSupport)

Starting with version [3.0.4](http://blog.bits-in-motion.com/2008/10/announcing-gallio-and-mbunit-v304.html), [Gallio](http://www.gallio.org) supports on-the-fly test results reporting to the TeamCity server.

Other testing frameworks (for example, MbUnit, NBehave, NUnit, xUnit.Net, and csUnit) are supported by Gallio and thus can provide test reporting back to TeamCity.

Gallio also supports NCover, which allows including coverage HTML reports to the TeamCity build overview. See [this article](including-third-party-reports-in-the-build-results.md) for details.

<anchor name="xUnitSupport"/>

<anchor name="SupportxUnit"/>

## xUnit
[//]: # (AltHead: xUnitSupport)

See the [general information](https://xunit.net/docs/getting-test-results-in-teamcity) about the xUnit support and a related [blog post](http://blog.benhall.me.uk/2008/09/xunit-teamcity-integration.html).

Note that we do not recommend using xUnit in combination with other testing frameworks in TeamCity, since it might mix up the test reporting results.

<seealso>
        <category ref="troubleshooting">
            <a href="visual-c-build-issues.md">Visual C Build Issues</a>
        </category>
</seealso>
