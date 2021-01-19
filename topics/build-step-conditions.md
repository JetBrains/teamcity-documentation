[//]: # (title: Build Step Conditions)
[//]: # (auxiliary-id: Build Step Conditions)

When configuring a build step, you can choose a general [execution policy](configuring-build-steps.md#Execution+policy) and, since TeamCity 2020.1, add a parameter-based _execution condition_.

Execution conditions make builds more flexible and address many common use cases, such as:
* running the step only in the default branch
* running the step only in the `release` branch
* skipping the step in [personal builds](personal-build.md)

You can quickly select any of the available common options in the build step __Add condition__ menu:

<img src="execution-conditions.png" alt="Build step execution condition"/>

Alternatively, select the __Other condition__ option to add the _parameter-based execution condition_, which is a logical condition that takes on input any [build parameter](configuring-build-parameters.md) provided by the TeamCity server or agent.

If you declare multiple execution conditions, the build step will be executed only if __all__ of them are satisfied in the current build run.

>When triggering a [custom build](triggering-a-custom-build.md), you can change the values of the parameters responsible for steps' execution. This way, you can flexibly control a single build run: for example, run a quick build without engaging tests or other optional steps.

In this demo, we explore a use case when you need to run a given step only if the build runs in the specific environment. This can be easily achieved with build step conditions.

<video href="M2wi6l0pZe4"
title="New in TeamCity 2020.2: Bitbucket Cloud Pull Request Support"/>

You can also read a recap of this tutorial in [this blog post](https://blog.jetbrains.com/teamcity/2020/07/new-in-2020-1-conditional-build-steps/).