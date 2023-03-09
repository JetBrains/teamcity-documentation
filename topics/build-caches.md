[//]: # (title: Build Cache)

The "Build Cache" feature allows build configurations to keep specific files produced during a build run (for example, downloaded [npm](nodejs.md) packages) and reuse them during subsequent builds. This technique eliminates excessive actions and accelerates building routines.

## Prerequisites

The Build Cache feature is currently available as a community technology preview (CTP) and may be changed in future releases.

To enable this build feature, navigate to the **Administration | Experimental Features** page and tick the corresponding checkbox.

## Publish Caches

1. [Add the build feature](adding-build-features.md) to a build configuration that should cache its files.
2. Since we want the build feature only to publish files, leave the **Publish** checkbox checked and uncheck **Use Cache**.
3. Specify paths (relative to the checkout directory) to files and folders that should be cached. Each path should start from a new line. Wildcards are not supported. The figure below illustrates the Build Cache feature configured to publish NodeJS packages downloaded by the `npm install` or `yarn install` build step.
     
   <img src="dk-buildcaches-paths.png" width="706" alt="Publishing NodeJS packages"/>

4. Run the build to ensure the build feature functions correctly. You should be able to see required files published as a ".teamcity.build_cache" [hidden artifact](build-artifact.md#Hidden+Artifacts) after a build finishes.
    
    <img src="dk-buildCaches-publishedArtifact.png" width="706" alt="Publish build cache"/>


## Obtain and Use Published Caches

When a cache is published, you can configure the required Build Cache features to download this cache to the checkout directory before a build starts. You can do this for:

* [The same build configuration](#Use+Caches+Published+Within+the+Same+Build+Configuration) whose Build Cache feature uploaded the cache;
* [Another build configuration](#Use+Caches+Published+by+Another+Build+Configuration) that belongs to the same project.

### Use Caches Published Within the Same Build Configuration

In this setup, a Build Cache feature reuses the same cache this feature published during the previous build runs.

1. Open the settings of the same Build Cache feature configured in the [Publish Caches](#Publish+Caches) section, and tick the **Use Cache** checkbox.
    
    The build feature description on the configuration's **Build Features** page should now reflect that the feature reuses the same cache it publishes.
    
   <img src="dk-buildCaches-singleConfDescription.png" width="706" alt="Build Cache feature description"/>

2. Run the build configuration and ensure it finishes successfully. You can inspect build logs to check whether build agents download the cache archive.

    <img src="dk-buildCaches-download.png" width="706" alt="Download build cache"/>

### Use Caches Published by Another Build Configuration

In this scenario, Build Cache features download caches published by other Build Cache features. Note that only build configurations of the same project can exchange their caches.

> One Build Cache feature can publish and use only one cache archive. If you need to utilize caches published by multiple build configurations, add the corresponding number of Build Cache features.
>
{type="tip"}

1. Add a Build Cache feature to a build configurations that should download cache published by another (source) configuration.
2. Specify the same **Cache name** as in the publisher build feature.
3. Disable the **Publish** option since this feature should only download a cache. Your settings dialog should look like the following.
   
   <img src="dk-buildcaches-downloadCache.png" width="706" alt="Download a published cache"/>

4. You can check feature descriptions to ensure your setup is correct. The description for a source configuration should state that this feature publishes a cache, and a consumer configuration should say that it uses cache with the same name.

   <img src="dk-buildCache-split.png" width="706" alt="Reuse caches published by other configurations"/> 

5. Repeat the previous steps for all build configurations that should download a cache archive.

## See Also

* [Artifacts Cache on Agents](build-artifact.md#Artifacts+Cache+on+Agent)
* [Data Clean-Up](teamcity-data-clean-up.md)