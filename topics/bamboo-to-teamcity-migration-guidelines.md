[//]: # (title: Bamboo to TeamCity Migration Guidelines)
[//]: # (help-id: Bamboo to TeamCity Migration Guidelines)


You can migrate from Atlassian Bamboo to TeamCity by converting your Bamboo plan configurations (exported from the Bamboo UI as Bamboo Specs in Java or YAML, or stored in Spec repositories) into TeamCity's Kotlin DSL build configuration format.

[Kotlin DSL](kotlin-dsl.md) (`.teamcity/settings.kts`) is the recommended approach: it is version-controlled, reviewable, and composable. Build configurations can also be created and managed directly through the TeamCity UI without writing any code.

## Key migration considerations

| Configuration aspect  | Bamboo                                                                 | TeamCity                                                                               | Migration tasks                                                                                                                                                                                                                                |
|-----------------------|------------------------------------------------------------------------|----------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Configuration files   | Bamboo Specs (Java or YAML)                                            | Kotlin DSL (`settings.kts`)                                                            | Convert Specs to Kotlin DSL using the [TeamCity CLI](teamcity-cli.md), or recreate build configurations and Pipelines via the TeamCity UI. The sub-tasks below are all part of this conversion: each maps to specific lines in your .kts file. |
| Variable syntax       | `${bamboo.variableName}` in config; dots become underscores in scripts | `%\param.name%` in config fields; env.-prefixed params available as `$NAME` in scripts | Update all variable references in build configurations and scripts                                                                                                                                                                             |
| Artifact sharing      | Named artifacts with subscriptions                                     | Artifact dependencies between build configurations                                     | Reconfigure artifact paths and dependency rules                                                                                                                                                                                                |
| Execution environment | Agents (local or remote)                                               | Build Agents (cloud or self-hosted)                                                    | Install and configure TeamCity agents                                                                                                                                                                                                          |
| Deployments           | Separate deployment projects                                           | Deployment build configurations with environments                                      | Use separate Deployment build configurations linked via build chains                                                                                                                                                                           |
| Parallelism           | Limited by Bamboo agent count and license tier                         | Limited by agent count and concurrent builds license                                   | Ensure sufficient agents and concurrent-build license capacity                                                                                                                                                                                 |


## Configuration examples

### Bamboo Specs vs. TeamCity Kotlin DSL

The following examples show a Bamboo Specs YAML plan and its TeamCity Kotlin DSL equivalent.

**Bamboo**

Bamboo organizes builds through a nested hierarchy: projects contain plans, plans define stages and jobs, and jobs execute individual tasks. Projects act as containers for shared variables, credentials, and repository connections used across multiple plans.

When reviewing your Bamboo Specs export, focus on the migration-critical elements:

* **Jobs and tasks:** The actual build commands and scripts
* **Stage definitions:** Sequential execution order and dependencies between jobs
* **Variables and artifacts:** Data and files shared between jobs or plans
* **Triggers and conditions:** Rules that determine when builds run

```yaml
version: 2
plan:
  project-key: AB
  key: TP
  name: test plan
stages:
  - Default Stage:
      manual: false
      final: false
      jobs:
        - Default Job
Default Job:
  key: JOB1
  tasks:
    - checkout:
        force-clean-build: false
        description: Checkout Default Repository
    - script:
        interpreter: SHELL
        scripts:
          - |-
            ruby -v
            bundle config set --local deployment true
            bundle install -j $(nproc)
            rubocop
            rspec spec
        description: run bundler
  artifact-subscriptions: []
repositories:
  - Demo Project:
      scope: global
triggers:
  - polling:
      period: '180'
branches:
  create: manually
  delete: never
  link-to-jira: true
```

**TeamCity (Kotlin DSL)**

TeamCity replaces the nested Bamboo project/plan/stage/job hierarchy with [Projects](creating-and-editing-projects.md) containing [Build Configurations](creating-and-editing-build-configurations.md). Each Build Configuration defines its own build steps, triggers, agent requirements, and artifact rules. By default, the Kotlin DSL lives in a .teamcity/ directory at the root of your repository.

```Kotlin
// .teamcity/settings.kts
import jetbrains.buildServer.configs.kotlin.*
import jetbrains.buildServer.configs.kotlin.buildSteps.script
import jetbrains.buildServer.configs.kotlin.triggers.vcs

project {
    buildType(TestPlan)
}

object TestPlan : BuildType({
    name = "Test Plan"

    vcs {
        root(DslContext.settingsRoot)
        cleanCheckout = false
    }

    steps {
        script {
            name = "Run Bundler"
            scriptContent = """
                ruby -v
                bundle config set --local deployment true
                bundle install -j ${'$'}(nproc)
                rubocop
                rspec spec
            """.trimIndent()
        }
    }

    triggers {
        vcs {
            // TeamCity detects VCS changes automatically — no polling interval needed
        }
    }
})
```

> TeamCity automatically checks out your source code before running build steps, so there is no need for an explicit checkout step equivalent to Bamboo's `checkout` task.
> 
{style="note"}



### Jobs and tasks to build steps


In Bamboo, a **Job** is composed of **Tasks**, either scripts or predefined tasks from the Atlassian Marketplace. The number of concurrent jobs depends on the number of available Bamboo agents.

In TeamCity, the equivalent of tasks are [build steps](configuring-build-steps.md) within a build configuration. Steps run sequentially by default. The number of concurrent builds depends on the number of connected Build Agents and the concurrency settings of your TeamCity license.


**Bamboo**

```yaml
Default Job:
  key: JOB1
  tasks:
    - checkout:
        force-clean-build: false
        description: Checkout Default Repository
    - script:
        interpreter: SHELL
        scripts:
          - |-
            ruby -v
            bundle config set --local deployment true
            bundle install -j $(nproc)
        description: run bundler

```

**TeamCity (Kotlin DSL)**

```Kotlin
steps {
    script {
        name = "Run Bundler"
        scriptContent = """
            ruby -v
            bundle config set --local deployment true
            bundle install -j ${'$'}(nproc)
        """.trimIndent()
    }
    script {
        name = "Run Tests"
        scriptContent = "bundle exec rspec spec"
    }
}
```


TeamCity has a large library of built-in build steps ([Maven](maven.md), [Gradle](gradle.md), [.NET](net.md), [Docker](docker.md), etc.) that mirror the predefined tasks available in the Bamboo Marketplace, so you often do not need to write raw shell scripts for common build tools.

In addition to that, TeamCity has [its own marketplace](https://plugins.jetbrains.com/teamcity) of plugins and recipes.


### Container images (Docker)

**Bamboo**

Builds run on the Bamboo agent's native OS by default, but can be configured to run inside Docker containers using the `docker` keyword at the plan or job level.

```yaml
docker: alpine:latest

Build Application:
  tasks:
    - script:
        - # Run builds
  docker:
    image: alpine:edge
```

**TeamCity**

In TeamCity, you have two options for running build steps inside containers:

* [Container Wrapper](container-wrapper.md) (formerly Docker Wrapper): configured per build step using the dockerImage property in Kotlin DSL, or via the Docker Settings section in the step UI. This runs that individual step inside the specified container.


* [Run in Docker](run-in-docker.md) build feature: configured at the Build Configuration level, it applies a single container image to all supported steps at once. This is the closest equivalent to Bamboo's plan-level docker: setting.

The agent must have Docker (or Podman, supported since TeamCity 2023.05) installed.


```Kotlin
// Option 1: Container Wrapper (per step)
steps {
    script {
        name = "Build Application"
        scriptContent = "# Run builds"
        dockerImage = "alpine:edge"
        dockerImagePlatform = ScriptBuildStep.ImagePlatform.Linux
    }
}

// Option 2: Run in Docker build feature (all steps in this Build Configuration)
features {
    runInDocker {
        imageName = "alpine:edge"
    }
}
```

### Variables

**Bamboo**

Bamboo has multiple variable namespaces. System variables use `${system.variableName}` and plan variables use `${bamboo.variableName}`. Inside script tasks, dots are converted to underscores, so `${bamboo.variableName}` becomes `$bamboo_variableName`.

```yaml
variables:
  username: admin
  releaseType: milestone

Default Job:
  tasks:
    - script:
        scripts:
          - echo "$bamboo_username is the DRI for $bamboo_releaseType"
```

**TeamCity**

In TeamCity, parameters use the `%\param.name%` syntax in all configuration fields (including script content, which TeamCity resolves before execution). To expose a parameter as a native environment variable inside the build process itself, prefix it with `env.` when defining it, then reference it as `$NAME` on Linux/macOS or `%\NAME%` on Windows.

Configuration parameters without the `env.` prefix are not automatically available as shell environment variables.

TeamCity also provides a rich set of [predefined build parameters](predefined-build-parameters.md).

```Kotlin
params {
    // Available as %username% in config fields; not a shell env var
    param("username", "admin")
    // Available as $RELEASE_TYPE in shell scripts (Linux/macOS)
    param("env.RELEASE_TYPE", "milestone")
}

steps {
    script {
        scriptContent = """
            echo "%username% is the DRI for ${'$'}RELEASE_TYPE"
        """.trimIndent()
    }
}
```

**Common variable equivalents**

| Bamboo variable                        | TeamCity equivalent                    |
|----------------------------------------|----------------------------------------|
| `${bamboo.planKey}`                    | `%\system.teamcity.buildType.id%`      |
| `${bamboo.buildNumber}`                | `%\build.number%`                      |
| `${bamboo.planRepository.branch}`      | `%\teamcity.build.branch%`             |
| `${bamboo.build.working.directory}`    | `%\system.teamcity.build.checkoutDir%` |
| `${bamboo.repository.revision.number}` | `%\build.vcs.number%`                  |


### Conditions and triggers

**Bamboo**

Bamboo triggers builds based on VCS polling, schedules, the outcomes of other plans, or on demand. Branch conditions can be applied to individual tasks.

```yaml
tasks:
  - script:
      scripts:
        - echo "Hello"
      conditions:
        - variable:
            equals:
              planRepository.branch: development

triggers:
  - polling:
      period: '180'
```

**TeamCity**

TeamCity workflows trigger on VCS changes (push-based, no polling interval required), schedules, or finish of other build configurations. Per-step branch conditions are handled through [Build Step execution conditions](build-step-execution-conditions.md) or [Snapshot Dependencies](configuring-dependencies.md).


```Kotlin
triggers {
    vcs {
        branchFilter = "+:development"
    }
    schedule {
        schedulingPolicy = cron {
            hours = "2"
            dayOfWeek = "MON-FRI"
        }
        branchFilter = "+:<default>"
    }
}

steps {
    script {
        name = "Hello"
        scriptContent = "echo Hello"
        // Step-level conditions can be set via the UI or executionMode
        executionMode = BuildStep.ExecutionMode.RUN_ON_SUCCESS
    }
}
```


For branch-specific logic equivalent to Bamboo task conditions, use Logical Condition build step execution conditions (available in TeamCity 2020.2+):

* Navigate to the Execution conditions of a build step;
* Add a condition: `teamcity.build.branch equals development`.

### Artifacts

**Bamboo**

In Bamboo, artifacts are defined with a name, location, and pattern. Jobs subscribe to artifacts from other jobs in the same plan, and `artifact-download` tasks fetch artifacts from other plans.

```yaml
Build:
  artifacts:
    - name: Test Reports
      location: target/reports
      pattern: '*.xml'
      required: false
      shared: false
    - name: Special Reports
      location: target/reports
      pattern: 'special/*.xml'
      shared: true

Test App:
  artifact-subscriptions:
    - artifact: Test Reports
      destination: deploy
```

**TeamCity**

In TeamCity, artifacts are published by specifying **Artifact paths** in the Build Configuration. Other Build Configurations can consume them via [Artifact Dependencies](artifact-dependencies.md), which can be configured to download specific paths before a dependent build starts.

```Kotlin
// Publishing build configuration
object Build : BuildType({
    name = "Build"

    artifactRules = """
        target/reports/*.xml => test-reports
        target/reports/special/*.xml => special-reports
    """.trimIndent()

    // ... steps
})

// Consuming build configuration
object TestApp : BuildType({
    name = "Test App"

    dependencies {
        artifacts(Build) {
            buildRule = lastSuccessful()
            artifactRules = "test-reports/*.xml => deploy/"
        }
    }

    // ... steps
})
```

Within a Build Chain, [Snapshot Dependencies](configuring-dependencies.md) ensure builds run in the correct order and use the same VCS revision. However, passing actual files between builds still requires explicit [Artifact Dependencies](artifact-dependencies.md). Snapshot dependencies alone do not transfer artifacts.


### Caching

Bamboo uses Git caches configured in the administration settings, stored on the Bamboo server or remote agents. TeamCity supports both Git mirror caches (managed automatically per agent) and per-build caches using the [Build Cache](build-cache.md) build feature (introduced in TeamCity 2023.11). Verify the exact Kotlin DSL imports against your TeamCity version before using the `cache { }` block, as the API may vary.

```Kotlin
features {
    // Cache node_modules across builds on the same agent
    cache {
        name = "node-modules-cache"
        rules = "node_modules"
        publishCondition = publishAlways()
    }
}

steps {
    script {
        name = "Install Dependencies"
        scriptContent = """
            bundle config set --local path vendor/ruby
            bundle install
            yarn install --frozen-lockfile
        """.trimIndent()
    }
}
```

For agent-level caching (equivalent to Bamboo's Git cache), TeamCity agents automatically maintain a local VCS mirror, so repeated checkouts do not re-clone the full repository.

### Deployments

**Bamboo**

Bamboo uses separate **Deployment Projects** that link to build plans to track, fetch, and deploy build artifacts to named environments.

```yaml
deployment:
  name: Deploy Ruby App
  source-plan: build-app

release-naming: release-1.0

environments:
  - Production

Production:
  tasks:
    - # scripts to deploy app to production
    - ./.ci/deploy_prod.sh
```

**TeamCity**

In TeamCity, deployments are modeled as Build Configurations of the [Deployment](deployment-build-configuration.md) type. They consume artifacts from upstream build configurations via snapshot or artifact dependencies.


```Kotlin
object DeployToProduction : BuildType({
    name = "Deploy to Production"
    type = BuildTypeSettings.Type.DEPLOYMENT

    vcs {
        root(DslContext.settingsRoot)
    }

    dependencies {
        snapshot(Build) {
            onDependencyFailure = FailureAction.FAIL_TO_START
        }
        artifacts(Build) {
            buildRule = sameChainOrLastFinished()
            artifactRules = "**/* => ."
        }
    }

    steps {
        script {
            name = "Deploy"
            scriptContent = "./.ci/deploy_prod.sh"
        }
    }

    // Require manual confirmation before deploying
    requirements {
        // agent requirements here
    }
})
```

For gated deployments (equivalent to Bamboo's manual-trigger environments), set the Build Configuration trigger to **Manual** and restrict permissions to specific roles using TeamCity's [role-based access control](managing-roles-and-permissions.md). You can also use the TeamCity [Build Approval](build-approval.md) feature. 


### Security scanning

Bamboo relies on third-party tasks from the Atlassian Marketplace for security scanning.

TeamCity integrates with leading security tools through dedicated build runners and plugins. Common options include:

* **JetBrains Qodana:** Static analysis and code quality scanning, available as a dedicated Qodana build runner in TeamCity.

* **Trivy / Snyk / SonarQube:** Run as a script step or via community plugins available in the JetBrains Marketplace.

* **OWASP Dependency-Check:** Run as a Maven/Gradle step or a standalone script step.

```Kotlin
steps {
    script {
        name = "Dependency Security Scan"
        scriptContent = """
            trivy fs --exit-code 1 --severity HIGH,CRITICAL .
        """.trimIndent()
        dockerImage = "aquasec/trivy:latest"
    }
}
```


### Secrets management

Bamboo manages secrets through Shared Credentials (SSH keys, passwords, API tokens) or third-party Atlassian Marketplace applications.

TeamCity provides several options for secrets management:

* **Typed parameters:** Mark any configuration parameter as Password type. TeamCity masks the value in build logs and the UI, and never exposes it in plain text.

* [**HashiCorp Vault integration**](hashicorp-vault.md): Store secrets in Vault and reference them in TeamCity using the built-in Vault connection. Secrets are injected as build parameters at runtime and masked in logs.

* **AWS Secrets Manager / Azure Key Vault**: Available via the JetBrains Marketplace plugins.

```Kotlin
params {
    password("env.DEPLOY_API_TOKEN", "credentialsJSON:vault-token-ref")
    param("deploy.target", "production")
}
```

> Never store secrets as plain-text parameters visible in the project configuration. Always use Password-type parameters or an external secrets backend. Avoid committing credentials to your Kotlin DSL, as `.teamcity/` directory is stored in VCS.
> 
{style="warning"}


## Create a migration plan

Before starting your migration, answer these questions:

* What Bamboo Tasks are used by jobs today and what do they do?
* Do any tasks wrap common build tools like Maven, Gradle, NPM, or Docker?
* What software is installed on Bamboo agents that must also be on TeamCity agents?
* How are you authenticating from Bamboo: SSH keys, API tokens, or other credentials?
* Are there shared credentials in Bamboo accessing external services?
* Are there shared plan templates or reusable Bamboo Specs in use?
* How many concurrent builds do you need to support, and how many agents are required?

## Migrate from Bamboo to TeamCity

### Prerequisites

* A TeamCity server (Cloud or On-Premises) set up and accessible.
* At least one TeamCity Build Agent connected and authorized.
* Access to your Bamboo project YAML Specs (exported from the Bamboo UI or from Spec repositories).
* Your source code repositories accessible from the TeamCity server.

### Migration steps

1. **Audit your Bamboo configuration**
   * Export your Bamboo projects and plans as YAML Specs from the Bamboo UI.
   * List all Bamboo tasks used in each job (Maven, Docker, SCP, custom scripts, etc.).
   * Document software versions installed on each Bamboo agent.
   * Identify all shared credentials and their usage across plans.

2. **Set up your TeamCity project and VCS roots**

   * Create a new [Project](creating-and-editing-projects.md) in TeamCity matching your Bamboo project.
   * Configure [VCS Roots](configuring-vcs-roots.md) pointing to your source code repositories (Git, GitHub, GitLab, Bitbucket, etc.).
   * TeamCity supports OAuth connections to major Git hosting providers for easy setup.

3. **Set up TeamCity Build Agents with equivalent software**

   * Install the same software versions that exist on your Bamboo agents.
   * For complex agent setups, create custom Docker images with your required tools and reference them in build steps using `dockerImage`.
   * Test that agents can execute your build commands successfully before migrating pipelines.
   
4. Convert Bamboo Specs to TeamCity Build Configurations

   * Enable [Versioned Settings](storing-project-settings-in-version-control.md) in your TeamCity project to use Kotlin DSL. By default, settings are stored in a `.teamcity/` directory in your repository root, but TeamCity also supports storing them in a separate dedicated VCS repository.

   * Use the TeamCity CLI to automate and accelerate the conversion. Install it with `brew install jetbrains/utils/teamcity` (macOS), `winget install JetBrains.TeamCityCLI` (Windows), or `curl -fsSL https://jb.gg/tc/install | bash` (Linux/macOS). The CLI supports scripted workflows for exporting, validating, and pushing pipeline configuration. A dedicated `teamcity migrate` command with Bamboo-specific support is currently in development and will automate the bulk of the Specs-to-DSL conversion when released.

   * Use the CLI's AI agent skill to get AI-assisted help with the conversion. Run `teamcity skill install` to register the TeamCity skill with your AI coding agent (Claude, Cursor, Copilot, or any MCP-compatible tool). The skill gives the agent direct knowledge of TeamCity's Kotlin DSL, build configuration structure, and common Bamboo-to-TeamCity mapping patterns, so you can prompt it to convert individual Bamboo Specs files and have it apply the output directly to your repository.

   * Replace the Bamboo plan/stage/job hierarchy with TeamCity Build Configurations and Build Chains (linked via Snapshot Dependencies).

   * Convert `${bamboo.variableName}` syntax to `%\variable.name%` in configuration fields and `$VARIABLE_NAME` in shell scripts.

   * Replace Bamboo-specific variables (e.g., `${bamboo.planKey}`) with TeamCity predefined parameters (e.g., `%\system.teamcity.buildType.id%`).

   * Remove explicit checkout tasks. TeamCity automatically checks out source code before each build step.

5. **Migrate artifact handling**

   * Replace Bamboo `artifact-subscriptions` with TeamCity [Artifact Dependencies](artifact-dependencies.md) between Build Configurations. 
   * Define artifact publication paths using `artifactRules` in each Build Configuration.
   * Within a Build Chain, artifact passing is handled automatically via Snapshot Dependencies.

6. **Convert Bamboo deployment projects**

   * Move Bamboo deployment tasks into TeamCity Deployment Build Configurations.
   * Replace Bamboo environment definitions with TeamCity Deployment Environments (TeamCity 2024.03+).
   * For Kubernetes deployments, use the Kubernetes support plugin or script steps with `kubectl` in a Docker container.
   * Configure manual approval gates using per-build-configuration trigger settings and role-based permissions.

7. **Migrate secrets and credentials**

   * Re-create Bamboo Shared Credentials as Password-type parameters or Connection entries in TeamCity.
   * For sensitive tokens, integrate with HashiCorp Vault or another supported secrets backend.
   * Audit all Kotlin DSL files before committing to VCS to ensure no secrets are hardcoded.

8. **Test and optimize your migrated workflows**

* Run test builds to verify functionality against your existing Bamboo results.
* Enable [Pull Request / Merge Request build triggers](configuring-vcs-triggers.md) to validate changes before merge.
* Use TeamCity's Build Chain visualization to confirm dependency ordering.
* Optimize performance using the Dependency Cache build feature, parallelism via [Parallel Tests](parallel-tests.md), and composite builds for large projects.

### Cutover strategy

Avoid a hard cutover on day one. A gradual parallel-run approach reduces risk and gives your team time to build confidence in TeamCity before decommissioning Bamboo.

Recommended rollout:

1. **Start with a low-risk project.** Pick a non-critical service or internal tool as your first migration target. Use it to validate your agent setup, Kotlin DSL patterns, and secrets configuration before touching production pipelines.

2. **Run both systems in parallel.** For each migrated plan, keep the Bamboo build active and trigger the equivalent TeamCity build on every commit. Compare results (artifact outputs, test reports, deployment outcomes) until parity is confirmed.

3. **Migrate in waves.** Group plans by team or service domain and migrate one wave at a time, typically over one to two sprint cycles per wave. This limits blast radius if something goes wrong.

4. **Redirect traffic and monitor.** Once a TeamCity pipeline has passed parallel validation for at least one full sprint, disable the Bamboo plan and make TeamCity the authoritative build. Monitor build success rates and durations closely for the first two weeks.

5. **Decommission Bamboo.** After all plans have been migrated and validated, disable remaining Bamboo agents and archive the project configuration before shutting down the Bamboo server.

> Set up a dedicated Slack (or Teams) channel for migration status updates and flag any build discrepancies between the two systems immediately.
>
{style="tip"}

### Verifying artifact parity

During the parallel-run period, it is not enough to confirm that TeamCity builds succeed. You also need to confirm that TeamCity produces the same artifacts as Bamboo did for the same source revision. Environment differences between Bamboo and TeamCity agents (compiler versions, dependency resolution, OS libraries) can cause silent divergence that only surfaces after you decommission Bamboo.

The recommended approach is a dedicated Artifact Parity Verification build configuration that runs automatically on every commit during the transition period. It downloads the artifact produced by the new TeamCity build and the artifact produced by the equivalent Bamboo build on the same revision, then diffs them and fails the build if they diverge.

**Example: Artifact parity verification build configuration**

```Kotlin
object ArtifactParityCheck : BuildType({
    name = "Artifact Parity Check (Bamboo vs TeamCity)"

    // Trigger on the same VCS root as your main build
    vcs {
        root(DslContext.settingsRoot)
    }

    // Pull in the artifact freshly built by TeamCity on this revision
    dependencies {
        artifacts(TeamCityBuild) {
            buildRule = sameChainOrLastFinished()
            artifactRules = "build/app.jar => tc-artifact/"
        }
    }

    steps {
        script {
            name = "Download Bamboo artifact for the same revision"
            scriptContent = """
                REVISION="%build.vcs.number%"

                # Use the Bamboo REST API to find the build for this revision
                # and download its artifact. Replace PLAN_KEY and ARTIFACT_NAME.
                BAMBOO_BUILD=${'$'}(curl -s -u "%bamboo.user%:%bamboo.token%" \
                  "%bamboo.url%/rest/api/latest/result/PLAN-KEY.json?expand=results.result" \
                  | jq -r ".results.result[]
                    | select(.vcsRevisionKey == \"${'$'}REVISION\")
                    | .buildResultKey" | head -1)

                echo "Matching Bamboo build: ${'$'}BAMBOO_BUILD"

                curl -s -u "%bamboo.user%:%bamboo.token%" \
                  "%bamboo.url%/rest/api/latest/result/${'$'}BAMBOO_BUILD/artifact/\
shared/ARTIFACT_NAME/app.jar" \
                  -o bamboo-artifact/app.jar
            """.trimIndent()
        }

        script {
            name = "Compare artifacts"
            scriptContent = """
                echo "TeamCity artifact checksum:"
                sha256sum tc-artifact/app.jar

                echo "Bamboo artifact checksum:"
                sha256sum bamboo-artifact/app.jar

                if ! diff <(sha256sum tc-artifact/app.jar | awk '{print $1}') \
                          <(sha256sum bamboo-artifact/app.jar | awk '{print $1}'); then
                  echo "MISMATCH: TeamCity and Bamboo artifacts differ on revision %build.vcs.number%"
                  exit 1
                fi

                echo "Artifacts match."
            """.trimIndent()
        }
    }

    // Store both artifacts for manual inspection if the check fails
    artifactRules = """
        tc-artifact/app.jar => parity-check/tc/
        bamboo-artifact/app.jar => parity-check/bamboo/
    """.trimIndent()
})
```

A few practical notes:

* Checksum comparison is the simplest signal, but not always the right one. Some build tools embed timestamps or random salts that make byte-identical output impossible even with the same inputs. In those cases, compare functional equivalence instead: unpack the artifact and diff its contents, run the same test suite against both, or compare exported metadata (dependency manifests, class lists, binary sizes within a tolerance).

* The Bamboo REST API (`/rest/api/latest/result`) lets you query builds by VCS revision. You need a Bamboo API token stored as a Password-type parameter (`%\bamboo.token%`) so it is masked in logs.

* Run this check in parallel with your main TeamCity build, not sequentially, to avoid adding to the critical path during the transition period.

* Retire this build configuration once you have decommissioned Bamboo. It has no purpose after the cutover is complete.

### What to expect after migration

Teams migrating from Bamboo to TeamCity typically see improvements across several dimensions, though exact results depend on your current Bamboo setup, agent hardware, and pipeline complexity:

| Metric                            | Typical improvement                                                                                                             |
|-----------------------------------|---------------------------------------------------------------------------------------------------------------------------------|
| Build queue wait time             | Significantly reduced: TeamCity's agent pool and cloud agent auto-scaling eliminate the fixed-agent bottleneck common in Bamboo |
| Concurrent build capacity         | Scales on demand with cloud agents, vs. Bamboo's fixed agent count                                                              |
| Pipeline configuration overhead   | Lower: Kotlin DSL is version-controlled, reviewable, and composable; no manual UI re-entry when cloning plans                   |
| Secrets and credential management | More structured: Password parameters and Vault integration replace Bamboo's scattered Shared Credentials                        |
| Visibility and debugging          | Improved: TeamCity's Build Chain view, per-step logs, and test history make failures faster to diagnose                         |

Establish your baseline Bamboo metrics (average build time, queue wait, weekly failure rate) before starting migration so you can produce a concrete before/after comparison for stakeholders once the cutover is complete.

## Reference: Bamboo concepts to TeamCity equivalents

| Bamboo concept           | TeamCity equivalent                                   |
|--------------------------|-------------------------------------------------------|
| Project                  | Project                                               |
| Plan                     | Build Configuration                                   |
| Stage                    | Build Chain step (via Snapshot Dependency)            |
| Job                      | Build Configuration (or Parallel step)                |
| Task                     | Build Step                                            |
| Agent                    | Build Agent                                           |
| Bamboo Specs (YAML/Java) | Kotlin DSL / Versioned Settings                       |
| Shared Credentials       | Connections / Password Parameters                     |
| Artifact Subscription    | Artifact Dependency                                   |
| Deployment Project       | Deployment Build Configuration                        |
| Deployment Environment   | Deployment Build Configuration (environment grouping) |
| Plan Branch              | Branch Build Configuration                            |
| Plan Trigger             | Build Trigger (VCS, Schedule, Finish Build)           |
| Global Variables         | Project-level Parameters                              | 
| Plan Variables           | Build Configuration Parameters                        |
