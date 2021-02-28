[//]: # (title: Configuring .NET Code Coverage)
[//]: # (auxiliary-id: Configuring .NET Code Coverage)

TeamCity supports .NET code coverage using NCover, PartCover, and dotCover coverage engines. Configuration via the TeamCity UI is supported for:
* [NUnit build runner](nunit.md)
* NAnt runner: [&lt;nunit2&gt; NAnt task](nunit-for-nant-build-runner.md)
* MSBuild runner: &lt;NUnit&gt; and &lt;NUnitTeamCity&gt; [NUnit for MSBuild](nunit-for-msbuild.md)

For the [.NET](net.md) runner and with NUnit version 3.x the only supported coverage tool is [JetBrains dotCover](jetbrains-dotcover.md).

If you use a test framework other than NUnit, you can configure coverage analysis manually using the [JetBrains dotCover](http://www.jetbrains.com/dotcover/) console runner and TeamCity service messages as described [here](manually-configuring-reporting-coverage.md).
 
 <seealso>
        <category ref="admin-guide">
            <a href="nunit-support.md">NUnit Support</a>
        </category>
</seealso>
