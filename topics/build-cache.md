[//]: # (title: Build Cache)

The "Build Cache" feature allows build configurations to publish specific files produced during a build run (for example, downloaded [npm](nodejs.md) packages or [Maven local repository artifacts](https://maven.apache.org/guides/introduction/introduction-to-repositories.html)). Published caches can be reused during subsequent builds by the same configuration that published them, or other configurations. This technique optimizes and accelerates building routines.

## Common Information

### Cache Publishers and Consumers

You can set up a Build Cache feature to operate in either of two modes.

* Initial Build Cache settings allow the feature to download caches published by the same feature during a previous build.

* Alternatively, you can set one Build Cache feature to publish a cache, and another Build Cache (from a different build configuration) to download it. This mode allows you to set up "publisher" and "consumer" features to exchange caches between different build configurations. The number of cache consumer features is unlimited, however, they should belong to the **same project** as the cache publisher.

### Size Limits

Caches are published as [hidden artifacts](build-artifact.md#Hidden+Artifacts) under the `.teamcity.build_cache` folder. To view a published cache, click the **Show hidden artifacts** link on the [](build-results-page.md#Artifacts+Tab).

<img src="dk-publishedCaches.png" width="706" alt="Published artifacts"/>

Since caches are published as artifacts, they are affected by the **Maximum build artifact file size** setting that you can set on the [Administration | Global Settings](teamcity-configuration-and-maintenance.md#Build+Settings) page.

### Order of Operations

If a build configuration is configured to upload caches, it arranges its build stages in the following order:

1. Resolve artifact dependencies
2. Download caches
3. Checkout the sources
4. Start the build


## Publish and Use Cache Within the Same Build Configuration

This section illustrates how to set up the Build Cache feature that allows a build configuration to reuse caches from its own previous builds.

1. <snippet id="settings-add-feature"><a href="adding-build-features.md">Add the build feature</a> to a build configuration and specify a unique <b>Cache Name</b>.</snippet>
2. Since we want the feature to both publish and use cache, leave both **Publish** and **Use Cache** settings enabled.
3. <snippet id="settings-specify-paths">Specify paths to files and folders that should be cached. Each path should start from a new line. Wildcards are not supported. Relative paths are resolved against checkout directories.<br/>

   For example, to cache NodeJS packages downloaded by the <code>npm install</code> or <code>yarn install</code> commands, type <code>node_modules/</code> in this field.
   
   <tip>
   To cache local Maven artifacts, set the <b>Artifact repository</b> setting of your <a href="maven.md">Maven</a> runner to <b>Maven default</b>. If you choose the <b>Per agent</b> mode, use the default .m2 location in the feature's publishing rules. Alternatively, add the <code>-Dmaven.repo.local</code> parameter to additional runner commands and point the Build Cache feature to the same directory.

   <!--teamcity.agent.home.dir/system/jetbrains.maven.runner/maven.repo.local-->
   
   </tip></snippet>

4. <snippet id="settings-publish-if-changed">By default, new builds do not publish caches if they are identical to those published by previous builds. If you wish each build to upload a cache, uncheck the <b>Publish only if changed</b> setting.</snippet>
   
5. Save the settings. The feature's description on the **Build Features** page should state that it publishes and uses the same cache.

   <img src="dk-buildCaches-singleConfDescription.png" width="706" alt="Build Cache feature description"/>

6. <snippet id="settings-check-if-published">Run the build and ensure all cached files are available on the build results page as the ".teamcity.build_cache" <a href="build-artifact.md#Hidden+Artifacts">hidden artifact</a>.</snippet>


7. To confirm that cache published during the previous run is used, run the build again and check the build log for corresponding messages.



## Exchange Caches Between Separate Build Configurations

In this setup, the "publisher" Build Cache feature added to one build configuration publishes its cache, while the "consumer" Build Cache feature added to another configuration downloads this cache. The dowloaded cache is placed to the same location that matches the publisher's **Publishing rules** path.


You can set up as many publisher and consumer features as required as long as you add features for configurations that belong to the same project.

> One Build Cache feature can publish and use only one cache archive. If you need to utilize caches published by multiple build configurations, add the corresponding number of Build Cache features.
>
{style="tip"}

### Set Up a Publisher

1. <include from="build-cache.md" element-id="settings-add-feature"/>
2. <include from="build-cache.md" element-id="settings-specify-paths"/>
3. <include from="build-cache.md" element-id="settings-publish-if-changed"/>
4. Uncheck the **Use Cache** checkbox.
5. Save the settings. The feature's description on the **Build Features** page should state that it only publishes the cache.

   <img src="dk-buildCaches-onlyPublish.png" width="706" alt="Only publish"/>
   
6. <include from="build-cache.md" element-id="settings-check-if-published"/>

### Set Up a Consumer

1. [Add the build feature](adding-build-features.md) to a build configuration and specify the same **Cache Name** the publisher feature uses.
2. Uncheck the **Publish** checkbox.
3. Save the settings. The feature's description on the **Build Features** page should state that it only uses the cache.
   
   <img src="dk-buildCaches-onlyUse.png" width="706" alt="Use only"/>

4. Run a new build and check the build log to ensure the required cache file is downloaded.
5. Repeat the steps above for every build configuration that needs this published cache.


## See Also

* [Artifacts Cache on Agents](build-artifact.md#Artifacts+Cache+on+Agent)
* [Data Clean-Up](teamcity-data-clean-up.md)