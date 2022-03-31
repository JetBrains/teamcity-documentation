[//]: # (title: Tests Split for Parallel Execution)
[//]: # (auxiliary-id: Tests Split for Parallel Execution)

TeamCity can split tests of a build in batches and run each batch on a separate build agent. This way, tests will run in parallel and the build will finish faster. The speed boost ratio depends on the number of test classes in the build and the number of used parallel agents.

Currently, the feature supports only [Maven](maven.md), [Gradle](gradle.md), and [IntelliJ IDEA Project](intellij-idea-project.md) build runners. The support for [.NET](net.md) is coming soon.

This feature addresses a popular use case when a build launches many independent tests on the same agent, one by one, while they could technically be running in parallel, utilizing resources of multiple agents. To emulate such a behavior, some users would configure several build configurations and join them in a chain with parallel connections. This worked, but required creating and maintaining a complex meta-structure to solve a rather simple task. We taught TeamCity to resolve it for users.

To enable this feature in a build configuration, go to **Build Configuration Settings | Build Features**, click **Add build feature**, and choose the _Run tests in parallel_ type. You will be prompted to set the number of batches, which also means the number of parallel agents to launch for a build.

>Before TeamCity could split a build, it needs to gather test statistics of at least one preceding build. This information helps subdivide tests in semi-equally sized batches so the total build time is as short as possible. The following splitted builds also contribute to this statistics and allow TeamCity to make smarter decisions.  
  If you enable this feature in a freshly added build configuration, its first build will run in a normal mode; if it finishes successfully and produces test reports, TeamCity will be able to split the second one.  
  If at some point TeamCity finds no builds to get statistics from (for example, all the recent builds have failed), it will have to start this flow from the beginning. It will try to run the next build regularly and split the following ones only after receiving the test statistics.

Here is how TeamCity will run a splitted build compared to a regular one:

* Before starting a build, it will analyze the history of recent builds and virtually split the current build’s tests in batches, according to the feature settings. Tests of the same class will always run on one agent.  
  If the configured number of batches is more than the number of test classes in a build, it will launch one agent per test class.
* It will launch builds in multiple hidden configurations — one per batch. Each hidden configuration is a read-only copy of the current build configuration that knows only about its assigned batch of tests.

After the build is finished, you can review the results of all tests on the **Tests** tab, as if it’s a regular build. Hidden configurations are displayed on the **Dependencies** tab.

Currently, this features has two notable limitations:
* Using splitted builds reduces the accuracy of the [build counter](configuring-general-settings.md#General+Build+Configuration+Settings) in the current configuration. If you rely on this number in your scripts, be cautious when enabling the new feature. Let us know about any concerns or bugs, and we will try to suggest a proper solution.
* Splitted builds cannot produce [artifacts](build-artifact.md). It is usually not a problem for builds dedicated to tests, as best practices imply that a separate configuration is used for producing/deploying artifacts. However, we still have plans to deliver this functionality in the future in order to support different use cases.

If your projects have builds with many tests, enabling this feature could noticeably reduce these builds’ time.