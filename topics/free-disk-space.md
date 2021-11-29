[//]: # (title: Free Disk Space)
[//]: # (auxiliary-id: Free Disk Space;Free disk Space)

>This page is about the _Free disk space_ build feature. If you want to learn how to automatically clean up the TeamCity data, see [this section](teamcity-data-clean-up.md). To learn where and how TeamCity stores its configuration settings, see [this section](teamcity-data-directory.md).

The _Free disk space_ [build feature](adding-build-features.md) allows ensuring certain free disk space __on the agent__ before the build by deleting files managed by the TeamCity agent (other build's checkout directories and various caches).   
When the feature is not configured, the default free space for a build is 3 GB.

## Free Disk Space Requirements

Before the build and before each build preparation stage, the agent will check the currently available free disk space in three locations: the agent's system, the agent's temp directory, and the build checkout directory. All these locations have to meet the same specified requirement. If the failure condition is specified, the build will fail if either of the locations does not meet the requirement.

If the amount is less than required, the agent will try to delete the data of other builds before proceeding.

The data cleaned includes:
* the checkout directories that were marked for [deletion](build-checkout-directory.md#Automatic+Checkout+Directory+Cleaning)
* contents of other build's checkout directories in the reversed most recently used order
* the cache of previously downloaded artifacts (that were downloaded to the agent via TeamCity artifact dependencies)
* cleaning the local [Docker caches](integrating-teamcity-with-docker.md#Docker+Disk+Space+Cleaner) 
* cleaning the local [NuGet packages caches](nuget.md#NuGet+Packages+Cache+Clean-up+on+Agents)

If you need to make sure a checkout directory is never deleted while freeing disk space, set the `system.teamcity.build.checkoutDir.expireHours` property to `never`.

## Settings

You can use the Free Disk Space [build feature](adding-build-features.md) to alter the default 3 GB of required disk space. Configure the settings below:

<table><tr>

<td>

Setting

</td>

<td>

Description

</td></tr><tr>

<td>

Required free space

</td>

<td>

You can specify a custom free disk space value here (in bytes or using one of the kb, mb, gb or tb suffixes).

</td></tr><tr>

<td>

Fail build if sufficient disk space cannot be freed

</td>

<td>

Enable to add the corresponding [build failure condition](build-failure-conditions.md).

</td></tr></table>

## Alternative Methods to Set Free Disk Space Value

To ensure compatibility, the free disk space value can be specified via the properties below. However, using the Free Disk Space build feature is recommended as these properties can be removed in the future TeamCity versions.

The properties can be defined:
* globally for a build agent (in the agent's [`buildAgent.properties`](configure-agent-installation.md) file)
* for a particular build configuration by specifying its system properties.

The required free space value is defined with the following properties:
* `system.teamcity.agent.ensure.free.space` for the build checkout directory
* `system.teamcity.agent.ensure.free.temp.space` for the agent's `temp` directory

If `teamcity.agent.ensure.free.temp.space` is not defined, the value of the `teamcity.agent.ensure.free.space` property is used.

The values of these properties specify the amount of the available free disk space to be ensured before the build starts. The value should be a number followed by kb, mb, gb, kib, mib, or gib suffix. Use no suffix for bytes.   
Example: `system.teamcity.agent.ensure.free.space = 5gb`

Here is how TeamCity will choose a free disk space value:
1. Use `system.teamcity.agent.ensure.free.space`, defined on the agent or overridden on the project or build configuration level.
2. If (1) is not defined, use `teamcity.agent.ensure.free.space`, defined on the agent or overridden on the project or build configuration level.
3. If (1-2) are not defined, use the custom value defined in the build feature.
4. If no custom values are defined, use the default value of 3 GB.

### Configuring Artifacts Cache

A TeamCity build agent maintains a cache of published and downloaded build artifacts to reduce network transfers to the same agent. The cache is stored in the `<[Build Agent Home](agent-home-directory.md)>\system\.artifacts_cache` directory and is cleaned automatically provided the _Free disk space_ build feature is configured correctly.

If caching artifacts is undesirable (for example, when the artifacts are large and not used within TeamCity, or if the artifacts cache directory is located not on the same disk as the build checkout directory, or if the builds do not define the _Free disk space_ build feature and the default 3Gb is not sufficient for a build), caching artifacts on the agent can be __turned off__ by adding the `teamcity.agent.filecache.publishing.disabled=true` configuration parameter to a project or one of the build configurations of a project. However, the agent will still cache artifacts downloaded as artifact dependencies.

[//]: # (Internal note. Do not delete. "Free disk spaced145e166.txt")

 <seealso>
        <category ref="admin-guide">
            <a href="teamcity-disk-space-watcher.md" product="tc">TeamCity Server Disk Space Watcher</a>
            <a href="build-failure-conditions.md">Build Failure Conditions</a>
        </category>
</seealso>
