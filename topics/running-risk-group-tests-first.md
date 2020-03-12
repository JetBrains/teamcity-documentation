[//]: # (title: Running Risk Group Tests First)
[//]: # (auxiliary-id: Running Risk Group Tests First)

<anchor name="testReorderingJUnitAndTestNG"/>

## Reordering Risk Tests for JUnit and TestNG
[//]: # (AltHead: testReorderingJUnitAndTestNG)

Supported environments:
* [Ant](ant.md) and [IntelliJ IDEA Project](intellij-idea-project.md) runners
* JUnit and TestNG frameworks when tests are started with usual JUnit or TestNG tasks

<tip>

TeamCity also allows [implementing tests reordering feature for a custom build runner](https://plugins.jetbrains.com/docs/teamcity/risk-tests-reordering-in-custom-test-runner.html).
</tip>

You can instruct TeamCity to run some tests before others. You can do this on the build runner settings page. Currently there are two groups of tests that TeamCity can run first:
* recently failed tests, i.e. the tests failed in previous finished or running builds as well as tests having high failure rate (a so called blinking tests)
* new and modified tests, i.e. tests added or modified in changelists included in the running build

<note>

The recently added or modified tests in your [personal build](personal-build.md) have the highest priority, and these tests run even before any other reordered test.
</note>

TeamCity allows you to enable both of these groups or each one separately.

TeamCity operates on test case basis, that is not the individual tests are reordered, but the full test cases. The tests cases are reordered only within a single test execution Ant task, so to maximize the feature effect, use a single test execution task per build.

Tests reordering works the following way:

#### JUint
1. TeamCity provides tests that should be run first (test classes).
2. When a JUnit task starts, TeamCity checks whether it includes these tests.
3. If at least one test is included, TeamCity generates a new fileset containing included tests only and processes it before all other filesets. It also patches other filesets to exclude tests added to the automatically generated fileset.
4. After that JUnit starts and runs as usual.

<note>

Some cases when automatic tests reordering will not work:
* if JUnit suites are created manually in test cases with help of suite() method
* if @RunWith annotation is used in JUnit4 tests
</note>

#### TestNG

##### TestNG versions less than 5.14:
1. TeamCity provides tests that should be run first (test classes).
2. When a TestNG task starts, TeamCity checks whether it includes these tests.
3. If at least one test is included, TeamCity generates a new xml file with suite containing included tests only and processes it before all other files. It also patches other files to exclude tests added to the automatically generated file.
4. After that TestNG starts and runs as usual.

<note>

Some cases when automatic tests reordering will not work
if &lt;package/&gt; element is used in the TestNG XML suite.
</note>

##### TestNG versions 5.14 or newer:
1. TeamCity provides tests that should be run first (test classes).
2. When a TestNG starts, TeamCity injects custom listener which will reorder tests if needed.
3. Before starting tests TestNG asks listener to reorder tests execution order list. If some test requires reordering, TeamCity listener moves it to to the start of the list.
4. After that TestNG runs tests in new order.

<note>

Some cases when automatic tests reordering will not work:
* if &lt;test/&gt; or &lt;suite/&gt; element in the TestNG XML suite has the `preserve-order` attribute set to `true`
* if TestNG suite file in YAML format
</note>

<anchor name="testReorderingForNUnit"/>

## Reordering Risk Tests for NUnit
[//]: # (AltHead: testReorderingForNUnit)

Supported build runners:
* [NAnt](nant.md), [MSBuild](msbuild.md), [NUnit](nunit.md), [Visual Studio 2003](visual-studio-2003.md) build runners
* NUnit testing framework

Tests reordering only supports reordering of recently failed tests. 


<note>

* When the "Run recently failed tests first" option is selected, the [Explicit](https://github.com/nunit/docs/wiki/Explicit-Attribute) attribute __will not work__.
* Test reordering is __not supported for parametrized NUnit tests.__
</note>


If risk tests reordering option is enabled, the feature for NUnit test runner works in the following way:
1. NUnit runs tests from the "risk" group using test name filter.
2. NUnit runs all other tests using inverse filter.

<note>

* Since tests run twice, thus risk test fixtures _Set Up_ and _Tear Down_ will be performed twice.
* Tests reordering feature applies to an NUnit task. That is, for NAnt and MSBuild runners, tests reordering feature will be initiated as many times as many NUnit tasks you have in your build script.
</note>

 __  __

__See also:__

__Concepts__: [Build Runner](build-runner.md)   
__Developing TeamCity Plugins__: [Risk Tests Reordering in Custom Test Runner](https://plugins.jetbrains.com/docs/teamcity/risk-tests-reordering-in-custom-test-runner.html)

__ __