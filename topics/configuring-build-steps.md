[//]: # (title: Configuring Build Steps)
[//]: # (help-id: Configuring Build Steps)


<show-structure for="chapter" depth="2"/>


A **build step** is the smallest unit of a CI/CD workflow. It defines a specific set of actions that are executed as a whole. Build steps belong to [build configurations](creating-and-editing-build-configurations.md) and [pipeline jobs](create-and-edit-pipelines.md).


## Build Steps in Configurations and Pipelines

TeamCity provides a wide range of build steps designed for specific build tools, such as [](net.md), [](maven.md), [](nant.md), or [Xcode](xcode-project.md).

The full set of steps is currently available for [build configurations](creating-and-editing-build-configurations.md). [Pipelines](create-and-edit-pipelines.md),  introduced in version 2025.07, support a limited subset. However, you can use the [script step](command-line.md) to build any project type, as it can run commands with any tools installed on an agent machine.


## Add Steps via TeamCity UI

<secondary-label ref="secondary-config"/>
<secondary-label ref="secondary-pipeline"/>

### Build Configurations

1. <include from="common-templates.md" element-id="open-configuration-settings-tab"><var name="configuration-tab-name" value="Build steps"/></include>
   
2. Click **Add build step** to pick a build step manually, or **Auto-detect build steps** to let TeamCity scan a remote repository targeted by a [VCS root](configuring-vcs-roots.md) and suggest appropriate steps.

    > When scanning a repository, TeamCity looks for project files within the top two levels of the source tree and identifies them by file extensions. For example:
    > 
    > * [PowerShell](powershell.md): `.ps1`
    > * [.NET](net.md): `project.json`, `.proj`, `.csproj`, `.vbproj`, `.sln`
    > 
    > If a suggested step has already been added to the configuration, TeamCity will skip it in the recommendations.
    > 
    {style="note"}

<img src="dk-all-build-steps-configuration.png" width="706" alt="New build step page"/>

The **New Build Step** page includes two columns:

* Default steps — predefined by JetBrains.
* [Recipes](working-with-meta-runner.md) (formerly meta-runners) — custom XML or YAML steps created by the TeamCity community or your team.

Access to public recipes from JetBrains Marketplace depends on your project settings. See the following article to learn more: [](working-with-meta-runner.md).



### Pipelines

1. Open [](job-settings.md) and scroll to the **Steps** section in the side settings panel.
2. Click any tile to add a corresponding build step.

<img src="dk-add-step-jobs.png" width="706" alt="Add build steps to a job"/>



## Add Steps in Code

<secondary-label ref="secondary-config"/>
<secondary-label ref="secondary-pipeline"/>

TeamCity supports [configuration-as-code](storing-project-settings-in-version-control.md) in [Kotlin DSL](kotlin-dsl.md) and [YAML](recipe-yaml-syntax.md) (currently only for pipelines) formats.


<tabs>

<tab title="Kotlin DSL">

```Kotlin
import jetbrains.buildServer.configs.kotlin.*
import jetbrains.buildServer.configs.kotlin.buildSteps.maven
import jetbrains.buildServer.configs.kotlin.buildSteps.script

object SampleConfig: BuildType({
    name = "Sample two-step configuration"
    steps {
        script {
            name = "Step 1"
            id = "simpleRunner"
            scriptContent = """echo "Hello world""""
        }
        maven {
            name = "Step 2"
            id = "Step_2"
            goals = "clean package"
            jdkHome = "%env.JDK_21_0_ARM64%"
            mavenVersion = custom {
                path = "%teamcity.tool.maven.3.8.6%"
            }
        }
    }
})
```

</tab>


<tab title="YAML">

```yaml
Job1:
    name: Sample two-step job
    steps:
      - type: script
        name: Step 1
        script-content: echo "Hello world"
      - type: maven
        name: Step 2
        pom-location: pom.xml
        goals: clean package
        jdk-home: '%env.JDK_21_0_ARM64%'
        maven-version:
          bundled_3_8: {}
```

</tab>

</tabs>


## Step Execution Conditions

<secondary-label ref="secondary-config"/>

Build steps run top to bottom, in the order they appear in the UI or configuration code. By default, TeamCity runs all steps until one fails. If a step fails, the build is marked as failed and remaining steps are skipped.

You can override this behavior in build configurations by setting a custom execution policy that defines when a step should run.

<img src="dk-step-execution-conditions.png" width="706" alt="Step execute conditions"/>

Execution conditions consist of two parts:

1. A general rule selected from a corresponding drop-down menu.
2. Optional additional conditions added via the **Add condition** menu.


### General Execution Rules

* __Only if build status is successful__ — before starting the step, the build agent requests the build status from the server, and skips the step if the status is "failed". This considers the failure conditions processed by the server, like failure on test failures or on metric change. Note that this still may be not exact as some failure conditions are processed on the server asynchronously ([TW-17015](https://youtrack.jetbrains.com/issue/TW-17015)).

* **Only if build status is failed** — same as above, but the agent skips the step if the build status is "success". This condition is useful for rollback or cleanup actions.

* __If all previous steps finished successfully__ — runs if all prior steps on the agent succeeded, without checking the build status reported by the server.

* __Even if some of the previous steps failed__ — runs regardless of prior step results or build status.

* __Always, even if build stop command was issued__ — ensures the step runs even if the build was canceled. For example, if two steps use this setting and the build is stopped during the first, the second will still execute. A second stop command fully terminates the build.

### Additional Conditions

You can refine execution behavior with custom conditions. For example, skip steps in personal builds or non-default branches, or run OS-specific steps by checking the `teamcity.agent.jvm.os.name` [parameter](predefined-build-parameters.md) value.


See the following topic for more information: [](build-step-execution-conditions.md).



## Bootstrap Steps
{instance="tc"}

<secondary-label ref="secondary-config"/>

A bootstrap step is performed right after a build is triggered, before a build agent checks out source files. This allows you to add a runner (typically the [](command-line.md) one) that performs initial setup and ensures the following steps run as expected.

To enable bootstrap steps, first add the `teamcity.internal.bootstrap.steps.enabled=true` entry either to [](server-startup-properties.md#TeamCity+Internal+Properties) or to the individual project (as this project's [configuration parameter](configuring-build-parameters.md)). This setting allows you to do the following:

* Enable the **Run during bootstrap** option in step settings.
  <img src="dk_bootstrap_step.png" width="706" alt="Bootstrap step" style="block"/>

* On the configuration's **Build Steps** page, click **Reorder build steps** and drag the required step before the "Preparation stage" block.
  <img src="dk_bootstrap_step_drag.png" width="706" alt="Drag and drop bootstrap step" style="block"/>



You can create a build configuration with a bootstrap step and turn this configuration into a [template](build-configuration-template.md). Such template allows you to quickly create more configurations that perform required prerequisite actions before the actual building process starts.

[Recipes](working-with-meta-runner.md) are considered as a single step and thus cannot be split into parts performed before and after a checkout.

To turn a regular step into a bootstrap one in [](kotlin-dsl.md), add a `teamcity.step.phase` parameter and set its value to "bootstrap".

```Kotlin
object MyConfig : BuildType({
  // ...
  steps {
    script {
      name = "Disk prep"
      id = "simpleRunner"
      scriptContent = "echo 'Initial setup'"
      param("teamcity.step.phase", "bootstrap") // Bootstrap step
    }
    // Regular steps
  }
  // ...
})
```


## Agentless Build Steps

<secondary-label ref="secondary-config"/>

_Agentless build steps_ are steps that can run without a [build agent](install-and-start-teamcity-agents.md) attached, in an external software.

Normally, a running build occupies a build agent until the very finish, even if its final steps are executed outside TeamCity. However, if a build does not need its agent for some remaining steps, it can detach from this agent. The agent becomes available and can be instantly assigned to another build.

This approach allows saving agents' work time and is optimal for configurations that use third-party tools for finalizing a build: for example, to run numerous tests or deploy a project. Such a build can finish outside TeamCity; the TeamCity server will detect its status reports directly, without using an agent as a mediator.

Let's consider an example build that consists of three steps: compilation, testing, and deployment. All of them are processed by an agent even though the deployment step is actually performed by the external software. The agent is just polling the external software and reporting build statuses to the TeamCity server.

<img src="agent-depend-build.png" alt="Regular build" width="556"/>

With the agentless approach, the agent does not need to handle the final deployment step and can run other builds from the queue. The TeamCity server will catch the following reports directly from the external tool.

<img src="agentless-build.png" alt="Build with agentless step" width="518"/>

<video src="https://youtu.be/O7LdxtiBkLY"
title="New in TeamCity 2020.2: Agentless Build Steps"/>

See also: [](detaching-build-from-agent.md)



## Step Status Parameters

<secondary-label ref="secondary-config"/>

TeamCity provides `teamcity.build.step.status.<step_ID>` parameters that report the status of the step with the given ID. These parameters can be used, for instance, for crafting fine-grained [execution conditions](build-step-execution-conditions.md).

Step IDs are shown below their names, and are editable only when you create them.

<img src="dk-stepID.png" width="706" alt="Step ID"/>

The available values of the `teamcity.build.step.status.<step_ID>` parameters are:

* `success` — when a step finishes with no errors.
* `failure` — when a step failed. This status is reported even when all build problems were [muted](investigating-and-muting-build-failures.md).
* `cancelled` — when a build was cancelled while this step was running.

The `teamcity.build.step.status.<step_ID>` parameters appear only after their corresponding steps finish, and are not available right from the moment a build starts. This means neither steps that are still running, nor skipped steps have their `teamcity.build.step.status.<step_ID>` parameters available.

You can check all step statuses in the [](build-results-page.md#Parameters+Tab) of the build results page...

<img src="dk-parametersTab-statuses.png" width="706" alt="Step statuses"/>

...or via [TeamCity REST API](https://www.jetbrains.com/help/teamcity/rest/teamcity-rest-api-documentation.html).

```Shell
http://<SERVER_URL>/app/rest/builds/<BUILD_ID>/resulting-properties
```
{prompt="GET"}



## Additional Information

* You can copy a build step from one build configuration to another from the original build configuration settings page.
* You can reorder build steps as needed. Note that if you have a build configuration inherited from a template, you cannot reorder inherited build steps. However, you can insert custom build steps (not inherited) at any place and in any order, even before or between inherited build steps. Inherited build steps can be reordered in the original template only.
* You can disable a build step temporarily or permanently, even if it is inherited from a build configuration template, using the corresponding option in the last column of the __Build Steps__ list.
