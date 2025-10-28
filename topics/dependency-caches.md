[//]: # (title: Dependency Caches)


**Maven Cache**, **Gradle Cache**, and **NuGet Cache** are configuration-free build features that speed up builds by allowing related steps to reuse dependencies downloaded during previous builds of the same build configuration. In addition, caches are shared between steps of the same build, given that these steps utilize the same dependency directory.

* Maven Cache — caches dependencies used by [](maven.md) build steps
* Gradle Cache — caches dependencies used by [](gradle.md) build steps
* NuGet Cache — caches NuGet packages used by [](net.md) build steps



## Common Principles

All "... Cache" build features operate in a similar manner. Caches undergo identical creation/publishing/validation/reuse phases regardless of the build feature type.


<deflist type="narrow">
    <def title="Caching">
        The initial cache is formed during the first build run. To do this, a build feature caches the contents of the following tracked directories:
        <list type="bullet">
            <li><b>Maven:</b> <a href="https://maven.apache.org/guides/introduction/introduction-to-repositories.html">local Maven repositories</a>. The default path depends on step <a href="maven.md#MavenLocalArtifactRepositorySettings">local artifact repository settings</a>.</li>
            <li><b>NuGet:</b> <a href="https://learn.microsoft.com/en-us/nuget/consume-packages/managing-the-global-packages-and-cache-folders">global package folder</a>. The default path is <code>${user.home}/.nuget/packages</code>.</li>
            <li><b>Gradle:</b> <a href="https://docs.gradle.org/current/userguide/dependency_resolution.html#sub:cache_copy">Gradle cache directory</a>. The default path is <code>${user.home}/.gradle/caches/modules-&lt;version&gt;</code>.</li>
        </list>
    </def>
    <def title="Publishing" instance="tc">
        After a cache was created, the build feature publishes it to the <a href="configuring-artifacts-storage.md" instance="tc">artifact storage</a>, making it easily accessible to agents that process subsequent builds.
    </def>
    <def title="Publishing" instance="tcc">
        After a cache was created, the build feature publishes it to the artifact storage, making it easily accessible to agents that process subsequent builds.
    </def>
    <def title="Validation">
        A cache is routinely checked to determine if it is still valid or needs replacement. A build feature can mark a cache as invalid at the start or end of a build (before or after executing build steps) in the following cases:
        <ul>
            <li>At the start:
                <ul>
                    <li instance="tc">If the periodic cleanup is scheduled. Caches are forcibly pruned and republished every 30 days starting from their first publish date. This mechanism ensures caches stay up-to-date. You can modify the cleanup frequency by changing the value of the <code>teamcity.depcache.invalidation.periodical.periodMins</code> <a href="server-startup-properties.md#TeamCity+Internal+Properties">internal property</a>.</li>
                    <li instance="tcc">If the periodic cleanup is scheduled. Caches are forcibly pruned and republished every 30 days starting from their first publish date. This mechanism ensures caches stay up-to-date.</li>
                    <li>If the <code>teamcity.depcache.invalidate</code> <a href="configuring-build-parameters.md">parameter</a> equals <b>true</b></li>
                </ul>
            </li>
            <li>At the end:
                <ul>
                    <li>If the list of build steps using this cache has changed</li>
                    <li>If the project dependency list has changed</li>
                </ul>
            </li>
        </ul>
        When a build feature encounters an invalid cache, it flushes this cache and republishes a newer version after the build ends.
    </def>
    <def title="Usage">
        A build feature unzips cached files from a validated cache to tracked repositories, merging existing file structure with the cached one. If a cache was marked as invalid before the actual build steps started, this build runs without using a cache and re-downloads all required dependencies anew. These fresh dependencies will be used for an updated cache published at the end of this build.
    </def>
</deflist>


## Special Notes and Limitations

### Common

* Dependency and Package caching is most effective on short-lived agents that terminate after a single build, as they avoid accumulating dependencies from other builds. For long-lived or permanent agents, periodically review `.teamcity.build_cache/depcache_***_cacheRoot_***.tar` [hidden artifacts](build-artifact.md#Hidden+Artifacts) to monitor cache size and contents. Each dependency directory is archived in its own cache, so your builds can publish multiple archives. If caches grow excessively due to redundant dependencies, consider disabling this feature.

* The cache is only published if the build was successful. See this ticket for more information: [TW-89838](https://youtrack.jetbrains.com/issue/TW-89838/The-dependency-cache-is-not-saved-and-not-published-for-the-failed-builds-with-failed-tests).

* To enable dependency caching for [builds running in container](container-wrapper.md), set the `-v <dependency_directory>:<dependency_directory>` volume.

* Certain build configuration setups rely on spawning "virtual" builds. For example, when using [](parallel-tests.md) or [](matrix-build.md), each test batch or parameter combination runs in a separate build. Each of these dynamically generated builds publish an identical cache. See the following ticket for more information: [TW-89837](https://youtrack.jetbrains.com/issue/TW-89837/The-same-dependency-cache-is-created-multiple-times-in-every-virtual-build).


### Maven Cache

* The build feature supports all three local Maven repo types: per-agent, per-build, and Maven default.
* If builds that produce and consume caches run inside Linux Docker containers, set the [Artifact repository](maven.md#MavenLocalArtifactRepositorySettings) Maven setting to either "Per agent" (default value) or "Per build configuration". Otherwise, a TeamCity server running as a regular user may lack sufficient permissions to access caches published by steps executed in Docker containers under the "root" user.

### NuGet Cache

* The build feature calls the `dotnet list package --format=json --output-version=1 --include-transitive` command to analyze project dependencies and detect invalid caches. The `--format` parameter is available in .NET SDK 7.0.200 and higher, meaning the build feature cannot operate on agents with older SDK versions.

* The caching is not supported for environments with package locations set via MSBuild-specific ways:

    * via MSBuild command line
    * in a project file
    * in a `Directory.Build.Props` file


## DSL Configuration

The sample below illustrates how to add dependency cache features in [](kotlin-dsl.md).

```Kotlin
object Build : BuildType({
    name = "Build"
    
    features {
        // ...
        gradleCache {}
        mavenCache {}
        nugetCache {}
    }
    // ...
})
```