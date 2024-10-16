[//]: # (title: Build Artifact)
[//]: # (auxiliary-id: Build Artifact)

_Build artifacts_ are files produced by a build. Typically, these include distribution packages, WAR files, reports, log files, and so on. When creating a build configuration, you specify the paths to the artifacts of your build on the [Configuring General Settings](configuring-general-settings.md#Artifact+Paths) page.

> This article covers the basic build artifact concepts. See the article links in the page footer for details of configuring and administering artifacts.
>
{style="tip"}

## Artifacts Storage

TeamCity contains an integrated lightweight builds artifact repository. The artifacts are stored either on the [server-accessible file system](configuring-artifacts-storage.md#Built-in+Artifacts+Storage) or on an [external storage](configuring-artifacts-storage.md#external-artifacts-storage).
{product="tc"}

TeamCity contains an integrated lightweight [Amazon S3](https://aws.amazon.com/s3) builds artifact repository.
{product="tcc"}

Upon the build finish, TeamCity searches for artifacts in the build [checkout directory](build-checkout-directory.md) according to the specified [artifact path or path patterns](configuring-general-settings.md#Artifact+Paths). The matching files are then uploaded ("published") to the TeamCity server, where they become available for downloading through the web UI or can be used in other builds using [artifact dependencies](dependent-build.md#Artifact+Dependency). You can [choose when to publish artifacts](configuring-general-settings.md#publish-artifacts): for all completed builds, only for successful builds, or for all builds, even the interrupted ones.

To download artifacts of a build, go to the [Artifacts](build-results-page.md#Artifacts+Tab) tab of the build results page or use the artifacts icon ![artifactIcon.png](artifactIcon.png) available on the project or build configuration __Overview__ page and on the TeamCity pages that list the builds.

> If you're using Safari and experiencing issues with incomplete artifact downloads, disable the "Open "safe" files after downloading" option in Safari general settings.
>
{style="tip"}

<anchor name="artifacts-as-archive"/>

>If you want to publish many artifacts in one build, we suggest that you pack them into an archive beforehand. This will make the publishing and the following downloads significantly faster. You will still be able to browse files within an archive in the build results and access archived files individually via [REST API](https://www.jetbrains.com/help/teamcity/rest/manage-finished-builds.html#Get+Build+Artifacts).
> 
>TeamCity can automatically create an archive from a directory when publishing build artifacts. To configure this behavior, you need to specify the build artifact path as follows: `directory => directory.*`, where `*` is the archive extension (like `directory.zip`). See more information and examples [here](configuring-general-settings.md#Artifact+Paths).
> 
{style="note"}

In case of the built-in storage, TeamCity keeps artifacts on the disk in a directory structure that can be accessed directly (for example, by configuring the operating system to share the directory over the network). The storage format is described in [TeamCity Data Directory](teamcity-data-directory.md#artifacts). The artifacts are stored on the server "as is" without additional compression. By default, the artifacts are stored under the `<[TeamCity Data Directory](teamcity-data-directory.md)\>/system/artifacts` directory which [can be changed](teamcity-configuration-and-maintenance.md).   
You can [configure an external artifacts](configuring-artifacts-storage.md#external-artifacts-storage) storage to replace the built-in one.
{product="tc"}

Build artifacts can also be uploaded to the server while the build is still running. To instruct TeamCity to upload the artifacts, the build script should be modified to send [service messages](service-messages.md#Publishing+Artifacts+While+Build+is+in+Progress).

You can automate artifacts downloading via [REST API](https://www.jetbrains.com/help/teamcity/rest/manage-finished-builds.html#Get+Build+Artifacts).

## Hidden Artifacts

In addition to user-defined artifacts, TeamCity also generates and publishes some _hidden artifacts_ for internal purposes.  
For example, for Maven builds, TeamCity creates the `maven-build-info.xml` file that contains Maven-specific data collected during the build. The content of the file is then used to visualize the Maven data on the Maven Build Info tab in the build results.

TeamCity stores hidden artifacts in the `.teamcity` directory in the artifacts' root.

[Artifact dependencies](artifact-dependencies.md) do not consider hidden artifacts, unless they explicitly specify `.teamcity` in their pattern.

Hidden artifacts are not deleted by the artifacts clean-up unless `.teamcity` is explicitly specified in the clean-up scope.

Examples of hidden artifacts:
* `maven-build-info.xml.gz` — Maven build data. Used to display data on the __Maven Build Info__ tab.
* the `properties` directory — holds properties calculated for the build on the agent. There are properties actual before the build and after the build. These are displayed on the __Properties__ tab of build results.
* `.NETCoverage` — raw .NET coverage data (for example, used to open dotCover data in VS add-in).
* `coverage_idea` — raw IntelliJ IDEA coverage data (for example, used to open coverage in IDEA).

### Retrieve Hidden Artifacts

Hidden artifacts are not displayed on the __Artifacts__ tab by default. If there are any of them in the current build, you can reveal them by clicking __Show hidden artifacts__.

To browse or download artifacts individually, use the artifacts' tree. To download the whole `.teamcity` directory, click __Download all (.zip)__ in the right part of the screen.

### Hide Artifacts

To hide an artifact, you need to publish it under the `.teamcity` directory.

[//]: # (Internal note. Do not delete. "Build Artifactd28e144.txt")

## Artifacts Cache on Agent

All artifacts published by a build are stored in the agent's artifacts cache in `<Build Agent home>\system\.artifacts_cache`, which helps speed up artifact dependencies in some cases.   
However, depending on the size of artifacts, [clean-up](teamcity-data-clean-up.md), and other settings, artifacts caching may cause low disk space on the agent. You can [configure](free-disk-space.md#Disabling+Artifacts+Cache) storing published artifacts in the agent cache.

## Build Artifacts Video Guide

<video src="https://youtu.be/mNYq424IQ-w"
title="TeamCity tutorial — How to work with artifacts (logs, graphics, binaries)"/>

<seealso>
        <category ref="concepts">
            <a href="dependent-build.md">Dependent Build</a>
        </category>
        <category ref="admin-guide">
            <a href="configuring-general-settings.md#Artifact+Paths">Configuring General Settings</a>
            <a href="configuring-dependencies.md">Configuring Dependencies</a>
            <a href="configuring-artifacts-storage.md" product="tc">Configuring Artifacts Storage</a>
            <a href="patterns-for-accessing-build-artifacts.md">Patterns For Accessing Build Artifacts</a>
        </category>
</seealso>