[//]: # (title: Build Step Execution Conditions)
[//]: # (auxiliary-id: Build Step Execution Conditions;Build Step Conditions)

When configuring a build step, you can choose a general [execution policy](configuring-build-steps.md#Execution+Policy) and, since TeamCity 2020.1, add a parameter-based _execution condition_.

Execution conditions make builds more flexible and address many common use cases, such as:
* running the step only in the default branch
* running the step only in the `release` branch
* skipping the step in [personal builds](personal-build.md)

You can quickly select any of the available common options in the build step __Add condition__ menu:

<img src="execution-conditions.png" alt="Build step execution condition"/>

Alternatively, select the __Other condition__ option to add the _parameter-based execution condition_, which is a logical condition that takes on input any [build parameter](configuring-build-parameters.md) provided by the TeamCity server or agent. For example, select the `teamcity.agent.jvm.os.name` parameter and set the condition to "contains `Windows`" to run the current build step only on agents that run on Windows.  
**See the reference on available conditions [here](requirement-conditions.md).**

If you declare multiple execution conditions, the build step will be executed only if __all__ of them are satisfied in the current build run.

If you use parameter-based execution conditions in a build which belongs to a [build chain](build-chain.md), note that the parameter, used in a step condition, might change own value during the build. This might cause an unexpected source reuse in the whole build chain. To prevent this, consider disabling the "_Do not run new build if there is a suitable one_" option in the [snapshot dependency](snapshot-dependencies.md) of the build configuration that depends on the build with such condition. If this option is enabled, TeamCity will show the corresponding health report for the affected build configuration.

>When triggering a [custom build](running-custom-build.md), you can change the values of the parameters responsible for the steps' execution. This way, you can flexibly control a single build run: for example, run a quick build without engaging tests or other optional steps.

In this demo, we explore a use case when you need to run a given step only if the build runs in the specific environment. This can be easily achieved with build step conditions.

<video href="2muXXD2-0jg"
title="New in TeamCity 2020.2: Bitbucket Cloud Pull Request Support"/>

You can also read a recap of this tutorial in [this blog post](https://blog.jetbrains.com/teamcity/2020/07/new-in-2020-1-conditional-build-steps/).


## Custom Conditions Based on Step Statuses

<chunk include-id="step-status-parameters">

TeamCity provides `teamcity.build.step.status.<step_ID>` parameters that report the status of the step with the given ID. Step IDs are shown below their names, and are editable only when you create them.

<img src="dk-stepID.png" width="706" alt="Step ID"/>

The available values of the `teamcity.build.step.status.<step_ID>` parameters are:

* `success` — when a step finishes with no errors.
* `failure` — when a step failed. This status is reported even when all build problems were [muted](investigating-and-muting-build-failures.md#Muting+Build+Problems).
* `cancelled` — when a build was cancelled while this step was running.

The `teamcity.build.step.status.<step_ID>` parameters appear only after their corresponding steps finish, and are not available right from the moment a build starts. This means neither steps that are still running, nor skipped steps have their `teamcity.build.step.status.<step_ID>` parameters available.

You can check all step statuses in the [](build-results-page.md#Parameters+Tab) of the build results page...

<img src="dk-parametersTab-statuses.png" width="706" alt="Step statuses"/>

...or via [TeamCity REST API](https://www.jetbrains.com/help/teamcity/rest/teamcity-rest-api-documentation.html).

```Shell
http://<SERVER_URL>/app/rest/builds/<BUILD_ID>/resulting-properties
```
{prompt="GET"}

</chunk>

The `teamcity.build.step.status.<step_ID>` parameters allow you to set up custom execution conditions based on statuses of previous steps. For example, in the sample config below step #3 runs only when step #2 fails **and** step #1 is successful.

```Kotlin
package _Self.buildTypes

import jetbrains.buildServer.configs.kotlin.*

object MyBuildConf : BuildType({
    steps {
        python {
            id = "Step1"
            // ...
        }
        python {
            id = "Step2"
            // ...
        }
        python {
            name = "Step3"
            conditions {
                equals("teamcity.build.step.status.Step1", "success")
                equals("teamcity.build.step.status.Step2", "failure")
            }
            // ...
        }
    }
})
```
