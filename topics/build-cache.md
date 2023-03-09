[//]: # (title: Build Cache)

The "Build Cache" feature allows build configurations to keep specific files produced during a build run (for example, downloaded [npm](nodejs.md) packages) and reuse them during subsequent builds. This technique optimizes and accelerates building routines.

## Prerequisites

Build Cache is currently available as an experimental feature and may be changed in future releases.

To enable this build feature, navigate to the **Administration | Experimental Features** page and tick the corresponding checkbox.

## Common Concepts

Build Cache feature can work in one- or two-way mode. In one-way mode it either publishes its cache or downloads cache published by another feature. This mode allows you to set up "publisher" and "consumer" features to exchange caches between different build configurations **of the same project**.

In two-way mode the feature downloads caches published by the same feature during a previous build. When you add a new Build Cache features, it is configured to operate in two-way mode and you need to uncheck related settings to disable unwanted behavior.


## Publish and Use Cache Within the Same Build Configuration

This section illustrates how to set up the Build Cache feature that allows a build configuration to reuse caches from its own previous builds.

1. [Add the build feature](adding-build-features.md) to a build configuration and specify a unique **Cache Name**.
2. Since we want the feature to both publish and use cache, leave both **Publish** and **Use Cache** settings enabled.
3. Specify paths to files and folders that should be cached (currently, only paths relative to the checkout directory are supported). Each path should start from a new line. Wildcards are not supported.
   
   The figure below illustrates the Build Cache feature configured to publish NodeJS packages downloaded by the `npm install` or `yarn install` build step.

   <img src="dk-buildcaches-paths.png" width="706" alt="Publishing NodeJS packages"/>
   
4. Save the settings and ensure that the build feature description confirms that your feature both publishes and uses its cache.

   <img src="dk-buildCaches-singleConfDescription.png" width="706" alt="Build Cache feature description"/>

5. Run the build. If you correctly set up the feature, you should be able to see required files published as a ".teamcity.build_cache" [hidden artifact](build-artifact.md#Hidden+Artifacts) when the build finishes.

    <img src="dk-buildCaches-publishedArtifact.png" width="706" alt="Publish build cache"/>

6. To confirm that cache published in the previous step is used, run the build again and check the build log.
   
   <img src="dk-buildCaches-download.png" width="706" alt="Download build cache"/>


## Exchange Caches Between Separate Build Configurations

In this setup, the "publisher" Build Cache feature added to one build configuration publishes its cache, while the "consumer" Build Cache feature added to another configuration downloads this cache to the project's checkout directory.

<img src="dk-buildCache-split.png" width="706" alt="Reuse caches published by other configurations"/> 

You can set up as many publisher and consumer features as required as long as you add features for configurations that belong to the same project.

> One Build Cache feature can publish and use only one cache archive. If you need to utilize caches published by multiple build configurations, add the corresponding number of Build Cache features.
>
{type="tip"}

### Set Up a Publisher

1. [Add the build feature](adding-build-features.md) to a build configuration and specify a unique **Cache Name**.
2. Specify paths to files and folders that should be cached (currently, only paths relative to the checkout directory are supported). Each path should start from a new line. Wildcards are not supported.
3. Uncheck the **Use Cache** checkbox and save feature settings.

### Set Up a Consumer

1. [Add the build feature](adding-build-features.md) to a build configuration and specify the same **Cache Name** the publisher feature uses.
2. Uncheck the **Publish** checkbox and save feature settings.
3. Repeat the steps above for every build configuration that needs this published cache.

## Additional Information

### Build Sequence

If a build configuration is configured to upload caches, it arranges its tasks in the following order:

1. Download artifacts
2. Download caches
3. Checkout the project
4. Start the build

### Transferring Caches

Caches are automatically compressed into archives when published, and unpacked into the project's checkout directory when downloaded. You do not need to manually specify expressions (as you do with [regular artifacts](build-artifact.md)).


## See Also

* [Artifacts Cache on Agents](build-artifact.md#Artifacts+Cache+on+Agent)
* [Data Clean-Up](teamcity-data-clean-up.md)