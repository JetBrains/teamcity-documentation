[//]: # (title: Tests Split for Parallel Execution)
[//]: # (auxiliary-id: Tests Split for Parallel Execution)

TeamCity is now capable of parallelizing the execution of your tests by intelligently distributing them across multiple build agents, thus minimizing the overall duration of tests. The tests of a build can be split into batches, and each batch will run on a separate build agent. The speed boost depends on the number of test classes in the build and the number of agents used in parallel.
Currently, the feature supports  [Maven](maven.md), [Gradle](gradle.md), [IntelliJ IDEA Project](intellij-idea-project.md), and [.NET](net.md) build runners. 

This feature addresses a popular use case when a build launches many independent tests on the same agent, one by one, while they could technically be running in parallel, utilizing resources of multiple agents. To emulate such behavior, some users would configure several build configurations and join them in a chain with parallel connections. This worked, but required creating and maintaining a complex meta-structure.
Now TeamCity has the _Run tests in parallel_ build feature that solves this task.

To enable this feature in a build configuration, go to **Build Configuration Settings | Build Features**, click **Add build feature**, and choose the _Run tests in parallel_ type. You will be prompted to set the number of batches, which also means the number of parallel agents to use in a build.

> This feature takes into account not only the latest tests run, but also the history of your tests. Before TeamCity splits tests into batches to run in parallel, it needs to gather test statistics of at least one preceding build. This information helps subdivide tests in semi-equally sized batches so that the total build time is as short as possible. The further builds with parallel tests will also contribute to this statistics and allow TeamCity to make smarter decisions.  
  If you enable this feature in a freshly added build configuration, its first build will run in the normal mode; when it finishes and produces test reports, TeamCity will be able to split the second one.  
  If at some point TeamCity finds no builds to get statistics from (for example, all the recent builds failed), it will have to start this flow from the beginning. It will try to run the next build regularly and divide the tests of the following ones only after receiving the test statistics.

Here is how TeamCity will parallelize tests of a build:

* Before starting a build, it will analyze the history of recent builds and virtually divide the tests of the current build into batches, according to the feature settings. Tests of the same class will always run on the same agent.  
  If the configured number of batches is larger than the number of test classes in a build, TeamCity will launch one agent per test class.
* TeamCity will launch builds in multiple hidden configurations: one per batch. Each hidden configuration is a read-only copy of the current build configuration that knows only the batch of tests assigned to it.

After the build is finished, you can review the results of all tests on the **Tests** tab, as if it were a regular build. The hidden configurations are displayed on the **Dependencies** tab.

Currently, this feature has two notable limitations:
* Using parallel tests reduces the accuracy of the [build counter](configuring-general-settings.md#General+Build+Configuration+Settings) in the current configuration. If you rely on this number in your scripts, be careful when enabling the _Run tests in parallel_ feature. 
* Builds with parallel tests cannot produce [artifacts](build-artifact.md). Best practices imply having a separate build configuration dedicated to tests and another build configuration that produces/deploys artifacts. However, different use cases might require running tests and producing artifacts within the same build. We're working to support this case too.
