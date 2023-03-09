[//]: # (title: What's New in TeamCity 2023.03)
[//]: # (auxiliary-id: What's New in TeamCity 2023.03;What's New in TeamCity)

## Integration with Bitbucket Server and Data Center

In addition to Bitbucket Cloud, TeamCity now supports Bitbucket Server and Data Center. The corresponding option is available in the connection types list, and on the **Create Project** page.

<img src="dk-whatsnew202303-bbserver.png" width="708" alt="TeamCity integration with Bitbucket Server and Data Center"/>

See the following links for more information: [Configuring Connections](configuring-connections.md#Bitbucket+Server+and+Data+Center) | [Creating and Editing Projects](creating-and-editing-projects.md#Creating+project+pointing+to+Bitbucket)


## Run Steps only for Failed Builds

You can now choose the "Only if build status is failed" [execution policy](configuring-build-steps.md#Execution+Policy) for individual steps. This policy allows you to create steps that will be ignored when your build finishes successfully and executed only when it fails.


<img src="dk-whatsnew2303-onlywhenfails.png" width="706" alt="Run the build step only when the build fails"/>


## New Build Cache Feature

We're expanding the list of [build features](adding-build-features.md) with **Build Cache** — the feature that allows you to cache files used by a build configuration (for example, Maven dependencies or NodeJS packages). Cached files can be reused by the same build configuration during subsequent runs, or shared with other configurations that belong to the same project. When a build starts, a TeamCity agent downloads the latest cache version to the project's working directory, thus optimizing and accelerating the build process.

<img src="dk-buildCache-split.png" width="706" alt="Reuse caches published by other configurations"/> 

Build Cache is currently available as a community technology preview (CTP). Navigate to **Administration | Experimental Features** and tick a corresponding checkbox to enable this build feature.

## Server Health Reports for Archived Projects

You can now generate [Server Health reports](server-health.md) for archived projects. To do this, select the *&lt;Archived Projects&gt;* scope.

<img src="dk-serverHealthArhived.png" width="706" alt="Server Health Reports for Archived Projects"/>



## Fixed issues
{product="tcc"}

See [TeamCity Build 130000 release notes](teamcity-release-notes-build-130000.md).

## Roadmap

See the [TeamCity roadmap](https://www.jetbrains.com/teamcity/roadmap/#teamcity-roadmap) to learn about future updates.