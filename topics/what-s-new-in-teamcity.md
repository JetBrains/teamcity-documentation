[//]: # (title: What's New in TeamCity 2023.11)
[//]: # (auxiliary-id: What's New in TeamCity 2023.11;What's New in TeamCity)

<!-- #REGION TC -->


## VCS Integrations
{product="tc"}


### Bitbucket Cloud
{product="tc"}

[Pull Requests](pull-requests.md#Bitbucket+Cloud+Pull+Requests) features that track Bitbucket Cloud repositories have two new Authentication Type options:

* Refreshable access tokens issued via corresponding [OAuth connections](configuring-connections.md#Bitbucket+Cloud).

* Permanent access tokens issued for a specific repository, project, or workspace.

Learn more: [](pull-requests.md#Bitbucket+Cloud+Pull+Requests).

### Bitbucket Server and Data Center
{product="tc"}

The [Pull Requests](pull-requests.md#Bitbucket+Server+Pull+Requests) feature can now utilize refreshable OAuth tokens to access repositories on Bitbucket Server / Data Center.

Learn more: [](pull-requests.md#Bitbucket+Server+Pull+Requests).

### GitHub
{product="tc"}

#### Seamless GitHub App Registration
{product="tc"}

In version 2023.05, we introduced the [new type](configuring-connections.md#GitHub) of connections to GitHub and GitHub Enterprise. These connections utilize [GitHub Apps](https://docs.github.com/en/apps/using-github-apps/about-using-github-apps), instead of the traditional OAuth-based access to repositories.

Starting with version 2023.11, you will be able to create these connections much faster, without manually setting up and registering new apps in GitHub. Choose the **Automatic** creation mode in the **Add connection** dialog and TeamCity will do the rest.

<img src="dk-GhAppManifestButton.png" width="706" alt="GitHub Manifest App Button"/>

Learn more: [Configuring Connections](configuring-connections.md#GitHub).

#### Refreshable Tokens
{id="refreshable-tokens-github"}

You can now issue refreshable access tokens for [GitHub App connections](configuring-connections.md#GitHub).


### GitLab
{prdocut="tc"}

[Commit Status Publishers](commit-status-publisher.md#GitLab) and [Pull Requests](pull-requests.md#GitLab+Merge+Requests) features that target GitLab repositories can now utilize refreshable application tokens to pass the authentication.

<img src="dk-csp-GitLabToken.png" width="708" alt="Acquire access token for GitLab"/>

### JetBrains Space
{product="tc"}

#### Automated Connections
{product="tc"}

With this release we introduce the updated hassle-free way to set up integrations between TeamCity and JetBrains Space projects. Instead of manually creating, setting up, and installing Space applications that grant TeamCity all required permissions, you can now delegate this routine to TeamCity. All you need to do is to point TeamCity to the right Space organization, and it will do the rest for you.

<img src="dk-space-newProjectConnection.png" alt="New Space Project Connection" width="706"/>

The updated integration utilizes two types of connections:

* Organization connection — creates a basic Space application that allows TeamCity to retrieve the list of projects in your organization and create new Space applications.
* Project connections — use the parent organization connection to create applications that allow TeamCity to access individual Space projects.

Learn more: [](configuring-connections.md#jetbrains-space-connection).


#### Automatic Status Publishing
{product="tc"}

Starting with this version, TeamCity build configurations that target [JetBrains Space](https://www.jetbrains.com/space/) repositories do not require a configured **Commit Status Publisher** build feature to post build-related updates. Set up a TeamCity project via a predefined [Space connection](configuring-connections.md#jetbrains-space-connection) and build statuses will be posted automatically.

<img src="dk-csp-space.png" width="706" alt="Publish Space build statuses"/>

The option to manually set up the Commit Status Publisher feature is still available for those who wish full control over the process, as well as for custom setups where TeamCity is unable to publish build statuses automatically.

Learn more: [](commit-status-publisher.md#JetBrains+Space).

#### Refreshable Tokens
{id="refreshable-tokens-space"}

You can now issue refreshable access tokens for [JetBrains Space connections](configuring-connections.md#jetbrains-space-connection).

### Perforce Helix Swarm
{product="tc"}

In version 2023.11, we have overhauled the "Perforce Helix Swarm" publisher of the [](commit-status-publisher.md) build feature. TeamCity can now utilize workflows and tests that already exist in your Swarm setup (instead of creating its own tests). In addition, the Publisher no longer requires credentials of a user with administrator access.

<img src="dk-swarm-personalbuild.png" width="706" alt="Personal build in TeamCity"/>

Learn more: [](integrating-with-helix-swarm.md).





## Amazon Web Services Integrations
{product="tc"}

### EC2 Plugin Update
{product="tc"}

We have overhauled the Amazon EC2 integration plugin. Apart from a refreshed look, the updated plugin features the following enhancements:

* Support for Mac AMIs. Mac VMs can be run only on dedicated Mac Mini hosts that should be booked for at least one day. Using the updated TeamCity EC2 plugin UI, you can now specify tags to locate a suitable host.
* You can now specify multiple instance types for a cloud image, which increases your chances to book a spot instance.
* The **Subnets** field now accepts multiple values, which allows you to specify different sets of incoming and outgoing traffic rules.
* TeamCity can now automatically choose Regions or Availability Zones in which your spot requests are most likely to succeed based on their [spot placement scores](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/spot-placement-score.html#sps-example-configs). To allow TeamCity request and utilize these scores, add the `ec2:GetSpotPlacementScores` [IAM permission](setting-up-teamcity-for-amazon-ec2.md#Required+IAM+permissions).

Learn more: [](setting-up-teamcity-for-amazon-ec2.md).


### S3 Plugin Update
{product="tc"}

Version 2023.11 ships an updated S3 plugin that allows you to create AWS S3 and S3-compatible storages for your build artifacts.

<img src="dk-s3-storage-overview.png" width="706" alt="Updated S3 Plugin"/>

The updated plugin version features the following enhancements:

* Buckets with enabled [Transfer Acceleration](https://aws.amazon.com/s3/transfer-acceleration/) are supported.
* All connection settings are automatically retrieved from the selected [AWS Connection](configuring-connections.md#AmazonWebServices).
* The AWS region is automatically retrieved from the selected bucket.
* You can now disable integrity verification that TeamCity carries out by default for all custom S3 storages.


Learn more: [](storing-build-artifacts-in-amazon-s3.md) | [Upgrade Notes](upgrade-notes.md#2023-11-s3-update)


### AWS Connection Improvements
{product="tc"}

Starting with this version, you can enable or disable the **Available for sub-projects** and **Available for build steps** settings in AWS connection settings. These options allow you to ensure the configured connections are not used by unauthorized TeamCity projects and their [](aws-credentials.md) build features.

<img src="dk-shareAwsConnections.png" width="706" alt="Share AWS connections"/>

In addition, you can now refer to the new section of our "Configuring Connections" documentation article to learn how to configure secure AWS connections that follow Amazon guidelines and do not require permanent user credentials: [](configuring-connections.md#Recommended+Setup).
{product="tc"}

Learn more: [](configuring-connections.md#AmazonWebServices).


## Agents with Bundled JDKs
{product="tc"}

Starting with version 2023.11, you can build distributions of TeamCity agents bundled with custom JDKs. These distributions allow you to install both an agent and a JDK it requires to operate in one go.

<img src="dk-fullAgentDistributions.png" width="706" alt="Full agent distributions page"/>

To create a custom agent distribution, navigate to **Administration | Agent JDKs** and add a new JDK option (you will need to specify the platform, the architecture, and a link for TeamCity to download this specific JDK).

<img src="dk-addAgentJDK.png" alt="Add Agent JDK" width="706"/>

When a new option is added, TeamCity will start building your custom agent distribution. You can download custom agent+JDK bundles by clicking **Install agent | Full distributions** on the **Agents | Overview** page.

Learn more: [](install-teamcity-agent.md).


## .NET
{product="tc"}


* Build agents now report the `DotNetWorkloads_<version>` parameter that returns all [.NET workloads](https://learn.microsoft.com/en-us/dotnet/core/tools/dotnet-workload-install) installed on the agent machine. Learn more: [](net.md#Parameters+Reported+by+Agent).

* The [](net.md) runner now provides the **Excluded test assemblies** setting for the `vstest` command. This field allows you to specify paths to files the command should ignore.

    <img src="dk-dotnet-vstestExclide.png" width="706" alt="Excluded assemblies for vstest"/>

* <include src="parallel-tests.md" include-id="alternative-dotnet-parallel-filtering-tc"/>


## Aggregate Batch Build Artifacts
{product="tc"}


When you run a build configurations that employs the [](parallel-tests.md) build feature, TeamCity splits a build into batches interconnected in an automatically generated [chain](build-chain.md). In previous version, [artifacts](build-artifact.md) produced during such builds were published in these individual batch builds, while a parent build had none.

<img src="dk-artifacts-parallelTestsMain.png" width="706" alt="Artifacts in parallel testing"/>

When viewing completed configuration builds, you could switch to the **Dependencies** tab to access these artifacts.

<img src="dk-artifacts-parallelTests.png" width="706" alt="Artifacts in parallel testing 2"/>

Starting with this version, artifacts produced by batch builds are aggregated in the **Artifacts** tab of a main build. You can also use the `teamcity.build.parallelTests.currentBatch` [parameter](configuring-build-parameters.md) to arrange artifacts produced by batch builds into different directories.

<img src="dk-artifacts-parallelBuildAggregate.png" width="706" alt="Aggregated artifacts"/>


Learn more: [](parallel-tests.md#Publish+Artifacts+Produced+By+Batch+Builds).


## Step Statuses and IDs
{product="tc"}

Starting with version 2023.11, you can specify IDs for your steps (similarly to project and configuration IDs).

<img src="dk-stepID.png" width="706" alt="Step ID"/>

TeamCity uses these IDs to generate new `teamcity.build.step.status.<step_ID>` parameters that report the step exit statuses ("success", "failure", or "cancelled").

<img src="dk-parametersTab-statuses.png" width="706" alt="Step statuses"/>

You can use these values to perform custom actions depending on the statuses of previous steps. For example, you can craft custom step execution conditions.

Learn more: [](configuring-build-steps.md#Step+Status+Parameters).


## Additional ReSharper Plugins for the Inspections Runner
{product="tc"}

The [](inspections-resharper.md) runner now features the **R# CLT Plugins** field that allows you to enter a list of required [ReSharper plugins](https://plugins.jetbrains.com/resharper).

<img src="dk-inspections-plugins.png" width="706" alt="ReSharper plugins list"/>

Learn more: [](inspections-resharper.md#JetBrains+ReSharper+Command+Line+Tools+Settings).


## Service Messages
{product="tc"}

Added the [new service message](service-messages.md#Writing+the+File+into+the+Build+Log) that allows you to track the contents of the given file and echo new lines to the build log.

<img src="dk-streamFiletoLog.png" width="706" alt="Stream file to log"/>

Learn more: [](service-messages.md#Writing+the+File+into+the+Build+Log).


## REST API
{product="tc"}


You can now send a `POST` request to the following endpoint to move a build configuration to another project:

```Shell
/app/rest/buildTypes/<BuildTypeLocator>/copy?<Target_ProjectLocator>
```
{prompt="POST"}

For example, the following request finds a build configuration with the "SourceProject_MyBuildConfig" ID and moves it to "MyProject2":

```Shell
http://localhost:8111/app/rest/buildTypes/id:SourceProject_MyBuildConfig/move?targetProjectId=MyProject2
```
{prompt="POST"}


## Sakura UI
{product="tc"}

* We have reworked the **Agent Parameters** tab. You can navigate to this tab when viewing any TeamCity agent to instantly check this agent's configuration and environment paramters and system properties.

  <img src="dk-sakura-agentParameters.png" width="706" alt="New Agent Parameters tab"/>

* You can now bookmark required agent pools to easily access them from the top of the agents and pools list. Learn more: [](configuring-agent-pools.md#Favorite+Pools).

* The [](build-results-page.md#Dependencies+Tab) now displays a find panel that allows you to search for specific dependent builds by configuration names.






## Miscellaneous
{product="tc"}

* The [DslContext](https://www.jetbrains.com/help/teamcity/kotlin-dsl-documentation/root/dsl-context/index.html?query=DslContext) object now exposes a string `serverUrl` property that allows you to get the URL of a TeamCity server in Kotlin DSL code.
* TeamCity distributes agents more effectively and processes large build chains with failing builds faster. Starting with version 2023.11, dependent builds whose ["On failed dependency" condition](snapshot-dependencies.md) is "Make build failed to start" no longer wait for an available agent when their dependencies fail or are canceled. Instead, the dependent build's status changes to "Failed to start" as soon as possible, and TeamCity proceeds to the next build in the chain.
* The [](commit-status-publisher.md) build feature now correctly publishes build statuses for configurations that target `refs/(merge-requests/*)/head` branches of GitLab repositories (the "merge result" branches). Previously, running TeamCity builds for merge result revisions caused the Publisher to encounter HTTP 404 errors.
* If users log into TeamCity using credentials of an external 2FA-protected service, TeamCity does not send additional 2FA requests. Learn more: [](managing-two-factor-authentication.md#Reduce+Excessive+Authorization+Requests).
* [](performance-monitor.md) now shows absolute values of the consumed/total agent memory.

* You can now add the `dateFormat=<value>` parameter to URLs used by your log analysis tools to retrieve build logs. Learn more: [](build-log.md#Modify+the+DateTime+Pattern).



<!-- #ENDREGION -->








<!-- #REGION TCC -->

## Agents with Bundled JDKs
{product="tcc"}

Starting with version 2023.11, you can build distributions of TeamCity agents bundled with custom JDKs. These distributions allow you to install both an agent and a JDK it requires to operate in one go.

<img src="dk-fullAgentDistributions.png" width="706" alt="Full agent distributions page"/>

To create a custom agent distribution, navigate to **Administration | Agent JDKs** and add a new JDK option (you will need to specify the platform, the architecture, and a link for TeamCity to download this specific JDK).

<img src="dk-addAgentJDK.png" alt="Add Agent JDK" width="706"/>

When a new option is added, TeamCity will start building your custom agent distribution. You can download custom agent+JDK bundles by clicking **Install agent | Full distributions** on the **Agents | Overview** page.

Learn more: [](install-teamcity-agent.md).



## Seamless GitHub App Registration
{product="tcc"}


In version 2023.05, we introduced the [new type](configuring-connections.md#GitHub) of connections to GitHub and GitHub Enterprise. These connections utilize [GitHub Apps](https://docs.github.com/en/apps/using-github-apps/about-using-github-apps), instead of the traditional OAuth-based access to repositories.

Starting with version 2023.11, you will be able to create these connections much faster, without manually setting up and registering new apps in GitHub. Choose the **Automatic** creation mode in the **Add connection** dialog and TeamCity will do the rest.

<img src="dk-GhAppManifestButton.png" width="706" alt="GitHub Manifest App Button"/>

Learn more: [Configuring Connections](configuring-connections.md#GitHub).


## AWS Connection Improvements
{product="tcc"}

Starting with this version, you can enable or disable the **Available for sub-projects** and **Available for build steps** settings in AWS connection settings. These options allow you to ensure the configured connections are not used by unauthorized TeamCity projects and their [](aws-credentials.md) build features.

<img src="dk-shareAwsConnections.png" width="706" alt="Share AWS connections"/>

Learn more: [](configuring-connections.md#AmazonWebServices).

## Token-Based Authentication
{product="tcc"}

Version 2023.11 allows your [Pull Request](pull-requests.md) features to utilize tokens to access repositories hosted on Bitbucket Cloud and Server/Data Center.

* For Bitbucket Server/Data Center repositories, the Pull Requests feature can now use refreshable OAuth tokens issued via [TeamCity connections](configuring-connections.md#Bitbucket+Server+and+Data+Center).

* For Bitbucket Cloud, you can specify permanent access tokens issued for a specific repository, project, or workspace.

Learn more: [](pull-requests.md#Bitbucket+Server+Pull+Requests) | [](pull-requests.md#Bitbucket+Cloud+Pull+Requests)



## Miscellaneous
{product="tcc"}


* You can now bookmark required agent pools to easily access them from the top of the agents and pools list. Learn more: [](configuring-agent-pools.md#Favorite+Pools).
* <include src="parallel-tests.md" include-id="alternative-dotnet-parallel-filtering-tcc"/>

<!-- #ENDREGION -->




## Upgrade Notes
{product="tc"}

Before upgrading, we highly recommend reading about important changes in version [2023.11 compared to 2023.05.4](upgrade-notes.md#Changes+from+2023.05.4+to+2023.11).

## Fixed Issues
{product="tc"}

See [TeamCity 2023.11 release notes](teamcity-2023-11-release-notes.md).



## Roadmap

See the [TeamCity roadmap](https://www.jetbrains.com/teamcity/roadmap/#teamcity-roadmap) to learn about future updates.

## We'd Love to Hear From You


We place a high value on your feedback and encourage you to share your thoughts and suggestions. See this link for more information: [Feedback](feedback.md).


