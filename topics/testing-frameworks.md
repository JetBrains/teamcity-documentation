[//]: # (title: Testing Frameworks)
[//]: # (auxiliary-id: Testing Frameworks)

TeamCity provides out-of-the-box support for a number of testing frameworks. In order to reduce feedback time on the test failures, TeamCity provides support for on-the-fly tests reporting where possible. On-the-fly tests reporting means that the tests are reported in the TeamCity UI as soon as they are run not waiting for the build to complete.

TeamCity directly supports the following _testing frameworks_:
* JUnit and TestNG for the following runners: 
     * Ant (when tests are run by the `junit` and `testng` tasks directly within the script, TeamCity reports tests on the fly)
     * Maven2 (when tests are run by [Maven Surefire plugin](http://maven.apache.org/plugins/maven-surefire-plugin/) or [Maven Failsafe plugin](http://maven.apache.org/surefire/maven-failsafe-plugin/integration-test-mojo.html), tests reporting occurs after each module test run finish)
     * [IntelliJ IDEA](intellij-idea-project.md) project (when run with appropriate IDEA run configurations. Note that such run configurations should be shared in the IDE and related files should be checked in to the version control)
* [NUnit](nunit-support.md) for the following runners: 
     * The NAnt (`nunit2` task)
     * The MSBuild ([NUnit community](http://msbuildtasks.tigris.org/) or [NUnitTeamCity](nunit-for-msbuild.md) tasks)
     * Microsoft Visual Studio Solution runners (2003, 2005, 2008, 2010, 2012, 2013, and Visual Studio 2015)
     * Any runner provided [TeamCity Addin for NUnit](teamcity-addin-for-nunit.md) is installed
* [MSTest](mstest-support.md) 2005, 2008, 2010, 2012, 2013, and 2015 (On-the-fly reporting is not available due to MSTest limitations)
* [VSTest](visual-studio-tests.md) 2012, 2013, 2015
* [MSpec](mspec.md)

__Ruby testing frameworks__, [Test::Unit](http://ruby-doc.org/stdlib/libdoc/test/unit/rdoc/classes/Test/Unit.html), [Test-Spec](http://search.cpan.org/~philip/Test-Spec-0.48/lib/Test/Spec.pm), [Shoulda](http://github.com/thoughtbot/shoulda), [RSpec](http://rspec.info/), [Cucumber](http://cukes.info), are supported by the TeamCity [Rake](rake.md) runner. The [minitest](https://rubygems.org/gems/minitest) framework requires the `minitest-reporters` gem to be additionally installed.

There are also testing frameworks that have embedded support for TeamCity: for example, [Gallio](net-testing-frameworks-support.md#Gallio) and [xUnit](net-testing-frameworks-support.md#xUnit).

See also external [plugins](https://plugins.jetbrains.com/teamcity). 

Also, you can import test run XML reports of supported formats with [XML Report Processing](xml-report-processing.md).

### Custom Testing Frameworks

If there is no TeamCity support yet for your testing framework, you can report tests progress to TeamCity from the build via [service messages](service-messages.md#Reporting+Tests) or generate one of the supported [XML reports](xml-report-processing.md) in the build.

Also, see [notes](how-to.md#Integrate+with+Build+and+Reporting+Tools) on integrating with various reporting/metric tools.


<seealso>
        <category ref="user-guide">
            <a href="viewing-tests-and-configuration-problems.md">Viewing Tests and Configuration Problems</a>
        </category>
        <category ref="concepts">
            <a href="build-state.md">Build State</a>
            <a href="build-runner.md">Build Runner</a>
        </category>
        <category ref="admin-guide">
            <a href="nunit-support.md">NUnit Support</a>
            <a href="mstest-support.md">MSTest Support</a>
            <a href="nant.md">NAnt</a>
        </category>
</seealso>
