[//]: # (title: Parallel Tests)
[//]: # (auxiliary-id: Parallel Tests)

TeamCity is now capable of parallelizing the execution of your tests by distributing them across multiple build agents, thus minimizing the overall duration of tests. The tests of a build can be split into batches, and each batch will run on a separate build agent. The speed boost depends on the number of test classes in the build and the number of agents used in parallel.
Currently, the feature supports  [Maven](maven.md), [Gradle](gradle.md), [IntelliJ IDEA Project](intellij-idea-project.md), and [.NET](net.md) build runners with [a few of limitations](#Known+limitations). 

This feature addresses a popular use case when a build consequently launches several independent tests on the same agent while they could technically be running in parallel, utilizing resources of multiple agents. 
To emulate such behavior, some users would configure several build configurations and join them in a chain with parallel connections. It worked, but required creating and maintaining a complex meta-structure.

## Run tests in parallel

Now TeamCity has the _Run tests in parallel_ build feature that solves this task.
To enable this feature in a build configuration, go to **Build Configuration Settings | Build Features**, click **Add build feature**, and choose the _Parallel Tests_ type. You will be prompted to set the number of batches, which also means the number of parallel agents to use in a build.

Before TeamCity splits tests into batches to run in parallel, it needs to gather test statistics of at least one preceding build. This information helps subdivide tests in semi-equally sized batches so that the total build time is as short as possible.
If you enable this feature in a freshly added build configuration, its first build will run in the normal mode; when it finishes and produces test reports, TeamCity will be able to split the second one.

TeamCity takes into account not only the latest tests run, but also the history of your tests: the next builds with parallel tests will also contribute to this statistics and allow TeamCity to make smarter decisions.
If at some point TeamCity finds no builds to get statistics from (for example, all the recent builds failed), it will have to start this flow from the beginning. 
It will try to run the next build regularly and divide the tests of the following ones only after receiving the test statistics.

### Under the hood

If the test statistics is available, then the following happens when a new build is triggered:

1. TeamCity generates copies of the current build configuration according to the number of batches specified in the build feature. 
These build configurations will have the same build steps as the original one.
In the future, the changes to the build steps of the original configuration will be propagated to the generated ones automatically.
2. The triggered build will be transformed into a [composite build](composite-build-configuration.md) with dependencies on the builds from the generated build configurations.
3. As soon as the first dependency build starts, the composite build will start too.

>The settings of the original build configuration are not affected by the _Parallel Tests_ build feature. 
The number and settings of generated build configurations are fully controlled by the _Parallel Tests_ build feature. The configurations are read-only by default.
They are not intended to be modified manually. 
The generated build configurations are placed into a subproject that is hidden.
If the project has [versioned settings](storing-project-settings-in-version-control.md) enabled, the generated build configurations will not be committed to the VCS repository.

A build of a generated build configuration will run a batch of tests. Each batch gets the tests to execute. 
[Maven](maven.md), [Gradle](gradle.md), [IntelliJ IDEA Project](intellij-idea-project.md), 
and [.NET](net.md) build runners will automatically run the tests of this batch.

If you run tests differently, you can still enable the _Parallel Tests_ build feature for your configuration 
and benefit from automatic test division: you will obtain the information about the tests to execute from [the build parameters](#custom-tests) that will be provided by the build feature.

>If the original build configuration has deployment steps, these steps will be performed the same number of times as the number of batches.

### Custom execution of parallelized tests
{id="custom-tests"}

In some cases the tests are being executed in such a way that TeamCity cannot affect their execution anyhow. For instance, they can be generated on the fly, or they can be reported by a third party build runner, or imported from a file, and so on.

For such cases the _Parallel Tests_ build feature provides a number of build parameters which can be used to by the custom tests execution logic.
The parameters are:

| Parameter name                                   | Description                                                                                    |
|--------------------------------------------------|------------------------------------------------------------------------------------------------|
| teamcity.build.parallelTests.currentBatch        | Contains the current batch number starting with 1                                              |
| teamcity.build.parallelTests.totalBatches        | Contains the total number of configured batches                                                |
| system.teamcity.build.parallelTests.excludesFile | Contains a path on the agent to a text file with tests which should be excluded from execution |

Format of the file with excluded tests is as follows:
```
#version=1
#algorithm=<name of the algorithm used to split tests, optional>
#current_batch=<number of the current batch, same as teamcity.build.parallelTests.currentBatch parameter>
#total_batches=<total number of batches, same as teamcity.build.parallelTests.totalBatches parameter>
#suite=<suite name, can be empty>
<new line separated list of test classes>
```

Here `version` represents a file format version. It is implied that the custom tests execution logic checks the version and reports an error or fails a build if version has an unexpected value. 

In the future new keywords starting with `#` character can be added to the file. All such unrecognized keywords should be ignored.    

Note: Java and .NET test frameworks usually report tests to TeamCity in the following format:

`[<suite name>: ]<fully qualified test class name>.<test method>[<test arguments>]`

Where `<suite name>` and `<test arguments>` are optional and not always present.

For example the following Java Test class:
```java
package org.example.tests;

class TestCase1 {

  public void testMethod1() { ... }

  public void testMethod2() { ... }
}
```

will produce the following test names in TeamCity:
```
org.example.tests.TestCase1.testMethod1
org.example.tests.TestCase1.testMethod2
```

Then the parameter `system.teamcity.build.parallelTests.excludesFile` will point to a text file with the following content:
```
#version=1
#current_batch=1
#total_batches=3
#suite=
org.example.tests.TestCase1
```

> Note: the granularity of tests filtering is per class (or test case) but not per test method.

The build step with custom tests filtering logic should use this file and filter out all the tests that belong to the classes mentioned there. All the other tests should be executed.


## Known limitations

Currently, this feature has a number of limitations.

### General

* The [Code coverage](code-quality-tools.md#code-coverage-tools) statistics will be inaccurate for builds with parallel tests.
* Using parallel tests reduces the accuracy of the [build counter](configuring-general-settings.md#General+Build+Configuration+Settings) in the current configuration.
* After enabling parallel tests in a build configuration, it will stop publishing [artifacts](build-artifact.md). Consider having a separate build configuration that publishes artifacts.
* The [Enforce Clean Checkout action](clean-checkout.md#Enforcing+Clean+Checkout) does not work for build configurations with parallel tests configured.
* Virtual build configurations copy the settings from the default branch of the original build configuration ignoring other branch settings.
* Parallel builds collect the results not only of their its own tests, but transitive dependencies too.
* A newly added test will run in each batch during the first run.

### Runners

* [Maven](maven.md)
  * Maven minimal supported version: 3.x.
  * Maven Surefire plugin minimal supported version: 2.13.
  * Maven Failsafe Plugin minimal supported version: 2.13.
  * Maven project parameters do not forward into the main build.
  
* [Gradle](gradle.md)
  * Gradle minimal supported version: 5.0

### Test Frameworks

* Test classes should be defined in the build tool filters.
* Definition from code are not supported.
* The test name **cannot** contain dots. 