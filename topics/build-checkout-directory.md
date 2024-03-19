[//]: # (title: Build Checkout Directory)
[//]: # (auxiliary-id: Build Checkout Directory)

The _build checkout directory_ is a directory on the TeamCity agent machine where all the sources of all builds are checked out into.
* If you use the [agent-side checkout mode](vcs-checkout-mode.md#agent-checkout), the build agent checks out the sources into this directory before the build.
* In case you use the [server-side checkout mode](vcs-checkout-mode.md#server-checkout), the TeamCity server sends incremental patches to the agent to update only the files changed since the last build in the given checkout directory.
* With the [manual checkout](vcs-checkout-mode.md#do-not-checkout-files-automatically) mode, no sources will be checked out, but the default build checkout directory will still be created to check out the sources via a build script. The directory will not be cleaned automatically unless its expiration period is configured as [described below](#Automatic+Checkout+Directory+Cleaning).

The sources are placed into the checkout directory according to the mapping defined in the [VCS Checkout Rules](vcs-checkout-rules.md).

The checkout directory is configured in the __Checkout Settings__ section on the [Version Control Settings](configuring-vcs-triggers.md) page; the default __Auto (recommended)__ value is strongly advised, but it is possible to configure a custom checkout directory as described [below](#Custom+checkout+directory).

If you want to investigate an issue and need to know the directory used by the build configuration, you can get the directory from the build log, or you can refer to the `<[Agent Work Directory](agent-work-directory.md)>/directory.map` generated file which lists build configurations with the directories they used last.

In your [build script](build-script-interaction-with-teamcity.md), you can refer to the effective value of the build checkout directory via the `teamcity.build.checkoutDir` [property](configuring-build-parameters.md) provided by TeamCity.

By default, this is also the directory [where builds will run](build-working-directory.md).

## Checkout Process

For checkout handled by TeamCity (the [server-side](vcs-checkout-mode.md#server-checkout) or [agent-side](vcs-checkout-mode.md#agent-checkout) checkout mode), TeamCity keeps track of the last revision checked out into each checkout directory on the agent and for the new build applies an incremental patch from the last used revision to the revision of the current build.   
The revisions used can be looked up on the __[Changes](build-results-page.md#Changes+Tab)__ tab of the build results page.

Incremental checkouts mean that any files not created or modified by TeamCity (for example, by the previous build scripts) are preserved in their modified state (unless dedicated VCS root-specific reset options are used).   
That is why it is recommended to:
* make sure the builds perform a clean procedure as the first step of the build for all the files that affect the build and might have been produced by previous builds. Typical files are compilation output, tests reports, build produce artifacts.
* make sure the builds never modify or delete the files under version control.

If TeamCity detects that it cannot build an incremental patch, a [clean checkout](clean-checkout.md) is enforced. It can also be enforced manually or configured to be performed on each build.

<anchor name="BuildCheckoutDirectory-Customcheckoutdirectory"/>

## Custom checkout directory

In most cases, the default __Auto (recommended)__ setting will cover your needs. With this default checkout directory, TeamCity ensures the best performance and consistent incremental sources updates. The name of the default automatically created directory is generated as follows: `<[Agent Work Directory](agent-work-directory.md)>/<VCS settings hash code>`. The VCS settings hash code is calculated based on the set of VCS roots, their checkout rules and VCS settings used by the build configuration (checkout mode). Effectively, this means that the directory is shared between all the build configurations with the same VCS settings.

If for some reason you need to specify a custom checkout directory (for example, the process of creating builds depends on some particular directory), make sure the following conditions are met:
* the checkout directory is not shared between build configurations with different VCS settings (otherwise, TeamCity will perform [clean checkout](clean-checkout.md) each time another build configuration is built in the directory);
* the content of the directory is not modified by processes other than those of a single TeamCity agent (otherwise, TeamCity might be unable to ensure consistent incremental sources update). If this cannot be eliminated, make sure to turn on the clean build checkout option for all the participating build configurations. This rule also applies to two TeamCity agents sharing the same working directory. As one TeamCity agent has no knowledge of another, the other agent appears as an external process to it.

<warning>

It is recommended that a custom checkout directory __does not contain__ the [agent's working directory](build-working-directory.md) because of the potential undesirable side effects.
</warning>

Note that the content of the checkout directory can be deleted by TeamCity under [certain circumstances](clean-checkout.md#Automatic+Clean+Checkout).

## Automatic Checkout Directory Cleaning

With the [server-side](vcs-checkout-mode.md#server-checkout) and [agent-side checkout](vcs-checkout-mode.md#agent-checkout) modes, checkout directories are automatically deleted from the disk if not used (no builds were run on the agent using the directory as the checkout directory) for a specified period of time (8 days by default) or when another build requires more free disk space than available. With the [manual checkout](vcs-checkout-mode.md#do-not-checkout-files-automatically) mode, automatic directory cleaning is not performed unless the directory expiration period is configured.

It is recommended to use the [Free disk space](free-disk-space.md) build feature to ensure that the build gets enough disk free space on the build agent.

[//]: # (Internal note. Do not delete. "Build Checkout Directoryd30e211.txt")

The time frame for automatic directory expiration can be changed by specifying a new value (in hours) by either of the following ways:
* `teamcity.agent.build.checkoutDir.expireHours` agent property in the `buildAgent.properties` file
* `system.teamcity.build.checkoutDir.expireHours` [build configuration property](configuring-build-parameters.md)   
   * `0` will cause deleting the checkout directories right after the build finishes
   * `never` will let TeamCity know that the directory should never be deleted by TeamCity 
   * `default` will enforce using the default value

_Expiration-based directory cleaning_ is performed in the background when build agent is idle (i.e. no builds are running).

Bear in mind that once the parameter is set, the change will only apply to all _newly executed builds_. For older checkout directories, where no new builds were executed after specifying the parameter, the _expiration_ value will remain as it was before the change.

 <seealso>
        <category ref="admin-guide">
            <a href="configuring-vcs-settings.md">Configuring VCS Settings</a>
        </category>
</seealso>
