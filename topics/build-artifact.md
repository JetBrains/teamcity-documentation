[//]: # (title: Build Artifact)
[//]: # (auxiliary-id: Build Artifact)

_Build artifacts_ are files produced by a build. Typically, these include distribution packages, WAR files, reports, log files, and so on. When creating a build configuration, you specify the paths to the artifacts of your build on the [Configuring General Settings](configuring-general-settings.md#Artifact+Paths) page.

## Artifacts Storage

TeamCity contains an integrated lightweight builds artifact repository. The artifacts are stored either on the [server-accessible file system](configuring-artifacts-storage.md#Built-in+Artifacts+Storage) or on an [external storage](configuring-artifacts-storage.md#External+Artifacts+Storage).

Upon the build finish, TeamCity searches for artifacts in the build [checkout directory](build-checkout-directory.md) according to the specified [artifact path or path patterns](configuring-general-settings.md#Artifact+Paths). The matching files are then uploaded ("published") to the TeamCity server, where they become available for downloading through the web UI or can be used in other builds using [artifact dependencies](dependent-build.md#Artifact+Dependency). You can [choose when to publish artifacts](configuring-general-settings.md#publish-artifacts): for all completed builds, only for successful builds, or for all builds, even the interrupted ones.

To download artifacts of a build, go to the [Artifacts](working-with-build-results.md#Build+Artifacts) tab of the build results page or use the artifacts icon ![artifactIcon.png](artifactIcon.png) available on the project or build configuration __Overview__ page and on the TeamCity pages that list the builds.   
If the artifacts are stored as an archive, you can still browse files inside this archive.

You can automate artifacts downloading as described in the [Patterns For Accessing Build Artifacts](patterns-for-accessing-build-artifacts.md) section.

In case of the built-in storage, TeamCity keeps artifacts on the disk in a directory structure that can be accessed directly (for example, by configuring the operating system to share the directory over the network). The storage format is described in [TeamCity Data Directory](teamcity-data-directory.md#artifacts). The artifacts are stored on the server "as is" without additional compression. By default, the artifacts are stored under the \<[TeamCity Data Directory](teamcity-data-directory.md)\>\/system\/artifacts directory which [can be changed](teamcity-configuration-and-maintenance.md).   
You can [configure an external artifacts](configuring-artifacts-storage.md#External+Artifacts+Storage) storage to replace the built-in one.

Build artifacts can also be uploaded to the server while the build is still running. To instruct TeamCity to upload the artifacts, the build script should be modified to send [service messages](build-script-interaction-with-teamcity.md#Publishing+Artifacts+while+the+Build+is+Still+in+Progress).

## Hidden Artifacts

In addition to user-defined artifacts, TeamCity also generates and publishes some artifacts for internal purposes. These are called hidden artifacts.   
For example, for Maven builds, TeamCity creates the `maven-build-info.xml` file that contains Maven-specific data collected during the build. The content of the file is then used to visualize the Maven data on the Maven Build Info tab in the build results.
* Hidden artifacts are placed under the `.teamcity` directory in the root of the build artifacts.
* Hidden artifacts are not listed on the __Artifacts__ tab of the build results by default. However, below the list of the artifacts there's a link that allows you to view hidden artifacts if any. When hidden artifacts are displayed, clicking the _Download all_ link will result in downloading all artifacts including hidden ones.
* Artifacts dependencies do not download hidden artifacts unless they explicitly have "`.teamcity`" in the pattern.
* Hidden artifacts are not deleted by the artifacts clean-up unless ".`teamcity`" is explicitly specified in the pattern.

You can configure publishing some builds artifacts under the `.teamcity` directory to make them hidden.

Some of the hidden artifacts are:
* `maven-build-info.xml.gz` – Maven build data. Used to display data on the __Maven Build Info__ build's tab.
* `properties` directory – holds properties calculated for the build on the agent. There are properties actual before the build and after the build. These are displayed on the build __Properties__ tab.
* `.NETCoverage` – raw .NET coverage data (for example, used to open dotCover data in VS addin).
* `coverage_idea` – raw IntelliJ IDEA coverage data (for example, used to open coverage in IDEA).


[//]: # (Internal note. Do not delete. "Build Artifactd28e144.txt")    


### Artifacts Cache on Agent

All artifacts published by a build are stored in the agent's artifacts cache in the `<Build Agent home>\system\.artifacts_cache` directory, which helps speed up artifact dependencies in some cases.   
However, depending on the size of artifacts, [clean-up](clean-up.md), and other settings, artifacts caching may cause low disk space on the agent. You can [configure](free-disk-space.md#Configuring+artifacts+cache) storing published artifacts in the agent cache.

__  __

__See also:__


__Concepts__: [Dependent Build](dependent-build.md)   
__Administrator's Guide__: [Configuring General Settings](configuring-general-settings.md) | [Configuring Dependencies](configuring-dependencies.md) | [Patterns For Accessing Build Artifacts](patterns-for-accessing-build-artifacts.md)

__ __