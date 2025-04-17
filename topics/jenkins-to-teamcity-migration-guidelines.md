[//]: # (title: Jenkins to TeamCity Migration Guidelines)
[//]: # (help-id: Jenkins to TeamCity Migration Guidelines)


## Overview

Migrating from Jenkins to TeamCity can enhance your CI/CD workflows with powerful build configuration options, robust [integrations](https://www.jetbrains.com/teamcity/integrations/), and intelligent [automation features](https://www.jetbrains.com/teamcity/features/build-automation/).

This guide provides a comprehensive overview of the migration process to help you transition smoothly.

> Considering migrating your CI/CD process from Jenkins to TeamCity?
>
> We offer professional services to help make your migration seamless. Reach out to us [using the form](https://www.jetbrains.com/teamcity/get-in-touch/), and we’ll be happy to discuss the best options for your needs.
>
{style="note"}

## TeamCity vs Jenkins: Key Similarities and Differences

TeamCity and Jenkins are both popular CI/CD tools used for automating builds, testing, and deployments. While they share some core functionalities, there are also notable distinctions between the two.

<procedure title="Similarities" type="choices">

* Both tools support pipelines as a way to structure build and deployment workflows (although they use slightly different terminology and approaches).
* Both TeamCity and Jenkins are highly configurable and customizable through plugins or extensions.
* Both tools support integration with container-based workflows, including Docker.

</procedure>


<procedure title="Differences" type="choices">

* **Configuration format**. TeamCity build routines are configured using a combination of a web-based UI and Kotlin-based configuration scripts for version-controlled setups. Jenkins, on the other hand, uses declarative pipelines (Groovy-based files) or scripted pipelines (Jenkins DSL).
* **Hosting options**. TeamCity offers both a self-hosted solution and a SaaS offering (through JetBrains-hosted cloud instances). Jenkins is exclusively self-hosted, requiring users to manage their own infrastructure.
* **Ease of setup and maintenance**. TeamCity provides a more user-friendly setup process and a polished interface out of the box. Jenkins typically requires more manual setup and configuration and relies heavily on plugins, which may increase maintenance overhead.
* **Built-in features vs plugins**. TeamCity includes built-in support for managing build agents, testing frameworks, code quality checks, and reporting tools. Jenkins’s functionality is heavily reliant on third-party plugins, which need to be selected and configured separately to achieve similar capabilities.
* **Integration with development tools**. Because TeamCity is developed by JetBrains, it tightly integrates with their ecosystem of developer tools, such as IntelliJ IDEA and other JetBrains IDEs. Jenkins provides integrations with many tools but typically requires plugins or custom configurations for setting up these connections.
* **Cost**. TeamCity's free tier limits the number of build agents and configurations, with additional costs for scaling up. Jenkins, being open-source, has no upfront costs, but hosting and plugin maintenance must be considered as operational expenses.

</procedure>



## Feature and Concept Comparison

<procedure title="Configuration files" type="steps">

* Jenkins uses a Jenkinsfile, written in Groovy. It defines the pipeline steps in code.
* TeamCity uses a [Kotlin or XML settings file](storing-project-settings-in-version-control.md). TeamCity can automatically generate settings from a project configured in the UI, and you can edit it locally with full IDE support. Settings files can be stored either alongside the built project, or a completely [separate remote repository](storing-project-settings-in-version-control.md#Choosing+the+Settings+Location).

</procedure>


<procedure title="Build definition" type="steps">

* Jenkins jobs are either configured through the UI or defined in the Jenkinsfile as scripted or declarative pipelines.
* Builds produced by build configurations, owned by parent projects. Each configuration can have multiple steps, triggers, parameters, and so on.

</procedure>

<procedure title="Variables" type="steps">

* Jenkins variables are set through environment blocks or <code>params {}</code> inside the pipeline script. Can also be configured in the UI.
* TeamCity uses parameters, defined per build configuration, project, or globally. These include environment variables passed to the agent's machine, and configuration parameters. Parameters can be <a href="use-parameters-in-build-chains.md">shared with other configurations</a> (output parameters) or configured to be private (input parameters).

</procedure>


<procedure title="Conditional steps" type="steps">

* Jenkins supports conditional execution via <code>when {}</code> blocks in declarative pipelines or Groovy if in scripted ones.
* TeamCity <a href="build-step-execution-conditions.md">step conditions</a> can be set in the UI or in Kotlin DSL. Supports a wide range of conditions: you can execute a step only when another step is successful, only if it fails, only if the specific parameter has a required value, and so on.

</procedure>


<procedure title="Artifact management" type="steps">

* In Jenkins, you must explicitly declare what files to archive using <code>archiveArtifacts</code>.
* You can use TeamCity UI, Kotlin DSL, or <a href="service-messages.md#Publishing+Artifacts+While+Build+is+in+Progress">manually sent service messages</a> to publish files produced during a build as an artifact. <a href="artifact-dependencies.md">Artifact dependencies</a> allow sharing published artifacts across various configurations.

</procedure>


<procedure title="Container managers" type="steps">

* Jenkins supports Docker via the Docker Pipeline plugin or Kubernetes plugin, but you need to install and configure them.
* TeamCity has [built-in support for Docker and Podman](integrating-teamcity-with-container-managers.md), including Docker steps, agent images, and service containers. No plugins needed.

</procedure>


<procedure title="Build agents" type="steps">

* Jenkins agents must be set up and connected manually (SSH, Docker, etc.). Agent templates via Kubernetes require plugin setup.
* TeamCity supports cloud agents (JetBrains-hosted), Docker-based agents, and traditional self-hosted agents, with minimal config.

</procedure>

<procedure title="Parallel execution" type="steps">

* Jenkins supports parallel blocks in scripted pipelines or parallel stages in declarative pipelines.
* TeamCity can run builds or build steps in parallel, using [build dependencies](snapshot-dependencies.md), [splitting tests into batches](parallel-tests.md), and agent-side parallelism.

</procedure>


<procedure title="Build logs" type="steps">

* Jenkins outputs build results to console by default. You can enhance logs with timestamps and colors using plugins. Search and navigation are limited.
* TeamCity offers [real-time log streaming](build-log.md) with timestamps and highlighting, [server load metrics in Prometheus format](teamcity-monitoring-and-diagnostics.md), and archived history per build step. It’s possible to integrate TeamCIty with 3rd party observability solutions like Dynatrace, Grafana, etc.

</procedure>


For a more detailed  feature comparison between TeamCity and Jenkins, please refer to [this document](https://resources.jetbrains.com/storage/products/teamcity/docs/TeamCity_vs_Jenkins_comparison_doc.pdf).


## Matching Jenkins Plugins to TeamCity Features
Unlike Jenkins, where many core features rely on third-party plugins, TeamCity comes with most essential functionality — like Git and Docker support, test reporting, secrets management, and notifications — built in. This means fewer moving parts to maintain, no plugin compatibility issues during upgrades, and a more stable, out-of-the-box experience.

Here’s how some of Jenkins plugins match to the same built-in functionality in TeamCity.

<table>

<tr>

<td>Jenkins plugin</td>
<td>Equivalent TeamCity feature</td>

</tr>

<tr>

<td><a href="https://plugins.jenkins.io/workflow-aggregator/">Pipeline</a></td>
<td>Build configurations & Kotlin DSL</td>

</tr>

<tr>

<td><a href="https://www.jenkins.io/doc/book/blueocean/">Blue Ocean</a></td>
<td><a href="build-chain.md">Build chains</a></td>

</tr>


<tr>

<td><a href="https://plugins.jenkins.io/credentials/">Credentials plugin</a></td>
<td><a href="typed-parameters.md#Create+a+Secret">Various parameters</a> for different use cases: secrets stored on the TeamCity server, parameters of the "password" type, and remote parameters that retrieve sensitive data from external storages like HashiCorp Vault</td>

</tr>


<tr>

<td>Artifactory plugin</td>
<td>Built-in artifact storage and cloud/on-premises repository support. Support for S3 buckets and other cloud storage providers.</td>

</tr>

<tr>

<td>Git plugin</td>
<td>First-class Git support with deep VCS integrations (GitHub, GitLab, Bitbucket, Azure Repos, and more).</td>

</tr>

<tr>

<td>Docker plugin</td>
<td>Built-in <a href="integrating-teamcity-with-container-managers.md">Docker and Podman support</a>: use Docker as build environment, pull services, build/push images</td>

</tr>

<tr>

<td>Slack notification plugin</td>
<td>Built-in <a href="notifications.md">notifications</a>: Slack, email, Microsoft Teams, webhook- and service message-based notifications with customizable templates.</td>

</tr>

<tr>

<td>Kubernetes plugin</td>
<td>Various levels of built-in of Kubernetes integration: <a href="setting-up-teamcity-for-kubernetes.md">set up a Kubernetes cloud profile</a> or use your cluster as an <a href="kubernetes-executor.md">external executor</a> processing TeamCity builds.</td>

</tr>

<tr>

<td>JUnit plugin</td>
<td>Native support for parsing JUnit test results, visual test reports, test history and flaky test detection.</td>

</tr>

<tr>

<td>HTML publisher plugin</td>
<td>Built-in support for publishing and viewing <a href="including-third-party-reports-in-the-build-results.md">HTML build reports</a>.</td>

</tr>

<tr>

<td>Throttle concurrent builds plugin</td>
<td>Smart queue prioritization, build limits per project/agent, and <a href="configuring-agent-requirements.md">agent requirements logic</a>.</td>
</tr>

<tr>

<td>AnsiColor plugin</td>
<td>Native log output with colored ANSI support and syntax highlighting in build logs</td>

</tr>

<tr>

<td>Timestamper plugin</td>
<td>TeamCity logs include structured timestamps, log folding, and filtering built-in</td>

</tr>

<tr>

<td>Workspace cleanup plugin</td>
<td>Built-in workspace and artifact <a href="teamcity-data-clean-up.md">cleanup rules</a> with customizable retention policies.</td>

</tr>


<tr>

<td>Build timeout plugin</td>
<td>Flexible build timeout options (absolute, inactivity, custom logic) directly in build configuration settings.</td>

</tr>


<tr>

<td>Parameterized trigger plugin</td>
<td>Built-in <a href="snapshot-dependencies.md">snapshot</a> and <a href="artifact-dependencies.md">artifact</a> dependencies trigger builds with specific parameters.</td>

</tr>

</table>


## Migration Examples

In Jenkins, a job is a fundamental unit that defines a sequence of tasks. In TeamCity, this concept is represented by a build configuration. Each build configuration specifies a set of build steps, triggers, and other settings necessary to perform a build.

Jenkins defines build pipelines using a Jenkinsfile written in Groovy. This file typically lives in the root of your repository and describes the steps and stages of your build process.

### Example 1. Basic pipeline configuration

Jenkins configuration:

```Groovy
pipeline {
agent any

    stages {
        stage('hello') {
            steps {
                echo "Hello World"
            }
        }
    }
}
```

In TeamCity, you can choose how to configure pipelines: via the UI or using the Kotlin DSL. Some benefits of using the Kotlin DSL for pipeline configuration include:

* Human-readability and maintainability
* Version control: Store configurations in any supported VCS for better traceability
* Extensibility: Supports loops, conditionals, and reusable components. Think a fully-fledged programming language rather than just a markup like XML, XAML, or YAML, which means custom data classes, custom libraries, and so on.
* UI synchronization: Changes made in the UI can be reflected in the DSL and vice versa.

If you proceed with the Kotlin DSL for configuration, instead of a single file, your setup lives in a [remote directory](storing-project-settings-in-version-control.md) (by default, the hidden `.teamcity` folder in the repository root) and is written in type-safe Kotlin code. This makes it easier to catch errors early and leverage IDE features like code completion.

Here’s the equivalent setup to the previously described Jenkins file in TeamCity Kotlin DSL:

```Kotlin
import jetbrains.buildServer.configs.kotlin.*

project {
    buildType(HelloBuild)
}

object HelloBuild : BuildType({
    name = "Hello Build"

    steps {
        script {
            name = "Say Hello"
            scriptContent = "echo Hello World"
        }
    }
})
```

This Kotlin code defines a project with a single build configuration that runs a script printing Hello World. TeamCity automatically picks up this configuration when connected to your repo.

### Example 2: Simple build job

Jenkins:

```Groovy
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
    }
}
```

In TeamCity, you can use the all-purpose [command-line step](command-line.md) to perform regular building actions, or specialized build steps tailored to interact with specific build tools: [](net.md), [](maven.md), [](python.md), [](gradle.md), and more.

TeamCity Kotlin DSL (CLI step):

```Kotlin
import jetbrains.buildServer.configs.kotlin.*

project {
    buildType(SimpleBuild)
}

object SimpleBuild : BuildType({
    name = "Simple Build"

    steps {
        script {
            name = "Maven Build"
            scriptContent = "mvn clean package"
        }
    }
})
```


Via the TeamCity UI (Maven step):

> Watch the [interactive demo](https://app.storylane.io/share/sajtxyc3yrn9) for an example.
>
{style="note"}

* [Create a build configuration](creating-and-editing-build-configurations.md).
* [Add a Maven build step](configuring-build-steps.md).
* Set the **Goal** property of a Maven step to `clean package`.


### Example 3: Running tests

Jenkins:

```Groovy
pipeline {
    agent any
    stages {
        stage('Test') {
            steps {
                sh 'pytest tests/'
            }
        }
    }
}
```

TeamCity Kotlin DSL:

```Kotlin
import jetbrains.buildServer.configs.kotlin.*

project {
    buildType(RunTests)
}

object RunTests : BuildType({
    name = "Run Tests"

    steps {
        script {
            name = "Run Pytest"
            scriptContent = "pytest tests/"
        }
    }

    requirements {
        contains("teamcity.agent.jvm.os.name", "Linux") // Optional: adjust based on your environment
    }
})
```


TeamCity UI:

* [Create a build configuration](creating-and-editing-build-configurations.md).
* [Add a build step](configuring-build-steps.md) of a [command-line](command-line.md) or [](python.md) type.
* Specify the `pytest tests/` step script.

### Example 4: Deploying to production

Jenkins:

```Groovy
pipeline {
    agent any
    stages {
        stage('Deploy') {
            steps {
                sh 'kubectl apply -f deployment.yaml'
            }
        }
    }
}
```

TeamCity Kotlin DSL:

```Kotlin
import jetbrains.buildServer.configs.kotlin.*

project {
    buildType(DeployToProduction)
}

object DeployToProduction : BuildType({
    name = "Deploy to Production"

    steps {
        script {
            name = "Apply Kubernetes Deployment"
            scriptContent = "kubectl apply -f deployment.yaml"
        }
    }

    requirements {
        contains("teamcity.agent.jvm.os.name", "Linux") // Adjust if needed
    }
})
```

Via the TeamCity UI:

* Create a [deployment configuration](deployment-build-configuration.md).
* [Add a CLI build step](configuring-build-steps.md).
* Set the `kubectl apply -f deployment.yaml` command.



## Triggers

Both Jenkins and TeamCity support build triggers to automate the initiation of builds. In Jenkins, triggers like `cron`, `pollSCM`, and `GitHub webhook triggers` are commonly used.

In TeamCity, various [triggers](configuring-build-triggers.md) are available to provide similar automation.

<include from="configuring-build-triggers.md" element-id="triggers-list"/>

By leveraging these triggers, you can ensure efficient automation in your CI/CD workflow, reducing manual intervention and improving deployment speed.

## Environment variables
Jenkins allows defining environment variables within the `environment` block. In TeamCity, environment variables are managed as [build parameters](configuring-build-parameters.md) that can be defined at the project or build configuration level.

Jenkins:

```Groovy
pipeline {
   environment {
      API_KEY = 'my_secret_key'
   }
   stages {
      stage('Build') {
         steps {
            echo "Using API Key: ${API_KEY}"
         }
      }
   }
}
```

TeamCity Kotlin DSL:

```Kotlin
import jetbrains.buildServer.configs.kotlin.*

project {
    buildType(UseApiKey)
}

object UseApiKey : BuildType({
    name = "Use API Key"

    params {
        password("API_KEY", "credentialsJSON:*****") // Replace with your actual secure credential reference
    }

    steps {
        script {
            name = "Echo API Key"
            scriptContent = "echo \"Using API Key: %API_KEY%\""
        }
    }
})
```

Equivalent in the TeamCity UI:

* Open build configuration or project settings and [create a parameter](typed-parameters.md) named **env.API_KEY**.
* Specify a parameter value.
* Reference it in a build step or configuration settings via the `%\env.API_KEY%` syntax.



## Post-build actions

In Jenkins, post-build actions are defined in the `post` section and include tasks such as notifications, archiving artifacts, or triggering other builds.

In TeamCity, similar functionality can be achieved using [Build Features](adding-build-features.md) or additional [Build Steps](configuring-build-steps.md) configured to execute based on success or failure conditions.

* [Artifacts](build-artifact.md) configured when you set up a VCS connection, inside a VCS root. Run your build at least once, and you'll be able to use the file/folder browser to easily select produced files that should be published as artifacts
* [](notifications.md) are set up on the per-build configuration basis
* Delivery tasks are performed by the [Deployment configurations](deployment-build-configuration.md) and [](deployers.md)

## Planning the migration

### Audit Your Jenkins Setup

Start by understanding everything that's currently running in Jenkins. This will help you avoid surprises and ensure feature parity in TeamCity.

* **Inventory all Jenkins jobs and pipelines**

    Export a full list of pipelines, whether scripted or declarative. Pay attention to naming conventions, folder structures, triggers, and branching strategies.


* **List all plugins in use**

    Use [](#Matching+Jenkins+Plugins+to+TeamCity+Features) to generate a list. For each plugin, note its purpose and whether it has a counterpart in TeamCity (many features are built-in).

* **Document external integrations**

    Identify how Jenkins communicates with tools like artifact repositories (Artifactory, Nexus), container managers (Docker, Podman, Kubernetes), scret managers (Vault, AWS Secrets Manager), notification tools (Slack, email, MS Teams), infrastructure tools (Terraform, Ansible, etc.)


* **Review authentication and access control**

    Are you using SSO, LDAP, GitHub OAuth, or manual users? Document roles and permissions for migration to TeamCity’s user/group model.


* **Measure performance and resource usage**

    Collect metrics like build durations, queue times, and agent utilization. This gives you a baseline for comparing post-migration performance.

### Prepare the TeamCity environment

Before starting the actual migration, make sure your TeamCity setup is ready.

* **Spin up a TeamCity instance**
   Choose between TeamCity Cloud (fully managed by JetBrains) or TeamCity On-Premises (hosted and maintained by your team).

* **Provision Build Agents**

    TeamCity requires build agents to execute jobs. Decide whether you'll use:
        
    * Self-hosted static agents (e.g., bare metal or VMs)
    * Cloud-based agents (e.g., AWS, GCP, Azure)
    * On-demand agents (with support for Docker/Kubernetes)


* **Connect your Version Control Systems**

    Add your repositories via VCS Roots. TeamCity supports GitHub / GitHub Enterprise, GitLab, Bitbucket Cloud, Server and Data Center, Bitbucket Cloud, Azure Repos, Perforce Helix, and other VCS providers via SSH/HTTP(S) protocols.


* **Learn the basics of TeamCity structure**

    TeamCity organizes work differently than Jenkins. Basic concepts include:

    * Projects – the top-level container
    * Build сonfigurations – similar to Jenkins jobs
    * Templates – reusable build logic
    * Build сhains – visualize and control dependencies
    * Kotlin DSL – define build logic as code, versioned in Git
    Please refer to the [](#Feature+and+Concept+Comparison) part of this guide.

* **Set up secrets and credentials**

    Define secure parameters in TeamCity for API keys, tokens, and passwords. Map any Jenkins credentials to these.

## Conclusion

Migrating from Jenkins to TeamCity offers greater efficiency, enhanced automation, and reduced dependency on plugins. We hope that this guide can serve as a good starting point in helping you to migrate from Jenkins to TeamCity.




<!--## Introduction
{instance="tc"}

This document provides the basics you need to know when migrating from Jenkins to a TeamCity CI server. TeamCity is quite different from Jenkins in terms of concepts related to managing CI and CD. To learn more about Continuous Delivery and get tips on building a production pipeline, follow our [CI/CD Guide](https://www.jetbrains.com/teamcity/ci-cd-guide/). If you want to jump in and get started with TeamCity right away, see the [Getting Started](getting-started-with-teamcity.md) instructions.

## Introduction
{instance="tcc"}

This document provides the basics you need to know when migrating from Jenkins to a TeamCity CI server. TeamCity is quite different from Jenkins in terms of concepts related to managing CI and CD. If you just want to jump in and get started, try to follow [this guide](getting-started-with-teamcity-cloud.md).


## Concepts

Jenkins and TeamCity mostly feature the same set of concepts, however, with slightly different naming. The following table provides a mapping for some of the Jenkins concepts to the TeamCity counterparts.

<snippet id="jenkins-mapping-to-teamcity">

<table><tr>

<td>

<b>Jenkins</b>

</td>

<td>

<b>TeamCity</b>

</td></tr><tr>

<td>

Jenkins Master/Node

</td>

<td>

TeamCity server

</td></tr><tr>

<td>

Dumb Slave / Permanent Agent

</td>

<td>

[Agent Pool](configuring-agent-pools.md)

</td></tr><tr>

<td>

Executor

</td>

<td>

[TeamCity Agent](install-and-start-teamcity-agents.md)

</td></tr><tr>

<td>

View or [Folder](https://plugins.jenkins.io/cloudbees-folder)

</td>

<td>

[Project](project-administrator-guide.md#Steps%2C+Configurations+and+Projects)

</td></tr><tr>

<td>

Job/Item/Project

</td>

<td>

[Build configuration](managing-builds.md)

</td></tr><tr>

<td>

Build

</td>

<td>

[Build Steps](configuring-build-steps.md)

</td></tr><tr>

<td>

Pre Step

</td>

<td>

[Build Steps](configuring-build-steps.md) (partially)

</td></tr><tr>

<td>

Post\-build Action

</td>

<td>

[Build Steps](configuring-build-steps.md)   (partially)

</td></tr><tr>

<td>

Build Triggers

</td>

<td>

[Build Triggers](configuring-build-triggers.md)

</td></tr><tr>

<td>

Source Control Management (SCM)

</td>

<td>

[Version Control System (VCS)](configuring-vcs-roots.md)

</td></tr><tr>

<td>

Workspace

</td>

<td>

[Build Checkout Directory](build-checkout-directory.md)

</td></tr><tr>

<td>

Pipeline

</td>

<td>

[Build Chain](build-chain.md) (via snapshot dependencies)

</td></tr><tr>

<td>

Label

</td>

<td>

[Agent Requirements](configuring-agent-requirements.md)

</td></tr></table>

</snippet>

## Migrating Freestyle project

A Freestyle project is the most common project type in Jenkins so we describe the TeamCity counterparts for a Freestyle project to guide the migration.

### Project and Build Configuration

There are some conceptual differences in how the build jobs are configured in Jenkins and TeamCity.

[Build Configuration](managing-builds.md) is the TeamCity's counterpart of Jenkins Job/Item/Project. However, a Build Configuration requires a [Project](project-administrator-guide.md#Steps%2C+Configurations+and+Projects) instance to be created first. In fact, the notion of Project in TeamCity is the first big difference that a user encounters when migrating from Jenkins. The Project contains a good portion of settings required for the build configurations.

### VCS Roots

Source Code Management (SCM) of a Jenkins build job corresponds to a [VCS root](configuring-vcs-roots.md) in Project Settings in TeamCity. A Project may include an arbitrary number of VCS Roots. Any Build Configuration may use any number of VCS Roots of the Project.

When only a part of the VCS Root is required, it is possible to use [VCS Checkout Rules](vcs-checkout-rules.md). This allows mapping the directories from the configured VCS Root to subdirectories in the [build checkout directory](build-checkout-directory.md) on a Build Agent.

### Build Environment

The Build Environment section of the Freestyle project in Jenkins specifies additional features for the project. In TeamCity, the corresponding features may be configured in various sections, depending on the goal: general project settings, build configuration settings, VCS root settings, and so on.

Examples:

* To clean the workspace before the build, enable the [Clean build](configuring-vcs-settings.md#Checkout+Settings) checkbox in Version Control Settings of a Build Configuration.
* To cancel the build if the process is stuck, configure [failure conditions](build-failure-conditions.md) of a Build Configuration. What is more, TeamCity provides [hanging build detection](configuring-general-settings.md#Build+Options) out of the box.
* Environment variables can be accessed by specifying a special `%env.<environment variable name>%` pattern (example: `%\env.JAVA_HOME%`) in various places of the configuration.

### Triggers

A Freestyle project allows configuring optional triggers to control when Jenkins will start the builds. In most cases, a trigger in a Jenkins project will be a counterpart in TeamCity.

A [number of options](configuring-build-triggers.md) for triggering builds in TeamCity are available. It is possible to watch for changes in a source control repository, monitor an external resource, or even a Maven dependency. It is possible to schedule builds periodically, besides, the [REST API](https://www.jetbrains.com/help/teamcity/rest/teamcity-rest-api-documentation.html) is available to trigger builds externally.

### Build

This is where the real work happens. This part is mostly identical in Jenkins and TeamCity. TeamCity provides a large number of [build steps](configuring-build-steps.md) out of the box. You can configure several build steps to run the required tasks for the given Build Configuration.

### Building Maven project

A Jenkins Maven project doesnt have a direct counterpart in TeamCity. Instead, an appropriate build step is selected to perform the work. In fact, [TeamCity automates](maven.md) a lot of the setup for Maven projects and provides an additional build triggering [option based on dependencies](configuring-maven-triggers.md#Maven+Artifact+Dependency+Trigger).

### Post-build Actions

Post\-build Actions of the Freestyle project can be mapped either to the specific attributes of a Project/Build Configuration in TeamCity or by using an additional build step. You may also find a relevant solution to the task by adding [Build features](adding-build-features.md) to the Build Configuration in TeamCity.

An individual build step may be [configured to execute](configuring-build-steps.md) depending on the success or failure of the previous build step(s) of the same Build Configuration.

### Working with branches

TeamCity provides support for the [Feature Branches](working-with-feature-branches.md) in distributed version control systems (DVCS).

The support for Feature Branches integrates with the various TeamCity functions. For instance, the [Branch Remote Run Trigger](branch-remote-run-trigger.md) automatically starts a new personal build each time TeamCity detects changes in particular branches of the VCS roots of the build configuration.

Heres a list of some of the highlights related to Feature Branches in TeamCity:
* The build branch name can be specified in an artifact dependency
* The VCS, Schedule, and Finish build triggers support the branch filter setting
* VCS labeling also supports the branch filter
* Mercurial bookmarks can be used in the branch specification, so, if you prefer to use bookmarks instead of standard Mercurial branches, you can fully use the TeamCity feature branches with them
* Git tags can be used in the branch specification. Some teams use Git tags the same way as branches, i.e., once a new tag is set, a build should be started in TeamCity in a branch designated by the tag name.
* [Automatic Merge](automatic-merge.md) functionality to merge a branch into another after a successful build. The functionality is available as a [Build feature](adding-build-features.md) option in Build Configuration settings.

### Plugins

TeamCity comes with a lot of features built\-in by default, and can be further extended by installing [additional plugins](https://plugins.jetbrains.com/teamcity).

  

### Build pipelines

A build pipeline in TeamCity is called a [Build Chain](build-chain.md). It is a set of Build Configurations connected via [snapshot dependencies](snapshot-dependencies.md). The [TeamCity Take on Build Pipelines](https://blog.jetbrains.com/teamcity/2016/03/teamcity-take-on-build-pipelines/) article describes in detail how TeamCity handles build chains and what the implications are.

### Distributed builds

In Jenkins, to offload the master node, a permanent agent is configured to run builds.

TeamCity server doesnt run any builds itself. Instead, it always delegates the job to a [build agent](install-and-start-teamcity-agents.md), which means in TeamCity builds are distributed by design.

The active agents count is visible at the top of the TeamCity server UI:

<img src="agentCount.png" width="300"/>

You can break the agents into separate groups called [agent pools](configuring-agent-pools.md) and assign those to the specific projects. In Build Configuration settings, it is possible to specify a number of [Agent Requirements](configuring-agent-requirements.md) needed for the build.

-->