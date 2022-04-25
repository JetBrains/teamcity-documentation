[//]: # (title: Tests Split for Parallel Execution)
[//]: # (auxiliary-id: Tests Split for Parallel Execution)

TeamCity is now capable of parallelizing the execution of your tests by distributing them across multiple build agents, thus minimizing the overall duration of tests. The tests of a build can be split into batches, and each batch will run on a separate build agent. The speed boost depends on the number of test classes in the build and the number of agents used in parallel.
Currently, the feature supports  [Maven](maven.md), [Gradle](gradle.md), [IntelliJ IDEA Project](intellij-idea-project.md), and [.NET](net.md) build runners with [a few of limitations](#known-limitations). 

This feature addresses a popular use case when a build consequently launches several independent tests on the same agent while they could technically be running in parallel, utilizing resources of multiple agents. 
To emulate such behavior, some users would configure several build configurations and join them in a chain with parallel connections. It worked, but required creating and maintaining a complex meta-structure.

## Run tests in parallel
Now TeamCity has the _Run tests in parallel_ build feature that solves this task.
To enable this feature in a build configuration, go to **Build Configuration Settings | Build Features**, click **Add build feature**, and choose the _Run tests in parallel_ type. You will be prompted to set the number of batches, which also means the number of parallel agents to use in a build.


## How TeamCity parallelizes tests

Before TeamCity splits tests into batches to run in parallel, it needs to gather test statistics of at least one preceding build. This information helps subdivide tests in semi-equally sized batches so that the total build time is as short as possible.
If you enable this feature in a freshly added build configuration, its first build will run in the normal mode; when it finishes and produces test reports, TeamCity will be able to split the second one.

TeamCity takes into account not only the latest tests run, but also the history of your tests: the next builds with parallel tests will also contribute to this statistics and allow TeamCity to make smarter decisions.
If at some point TeamCity finds no builds to get statistics from (for example, all the recent builds failed), it will have to start this flow from the beginning. It will try to run the next build regularly and divide the tests of the following ones only after receiving the test statistics.

The number of agents used by the build depends on the number of batches specified in the build feature and the number of test classes in the build:

* Before starting a build, TeamCity will analyze the history of recent builds and virtually divide the tests of the current build into the number of batches specified in the feature settings. Each batch will run on a separate agent. 
* Tests of the same class will always run on the same agent. If the configured number of batches is larger than the number of test classes in a build, TeamCity will launch one agent per test class. 
* When you configure the build feature which runs tests in parallel, TeamCity converts the initial build configuration into a [composite build](composite-build-configuration.md) and generates virtual build configurations; 
each of them is a read-only copy of the current build configuration with a batch of tests assigned to it. 
The number of batches controls the number of virtual build configurations: each of them will run on a separate  agent. These build configurations are not visible in the build overview and the list of tests.
* After the build is finished, you can review the results of all tests on the **Tests** tab, as if it were a regular build. The hidden configurations are displayed on the **Dependencies** tab.

## Known limitations

Currently, this feature has a few limitations. We're working to overcome them.

### General

* [Code coverage](code-quality-tools.md#code-coverage-tools) does not fully work for builds with parallel tests.
* Using parallel tests reduces the accuracy of the [build counter](configuring-general-settings.md#General+Build+Configuration+Settings) in the current configuration. If you rely on this number in your scripts, be careful when enabling the _Run tests in parallel_ feature.
* Builds with parallel tests cannot produce [artifacts](build-artifact.md). Best practices imply having a separate build configuration dedicated to tests and another build configuration that produces/deploys artifacts. However, different use cases might require running tests and producing artifacts within the same build. We're working to support this case too.
* The [Enforce Clean Checkout action](clean-checkout.md#enforcing-clean-checkout) does not work for build configurations with parallel tests configured.
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
* Вefinition from code are not supported.
* The test name **cannot** contain dots. 