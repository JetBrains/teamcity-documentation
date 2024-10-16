[//]: # (title: Build Step Execution Conditions)
[//]: # (auxiliary-id: Build Step Execution Conditions;Build Step Conditions)

When configuring a build step, you can choose a general [execution policy](configuring-build-steps.md#Execution+Policy) and, since TeamCity 2020.1, add a parameter-based _execution condition_.

Execution conditions make builds more flexible and address many common use cases, such as:
* running the step only in the default branch
* running the step only in the `release` branch
* skipping the step in [personal builds](personal-build.md)

You can quickly select any of the available common options in the build step __Add condition__ menu:

<img src="execution-conditions.png" alt="Build step execution condition" width="706" border-effect="line"/>

Alternatively, select the __Other condition__ option to add the _parameter-based execution condition_, which is a logical condition that takes on input any [build parameter](configuring-build-parameters.md) provided by the TeamCity server or agent.

For example, to run the build step only on the `testbranch` branch, you can test the value of the `teamcity.build.branch` parameter, as follows:

<img src="execution-conditions-other.png" alt="teamcity.build.branch equals testbranch" width="460" border-effect="line"/>

In the Kotlin DSL, you can check the `teamcity.agent.jvm.os.name` parameter to run the current build step only on Windows agents, as follows:

```Kotlin
package _Self.buildTypes
import jetbrains.buildServer.configs.kotlin.*


object MyBuildConfig : BuildType({
    // ...
    steps {
        script {
            name = "Step 1"
            conditions {
                contains("teamcity.agent.jvm.os.name", "Windows")
            }
            // ...
        }
    }
})
```

The sample below illustrates how to utilize [parameters that report step exit statuses](configuring-build-steps.md#Step+Status+Parameters) to create a custom condition (step #3 runs only when step #2 fails **and** step #1 is successful.).


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

> Refer to this documentation article for the reference on available conditions: [](requirement-conditions.md).
> 
{style="tip"}

If you declare multiple execution conditions, the build step will be executed only if __all__ of them are satisfied in the current build run.

If you use parameter-based execution conditions in a build which belongs to a [build chain](build-chain.md), note that the parameter, used in a step condition, might change own value during the build. This might cause an unexpected source reuse in the whole build chain. To prevent this, consider disabling the "_Do not run new build if there is a suitable one_" option in the [snapshot dependency](snapshot-dependencies.md) of the build configuration that depends on the build with such condition. If this option is enabled, TeamCity will show the corresponding health report for the affected build configuration.

>When triggering a [custom build](running-custom-build.md), you can change the values of the parameters responsible for the steps' execution. This way, you can flexibly control a single build run: for example, run a quick build without engaging tests or other optional steps.

In this demo, we explore a use case when you need to run a given step only if the build runs in the specific environment. This can be easily achieved with build step conditions.

<video src="https://youtu.be/2muXXD2-0jg"
title="New in TeamCity 2020.2: Bitbucket Cloud Pull Request Support"/>

You can also read a recap of this tutorial in [this blog post](https://blog.jetbrains.com/teamcity/2020/07/new-in-2020-1-conditional-build-steps/).







