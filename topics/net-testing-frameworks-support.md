[//]: # (title: .NET Testing Frameworks Support)
[//]: # (auxiliary-id: .NET Testing Frameworks Support)
To support the real\-time reporting of test results, TeamCity should either run the tests using its own test runner or be able to interact with the testing frameworks so it receives notifications on test events. Custom TeamCity\-aware test runners are used to implement the bundled support for the testing frameworks.

## NUnit

Refer to the [NUnit Support](nunit-support.md) page for details.

## MSTest

Refer to the [MSTest Support](mstest-support.md) page for details.

## MSPec

Dedicated test runner is available for MSPec support. Refer to the [MSpec](mspec.md) page for details.

<anchor name="GallioSupport"/>

## Gallio
[//]: # (AltHead: GallioSupport)

Starting with version [3.0.4](http://blog.bits-in-motion.com/2008/10/announcing-gallio-and-mbunit-v304.html), [Gallio](http://www.gallio.org) supports on\-the\-fly test results reporting to TeamCity server.

Other testing frameworks (for example, MbUnit, NBehave, NUnit, xUnit.Net, and csUnit) are supported by Gallio and, thus, can provide tests reporting back to TeamCity.

As for coverage, Gallio supports NCover, to include coverage HTML reports to TeamCity build tab. See [Including Third-Party Reports in the Build Results](including-third-party-reports-in-the-build-results.md).

<anchor name="xUnitSupport"/>
<anchor name="SupportxUnit"/>

## xUnit
[//]: # (AltHead: xUnitSupport)

[General information](http://xunit.github.io/docs/getting-test-results-in-teamcity.html) about xUnit support from its authors. Also a related [blog post](http://blog.benhall.me.uk/2008/09/xunit-teamcity-integration.html).



__  __

__See also:__



__Troubleshooting__: [Visual C Build Issues](visual-c-build-issues.md)
