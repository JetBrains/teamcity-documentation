[//]: # (title: What's New in TeamCity 2023.07)
[//]: # (auxiliary-id: What's New in TeamCity 2023.07;What's New in TeamCity)

## Aggregate Batch Build Artifacts

When you run a build configurations that employs the [](parallel-tests.md) build feature, TeamCity splits a build into batches interconnected in an automatically generated [chain](build-chain.md). In previous version, [artifacts](build-artifact.md) produced during such builds were published in these individual batch builds, while a parent build had none.

<img src="dk-artifacts-parallelTestsMain.png" width="706" alt="Artifacts in parallel testing"/>

When viewing completed configuration builds, you could switch to the **Dependencies** tab to access these artifacts.

<img src="dk-artifacts-parallelTests.png" width="706" alt="Artifacts in parallel testing 2"/>

Starting with this version, artifacts produced by batch builds are aggregated in the **Artifacts** tab of a main build. You can also use the new `teamcity.build.parallelTests.currentBatch` [parameter](configuring-build-parameters.md) to arrange artifacts produced by batch build into different directories.

<img src="dk-artifacts-parallelBuildAggregate.png" width="706" alt="Aggregated artifacts"/>


Learn more: [](parallel-tests.md#Publish+Artifacts+Produced+By+Batch+Builds).

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
