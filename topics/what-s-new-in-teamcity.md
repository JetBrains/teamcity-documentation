[//]: # (title: What's New in TeamCity 2023.03)
[//]: # (auxiliary-id: What's New in TeamCity 2023.03;What's New in TeamCity)

## Integration with Bitbucket Server and Data Center

In addition to Bitbucket Cloud, TeamCity now supports Bitbucket Server and Data Center. The corresponding option is available in the connection types list, and on the **Create Project** page.

<img src="dk-whatsnew202303-bbserver.png" width="708" alt="TeamCity integration with Bitbucket Server and Data Center"/>

See the following links for more information: [Configuring Connections](configuring-connections.md#Bitbucket+Server+and+Data+Center) | [Creating and Editing Projects](creating-and-editing-projects.md#Creating+project+pointing+to+Bitbucket)


## Run Steps only for Failed Builds

You can now choose the "Only if build status is failed" [execution policy](configuring-build-steps.md#Execution+Policy) for individual steps. This policy allows you to create steps that will be ignored when your build finishes successfully, and employed only when it fails.


<img src="dk-whatsnew2303-onlywhenfails.png" width="708" alt="Run the build step only when the build fails"/>



## Server Health Reports for Archived Projects

You can now generate [Server Health reports](server-health.md) for archived projects. To do this, select the *&lt;Archived Projects&gt;* scope.

<img src="dk-serverHealthArhived.png" width="708" alt="Server Health Reports for Archived Projects"/>



## Fixed issues
{product="tcc"}

See [TeamCity Build 130000 release notes](teamcity-release-notes-build-130000.md).

## Roadmap

See the [TeamCity roadmap](https://www.jetbrains.com/teamcity/roadmap/#teamcity-roadmap) to learn about future updates.