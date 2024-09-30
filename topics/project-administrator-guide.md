# Project Administrator Guide

This section focuses on **Project administration**. To learn more about other aspects of TeamCity, refer to the [](server-administrator-guide.md) and [](user-guide.md) sections.


## Steps, Configurations and Projects

In TeamCity, a building routine consists of the following blocks:

<img src="dk-basic-tree-diagram3.png" alt="TeamCity elements" width="706"/>

* **Build step** — an essential building block that executes a predefined set of commands. This can be a single command (like `dotnet test` or `gradle clean build`) or a series of operations (such as a custom Python or Bash script). Build steps run fully, with no partial execution.

* **Build configuration** — a sequence of build steps executed in a specific order. With a configuration, you can:

    * arrange steps in any order you need;
    * temporarily disable individual steps.
    * set [conditions](build-step-execution-conditions.md) for when steps should be executed. If a condition is not met, the corresponding step is skipped. For instance, step #3 could be set to run only if step #2 fails, and step #4 might be configured to execute only on Windows agents.

    Configurations can also be turned into [templates](build-configuration-template.md) making it easy to clone and reuse them without manually configuring each new setup. Once cloned, each copy can be customized independently.

    You can also incorporate configurations from the same or different projects into one [unified workflow](build-chain.md).


* **Project** — a collection of independent build configurations. These configurations can be run separately but are grouped under a single project to share common resources: [connections](configuring-connections.md), [parameters](configuring-build-parameters.md), [artifact storages](configuring-artifacts-storage.md), [cloud agent profiles](agent-cloud-profile.md), and so on.

    Each project can be owned by another project for the same benefits: subprojects can access entities owned by their parent projects. The topmost project, called the **Root project**, is created automatically by TeamCity. This Root project cannot be removed and is ideal for setting up globally accessible parameters, connections, cloud profiles, and other shared resources.


## Basic CI Workflow in TeamCity

The following diagram illustrates the basic TeamCity workflow:

<img src="cicd-flow.png" width="711" alt="Basic CI flow with TeamCity"/>

1. The TeamCity server detects a change in your repository. This happens in one of the following ways:
    * Default setup: every 60 seconds TeamCity automatically [polls](teamcity-configuration-and-maintenance.md#Version+Control+Settings) all VCS repositories targeted by its projects.
    * Webhooks: if [a post-commit hook](configuring-vcs-post-commit-hooks-for-teamcity.md) is configured, any commit sends this webhook to TeamCity. This approach has two major benefits: decreased server load since the server no longer periodically polls the repo, and shortened feedback loop (the server gets notified about changes immediately after a change was introduced).
2. The server writes this change to the database.
3. A [trigger](configuring-build-triggers.md) attached to the build configuration detects the relevant change in the database and initiates a build.
4. The triggered build appears in the [build queue](working-with-build-queue.md).
5. The build is assigned to a free compatible build agent.
6. The agent executes the build steps, described in the build configuration. While executing the steps, the agent reports the build progress to the TeamCity server. It sends all the log messages, test reports, code coverage results on the fly, so you can monitor the build process in real time.
7. After finishing the build, the agent sends [build artifacts](build-artifact.md) to the server.


## VCS Roots

A **VCS root** is a cornerstone of the TeamCity &larr;&rarr; VCS repository communication. This integral element implements repository access required to perform a wide range of operations: repository checkout, code sources tagging, communicating build statuses back to VCS, and so on.

VCS roots store the following information:

* Fetch and push URLs that TeamCity uses to pull and push remote files.
* Branch information: the list of repository branches TeamCity should track and which branch is the default (main) one.
* Authentication settings: credentials TeamCity uses to access a repo.
* Checkout settings: specify how remote files should be stored and whether submodules should be checked out along with the main repository.
* Custom changes polling settings that allow you to override the default 60-second interval.

Sections related to VCS roots are available in both project and configuration settings.

<img src="dk-roots-in-projects-and-configs-v.png" alt="Root settings in projects and configs" width="706"/>

However, configurations never own roots. You can "attach" a VCS root to a configuration, but roots are always stored in (owned by) projects. This technique results in the following:

* Multiple build configurations can access the same repository with the same auth and checkout settings by sharing the same root.
* Changing VCS root settings affects all configurations that use it.
* When editing VCS root settings, you have an option to duplicate this root and store updated settings in this new clone, keeping the original root unchanged. This allows you to customize one build configuration but leave other configurations that share this root unaffected.

Although a VCS root is an existential part of any build configuration that works with a remote repository, in many scenarios TeamCity generates roots automatically and does not require that you create them by hand for each new build configuration. See the [tutorial](#Tutorial%3A+Create+Your+First+Project+in+TeamCity) at the end of this document for an example.



## Build Chains

???

Snapshot vs artifact dependencies

Related article: [](configuring-dependencies.md)

## Tutorial: Create Your First Project in TeamCity

The majority of projects built in your TeamCity will be solutions actively developed by your teams, and as such, hosted on your personal or organization VCS accounts. Refer to [Option 1](#Option+1%3A+From+Your+Repository) to learn how to set up a project that targets a repository owned by or shared with you.

In some cases you may also want to set up a project that targets a third-party repository. If this repository is public, you can set up a project using a direct link. See [Option 2](#Option+2%3A+From+a+Third+Party+Public+Repository) for the details.

### Option 1: From Your Repository

- Create a connection first! No need to create roots manually

### Option 2: From a Third Party Public Repository


### Run Your First Build