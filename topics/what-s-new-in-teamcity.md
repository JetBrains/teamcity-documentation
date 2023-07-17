[//]: # (title: What's New in TeamCity 2023.07)
[//]: # (auxiliary-id: What's New in TeamCity 2023.07;What's New in TeamCity)


## EC2 Plugin Update

In version 2023.07, we have overhauled the Amazon EC2 integration plugin. Apart from a refreshed user interface, the updated plugin features the following enhancements:

* Support for Mac AMIs. Note that these machine images can be run only on dedicated Mac Mini hosts that should be booked for at least one day.
* You can now specify multiple instance types for a cloud image, which increases your chances to book a spot instance.
* The **Subnets** field now accepts multiple values, meaning you can specify different sets of incoming and outgoing traffic rules.
* TeamCity can now automatically choose Regions or Availability Zones in which your spot requests are most likely to succeed based on their [spot placement scores](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/spot-placement-score.html#sps-example-configs). To allow TeamCity request and utilize these scores, add the `ec2:GetSpotPlacementScores` [IAM permission](setting-up-teamcity-for-amazon-ec2.md#Required+IAM+permissions).

Learn more: [](setting-up-teamcity-for-amazon-ec2.md).

## Aggregate Batch Build Artifacts

When you run a build configurations that employs the [](parallel-tests.md) build feature, TeamCity splits a build into batches interconnected in an automatically generated [chain](build-chain.md). In previous version, [artifacts](build-artifact.md) produced during such builds were published in these individual batch builds, while a parent build had none.

<img src="dk-artifacts-parallelTestsMain.png" width="706" alt="Artifacts in parallel testing"/>

When viewing completed configuration builds, you could switch to the **Dependencies** tab to access these artifacts.

<img src="dk-artifacts-parallelTests.png" width="706" alt="Artifacts in parallel testing 2"/>

Starting with this version, artifacts produced by batch builds are aggregated in the **Artifacts** tab of a main build. You can also use the `teamcity.build.parallelTests.currentBatch` [parameter](configuring-build-parameters.md) to arrange artifacts produced by batch builds into different directories.

<img src="dk-artifacts-parallelBuildAggregate.png" width="706" alt="Aggregated artifacts"/>


Learn more: [](parallel-tests.md#Publish+Artifacts+Produced+By+Batch+Builds).


## .NET

Build agents now report the `DotNetWorkloads_<version>` parameter that returns all [.NET workloads](https://learn.microsoft.com/en-us/dotnet/core/tools/dotnet-workload-install) installed on the agent machine.

Learn more: [](net.md#Parameters+Reported+by+Agent).

## REST API

You can now send a `POST` request to the following endpoint to move a build configuration to another project:

```Shell
/app/rest/buildTypes/<BuildTypeLocator>/copy?<Target_ProjectLocator>
```
{prompt="POST"}

For example, the following request finds a build configuration with the "SourceProject_MyBuildConfig" ID and moves it to "MyProject2":

```Shell
http://localhost:8111/app/rest/buildTypes/id:SourceProject_MyBuildConfig/copy?targetProjectId=MyProject2
```
{prompt="POST"}

## Miscellaneous

* The [DslContext](https://www.jetbrains.com/help/teamcity/kotlin-dsl-documentation/root/dsl-context/index.html?query=DslContext) object now exposes a string `serverUrl` property that allows you to get the URL of a TeamCity server in Kotlin DSL code.

## Upgrade Notes
{product="tc"}

Upgrade notes are not published for TCC.

## Fixed Issues
{product="tc"}

Release notes are not published for TCC.



## Roadmap

See the [TeamCity roadmap](https://www.jetbrains.com/teamcity/roadmap/#teamcity-roadmap) to learn about future updates.

## We'd Love to Hear From You


We place a high value on your feedback and encourage you to share your thoughts and suggestions. See this link for more information: [Feedback](feedback.md).
