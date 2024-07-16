[//]: # (title: Deployment Build Configuration)
[//]: # (auxiliary-id: Deployment Build Configuration)

When your project is built and tested, you often need to deploy it to its final infrastructure. For example, upload a package to the NuGet Gallery, deliver a container to a DockerHub repository, or update your documentation website sources. Different CI/CD solutions use different terms for this last step of a pipeline: "deploy" stage, delivery target, release, production, and so on.



 In TeamCity, delivery tasks are performed by the same "build configuration" objects that carry out regular building routines. However, since building and delivery are different tasks triggered by different team members, a configuration that deploys a product can be explicitly marked as a **Deployment Configuration**.

## Key Takeaways

* Deployment configurations are designed to differentiate configurations that perform regular building routines from configurations that deliver apps to external infrastructures.
* Deployment configurations do not differ from regular configurations in terms of functionality. They can utilize the same build features, employ same build runners, and so on.
* If a deployment configuration follows a regular building/testing configuration within the same chain, TeamCity users with sufficient permissions can trigger deployment directly from regular configurations.
* Deployment configurations do not allow running personal builds. Also, you cannot simultaneously run two builds that belong to the one deployment configuration.
* Splitting deployment and building/testing configurations into different subprojects is recommended to set up fine-grained user permissions.
* To switch a build configuration type, navigate to the **Configuration settings | General** tab.

## Example

In this tutorial, you will create deployment configurations to supplement a pipeline developed in the [](composite-build-configuration.md) article.

<img src="dk-deploymentConf-finalChainWide.png" width="706" alt="Complete Delivery Chain"/>


### Create a Building Pipeline

Follow the steps described in the [](composite-build-configuration.md) article to create a chain of five interconnected configurations that end with the composite "Build All" configuration.

<img src="dk-compositeConf-failing.png" width="706" alt="Composite build failing"/>

### Create a Deployment Subproject

Regular project developers who can edit and run building configurations should not be able to run delivery tasks. To set up fine-grained user permissions and securely store delivery-related parameters and credentials away from the main building pipeline, we recommend creating delivery configurations in a separate project.

1. Go to **Administration | &lt;Your project&gt;** and click **Create subproject**.
2. Choose the **Manually** tile to create a blank project unrelated to any specific VCS repository.
3. Set the subproject name to *"Deployment Configurations"*.
4. Depending on the type of your project and delivery target, deployment configurations can include different steps. For example, your configuration can employ the [](net.md) runner that executes the `nuget push` command to upload a package to a NuGet Gallery, or one of [](deployers.md) that upload files to an FTP server or Windows shares.
    
    In this sample, deployment configurations upload Docker images to a DockerHub registry. For that reason, you need to create a [Docker connection](configuring-connections.md#Docker+Registry) that specifies the registry address and your login credentials. This connection will be available for all configurations in this subproject. To create a connection, navigate to the subproject's **Connections** tab.

The [Kotlin](kotlin-dsl.md) code below illustrates the final setup.

```Kotlin
// Main Project
project {
    buildType(Building_1) // The "Build All" configuration
    subProject(BuildingConfigsProject) // The subproject with building configs
    subProject(DeploymentConfigsProject) // Our new subproject for deployment
    
}

// ...

object DeploymentConfigsProject : Project({
    name = "Deployment Configurations"
    description = "This subproject contains configurations that perform delivery tasks"

    features {
        dockerRegistry {
            id = "PROJECT_EXT_5"
            name = "Docker Registry"
            userName = "your_dockerhub_name"
            password = "credentialsJSON:0ff181ee-cc10-48ac-b5f4-ce50ca2013b4"
        }
    }
})
```

### Add Your First Deployment Configuration

1. On the **General Settings** tab of your new subproject, click **Create build configuration**.
2. Choose the **Manually** option and enter *"Deploy Console (Windows)"* as the configuration name.
3. Set the build configuration type to "Deployment".
4. In the **Version Control Settings**, click **Attach VCS root** and choose the same root all *"Build..."* configurations use.
5. *"Deploy Console (Windows)"* depends on the *"Build console & web (win-x64)"* build configuration and must be able to access its artifacts.
    
    Navigate to **Building Configuration | Dependencies"** and add corresponding snapshot and artifact (`bin => context`) dependencies.
  
6. Since we need to upload generated containers, add the [](docker-support.md) build feature that uses a Docker connection created in the [](#Create+a+Deployment+Subproject) section.
7. Add three build steps to your configuration:

   * Step #1 — the [Docker](docker.md) runner that pulls (or updates, if already pulled) a required [.NET Runtime container](https://mcr.microsoft.com/product/dotnet/runtime/about) image.
   * Step #2 — the [Docker](docker.md) runner that builds an image using instructions in `context/console.windows.dockerfile`.
   * Step #3 — another Docker runner step that publishes a newly built image.

The final configuration setup should look like the following:

```Kotlin
object DeploymentConfigsProject_DeployConsoleWindows : BuildType({
  name = "Deploy Console (Windows)"

  enablePersonalBuilds = false
  type = BuildTypeSettings.Type.DEPLOYMENT
  maxRunningBuilds = 1

  vcs {
    root(DslContext.settingsRoot)
  }

  steps {
    dockerCommand {
      name = "Pull container"
      commandType = other {
        subCommand = "pull"
        commandArgs = "mcr.microsoft.com/dotnet/runtime:7.0"
      }
    }
    dockerCommand {
      name = "Build container"
      commandType = build {
        source = file {
          path = "context/console.windows.dockerfile"
        }
        contextDir = "context"
        platform = DockerCommandStep.ImagePlatform.Windows
        namesAndTags = "username/clock-console:windows"
        commandArgs = "--build-arg baseImage=mcr.microsoft.com/dotnet/runtime:7.0"
      }
    }
    dockerCommand {
      name = "Push container"
      commandType = push {
        namesAndTags = "username/clock-console:windows"
      }
    }
  }

  features {
    dockerSupport {
      loginToRegistry = on {
        dockerRegistryId = "PROJECT_EXT_5"
      }
    }
  }

  dependencies {
    dependency(Building_BuildConsoleWebWinX64) {
      snapshot {
      }

      artifacts {
        artifactRules = "bin => context"
      }
    }
  }

  requirements {
    contains("teamcity.agent.os.name", "windows-server-2022")
  }
})
```

### Run a Delivery Configuration

The code above highlights the first unique feature of deployment build configurations: its default settings differ from the settings of regular configurations.

* The **Allow triggering personal builds** option is disabled.
* The **Total maximum builds in the configuration** is limited to 1 to avoid potential issues caused by multiple builds trying to access the same delivery destination simultaneously.

Other unique features of deployment build configurations include:

* The button that starts a deployment build is called **Deploy** instead of **Run**.
    
    <img src="dk-deploymentConf-button.png" width="706" alt="Deployment button"/>

* You can deploy artifacts directly from configurations that produced them. Open the details of a finished regular build and click **Deploy** under the "Deployments" section.
    
    <img src="dk-deploymentConf-DeployFromBuild.png" width="706" alt="Run deployment from regular builds"/>
    
    When a deployment build starts, you can track its progress from the "Deployments" section. When it finishes, the **Redeploy** button appears. This button allows you to re-run the delivery routine.
    
    <img src="dk-deploymentConf-redeploy.png" width="706" alt="Redeploy Delivery tasks"/>

    If a build configuration's artifacts are already deployed, earlier builds warn users that triggering a deployment configuration will override newer deliveries.
    
    <img src="dk-deploymentConf-rollback.png" width="706" alt="Rollback deployment"/>

* The **Changes** and **Change Log** tabs of TeamCity projects, configurations, and individual builds allow you to click revision numbers to view detailed information about each change. If your pipeline has delivery configurations, this change details page displays the **Deployments** tab that allows you to quickly identify when this change was delivered for the first time.
    
    <img src="dk-deploymentConf-changes.png" width="706" alt="Delivery for changes page"/>

* Regular build configurations show builds with the latest changes first. Deployment configurations arrange their builds chronologically.

### Add More Delivery Configurations

1. Navigate to the *"Deploy Console (Windows)"* configuration settings and click **Actions | Copy Configuration** to create three copies of your delivery configuration.
2. Modify the new copies as follows:

   * **Deploy Web (Windows)** — uses instructions from `context/web.linux.dockerfile` to build an image and pushes this image into a separate "docker_username/clock-web" registry.
   * **Deploy Console (Linux)** — has snapshot and artifact dependencies to the *Build console &amp; web (linux-x64)* configuration. Utilizes a different, Linux-based image in its build steps.
   * **Deploy Web (Linux)** — both of the above. 
    
3. Navigate to the parent (topmost) project and create a new step-less configuration next to the existing *"Build All"*. Call this new configuration *"Deploy All"* and set its type to "Deployment".
4. In the *"Deploy All"* configuration settings, add snapshot dependencies to all four individual "*Deploy...*" configurations.

You should end up with four independent deployment configurations and two configurations on the main project level that conveniently run all building and deployment tasks.

<img src="dk-deploymentConf-finalChain.png" width="706" alt="Final Build Chain"/>

Note that with this setup, build results pages of individual *"Build..."* configuration builds show multiple options in their "Deployments" section. When a build finishes, you can trigger any related deployment target.

<img src="dk-deploymentConf-multipleDepTargets.png" width="706" alt="Multiple Deployment Targets"/>

The final configs of all *"Deploy..."* configurations is shown below.

<tabs>
<tab title="Deploy All">

```Kotlin
object DeployAll : BuildType({
    name = "Deploy All"

    enablePersonalBuilds = false
    type = BuildTypeSettings.Type.DEPLOYMENT
    maxRunningBuilds = 1

    dependencies {
        snapshot(DeploymentConfigsProject_DeployConsoleLinux) {
        }
        snapshot(DeploymentConfigsProject_DeployConsoleWindows) {
        }
        snapshot(DeploymentConfigsProject_DeployWebLinux) {
        }
        snapshot(DeploymentConfigsProject_DeployWebWindows) {
        }
    }
})
```

</tab>

<tab title="Deploy Console (Windows)">

```Kotlin
object DeploymentConfigsProject_DeployConsoleWindows : BuildType({
    name = "Deploy Console (Windows)"

    enablePersonalBuilds = false
    type = BuildTypeSettings.Type.DEPLOYMENT
    maxRunningBuilds = 1

    vcs {
        root(DslContext.settingsRoot)
    }

    steps {
        dockerCommand {
            name = "Pull container"
            commandType = other {
                subCommand = "pull"
                commandArgs = "mcr.microsoft.com/dotnet/runtime:7.0"
            }
        }
        dockerCommand {
            name = "Build container"
            commandType = build {
                source = file {
                    path = "context/console.windows.dockerfile"
                }
                contextDir = "context"
                platform = DockerCommandStep.ImagePlatform.Windows
                namesAndTags = "username/clock-console:windows"
                commandArgs = "--build-arg baseImage=mcr.microsoft.com/dotnet/runtime:7.0"
            }
        }
        dockerCommand {
            name = "Push container"
            commandType = push {
                namesAndTags = "username/clock-console:windows"
            }
        }
    }

    features {
        dockerSupport {
            loginToRegistry = on {
                dockerRegistryId = "PROJECT_EXT_5"
            }
        }
    }

    dependencies {
        dependency(Building_BuildConsoleWebWinX64) {
            snapshot {
            }

            artifacts {
                artifactRules = "bin => context"
            }
        }
    }

    requirements {
        contains("teamcity.agent.os.name", "windows-server-2022")
    }
})
```
</tab>

<tab title="Deploy Console (Linux)">

```Kotlin
object DeploymentConfigsProject_DeployConsoleLinux : BuildType({
    name = "Deploy Console (Linux)"

    enablePersonalBuilds = false
    type = BuildTypeSettings.Type.DEPLOYMENT
    maxRunningBuilds = 1

    vcs {
        root(DslContext.settingsRoot)
    }

    steps {
        dockerCommand {
            name = "Pull runtime dependencies"
            commandType = other {
                subCommand = "pull"
                commandArgs = "mcr.microsoft.com/dotnet/runtime-deps:7.0-alpine"
            }
        }
        dockerCommand {
            name = "Build container"
            commandType = build {
                source = file {
                    path = "context/console.linux.dockerfile"
                }
                contextDir = "context"
                platform = DockerCommandStep.ImagePlatform.Linux
                namesAndTags = "username/clock-console:ubuntu"
                commandArgs = "--build-arg baseImage=mcr.microsoft.com/dotnet/runtime-deps:7.0-alpine"
            }
        }
        dockerCommand {
            name = "Push container"
            commandType = push {
                namesAndTags = "username/clock-console:ubuntu"
            }
        }
    }

    features {
        dockerSupport {
            loginToRegistry = on {
                dockerRegistryId = "PROJECT_EXT_5"
            }
        }
    }

    dependencies {
        dependency(Building_BuildConsoleWebLinuxX64) {
            snapshot {
            }

            artifacts {
                artifactRules = "bin => context"
            }
        }
    }

    requirements {
        contains("teamcity.agent.os.name", "ubuntu-20.04")
    }
})
```

</tab>

<tab title="Deploy Web (Windows)">

```Kotlin
object DeploymentConfigsProject_DeployWebWindows : BuildType({
    name = "Deploy Web (Windows)"

    enablePersonalBuilds = false
    type = BuildTypeSettings.Type.DEPLOYMENT
    maxRunningBuilds = 1

    vcs {
        root(DslContext.settingsRoot)
    }

    steps {
        dockerCommand {
            name = "Pull container"
            commandType = other {
                subCommand = "pull"
                commandArgs = "mcr.microsoft.com/dotnet/runtime:7.0"
            }
        }
        dockerCommand {
            name = "Build container"
            commandType = build {
                source = file {
                    path = "context/web.windows.dockerfile"
                }
                contextDir = "context"
                platform = DockerCommandStep.ImagePlatform.Windows
                namesAndTags = "username/clock-web:windows"
                commandArgs = "--build-arg baseImage=mcr.microsoft.com/dotnet/runtime:7.0"
            }
        }
        dockerCommand {
            name = "Push container"
            commandType = push {
                namesAndTags = "username/clock-web:windows"
            }
        }
    }

    features {
        dockerSupport {
            loginToRegistry = on {
                dockerRegistryId = "PROJECT_EXT_5"
            }
        }
    }

    dependencies {
        dependency(Building_BuildConsoleWebWinX64) {
            snapshot {
            }

            artifacts {
                artifactRules = "bin => context"
            }
        }
    }

    requirements {
        contains("teamcity.agent.os.name", "windows-server-2022")
    }
})
```

</tab>

<tab title="Deploy Web (Linux)">

```Kotlin
object DeploymentConfigsProject_DeployWebLinux : BuildType({
    name = "Deploy Web (Linux)"

    enablePersonalBuilds = false
    type = BuildTypeSettings.Type.DEPLOYMENT
    maxRunningBuilds = 1

    vcs {
        root(DslContext.settingsRoot)
    }

    steps {
        dockerCommand {
            name = "Pull runtime dependencies"
            commandType = other {
                subCommand = "pull"
                commandArgs = "mcr.microsoft.com/dotnet/runtime-deps:7.0-alpine"
            }
        }
        dockerCommand {
            name = "Build container"
            commandType = build {
                source = file {
                    path = "context/web.linux.dockerfile"
                }
                contextDir = "context"
                platform = DockerCommandStep.ImagePlatform.Linux
                namesAndTags = "username/clock-web:ubuntu"
                commandArgs = "--build-arg baseImage=mcr.microsoft.com/dotnet/runtime-deps:7.0-alpine"
            }
        }
        dockerCommand {
            name = "Push container"
            commandType = push {
                namesAndTags = "username/clock-web:ubuntu"
            }
        }
    }

    features {
        dockerSupport {
            loginToRegistry = on {
                dockerRegistryId = "PROJECT_EXT_5"
            }
        }
    }

    dependencies {
        dependency(Building_BuildConsoleWebLinuxX64) {
            snapshot {
            }

            artifacts {
                artifactRules = "bin => context"
            }
        }
    }

    requirements {
        contains("teamcity.agent.os.name", "ubuntu-20.04")
    }
})
```

</tab>
</tabs>


### Set Up User Permissions

The **Administration | User Management** section allow you to set up per-project [user roles and permissions](managing-roles-and-permissions.md). Consider the following example.

* User "Alice" has the "Project Developer" role for the entire parent project. This role grants the same permissions in both _"Building Configurations"_ and _"Deployment Configurations"_ subprojects.
* User "Bob" is "Project Developer" in _"Building Configurations"_ and "Project Viewer" in "_Deployment Configurations"_.

In this setup, Alice can trigger building and deployment tasks, whereas Bob cannot start delivery. When Bob browses build results of any *"Build..."* configuration, TeamCity does not show the "Deployments" section.

TeamCity does not currently allow you to set roles and permissions on a build configuration scope. As a result, you cannot set up permissions that would allow Bob to run *"Build All"* builds but prevent them from triggering *"Deploy All"* builds. To work around this issue, move these aggregate configurations into corresponding subprojects.

<seealso>
        <category ref="concepts">
            <a href="build-chain.md">Build Chain</a>
            <a href="dependent-build.md">Dependent Build</a>
        </category>
        <category ref="admin-guide">
            <a href="snapshot-dependencies.md">Snapshot Dependencies</a>
            <a href="build-dependencies-setup.md">Build Dependencies Setup</a>
        </category>
</seealso>