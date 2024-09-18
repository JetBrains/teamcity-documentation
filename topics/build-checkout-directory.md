[//]: # (title: Build Checkout Directory)
[//]: # (auxiliary-id: Build Checkout Directory)

The _build checkout directory_ is a directory on the TeamCity agent machine where the build sources from a specific [VCS root](vcs-root.md) are checked out. This directory can be shared between multiple build configurations, if the build configurations are configured with the same VCS root. For more details, see [](#Default+Checkout+Directory) and [](#Custom+Checkout+Directory).

> For the complete Version Control Settings available in a TeamCity build configuration, see [](configuring-vcs-settings.md).
> 
{style="tip"}

## Checkout Process

The checkout process is affected by the [VCS Checkout Mode](vcs-checkout-mode.md), as follows:

* If you use the [agent-side checkout mode](vcs-checkout-mode.md#agent-checkout), the build agent checks out the sources into the build checkout directory before the build.
* If you use the [server-side checkout mode](vcs-checkout-mode.md#server-checkout), the TeamCity server sends incremental patches to the agent to update only the files changed since the last build in the given checkout directory.
* If you use the [manual checkout](vcs-checkout-mode.md#do-not-checkout-files-automatically) mode, no sources will be checked out, but the default build checkout directory will still be created to check out the sources using a build script. The directory will not be cleaned automatically unless its expiration period is configured as [described below](#Checkout+Directory+Expiration).

For checkout handled by TeamCity (the [server-side](vcs-checkout-mode.md#server-checkout) or [agent-side](vcs-checkout-mode.md#agent-checkout) checkout mode), TeamCity keeps track of the last revision checked out in each checkout directory on the agent and for the new build applies an incremental patch from the last used revision to the revision of the current build. The revisions used can be looked up on the __[Changes](build-results-page.md#Changes+Tab)__ tab of the build results page.

Incremental checkouts mean that any files not created or modified by TeamCity (for example, by the previous build scripts) are preserved in their modified state (unless dedicated VCS root-specific reset options are used). That is why it is recommended to:

* make sure the builds perform a clean procedure as the first step of the build for all the files that affect the build and might have been produced by previous builds. Typical files are compilation output, tests reports, build produce artifacts.
* make sure the builds never modify or delete the files under version control.

If TeamCity detects that it cannot build an incremental patch, a [clean checkout](clean-checkout.md) is enforced. It can also be enforced manually or configured to be performed on each build.

## Checkout Directory Location

### Default Checkout Directory

The default checkout directory location (when **Checkout directory** field is set to _Auto_ in the build settings [Version Control Settings](configuring-vcs-settings.md) page) is given by:

```Plain Text
<Build_Agent_Home>/work/<VCS_Settings_Hash_Code>
```

If the location of the [agent work directory](agent-work-directory.md) is customized, this changes to:

```Plain Text
<Agent_Work_Dir>/<VCS_Settings_Hash_Code>
```

The VCS settings hash code, `<VCS_Settings_Hash_Code>`, is calculated based on the set of VCS roots, their checkout rules and VCS settings used by the build configuration (checkout mode). Effectively, this means that the directory is shared between all the build configurations with the same VCS settings.

Source files are placed in the checkout directory according to the mapping defined in the [VCS Checkout Rules](vcs-checkout-rules.md).


<anchor name="BuildCheckoutDirectory-Customcheckoutdirectory"/>

<anchor name="Custom+checkout+directory"/>

### Custom Checkout Directory

To configure a custom checkout directory, set the **Checkout directory** field to _Custom path_ in the build settings [Version Control Settings](configuring-vcs-settings.md) page and enter the custom path in the field provided.

Make sure the following conditions are satisfied:
* the checkout directory is not shared between build configurations with different VCS settings (otherwise, TeamCity will perform [clean checkout](clean-checkout.md) each time another build configuration is built in the directory);
* the content of the directory is not modified by processes other than those of a single TeamCity agent (otherwise, TeamCity might be unable to ensure consistent incremental sources update). If this cannot be eliminated, make sure to turn on the clean build checkout option for all the participating build configurations. This rule also applies to two TeamCity agents sharing the same working directory. As one TeamCity agent has no knowledge of another, the other agent appears as an external process to it.

<warning>

Note that setting checkout directories as absolute paths that include agent home/working directories can lead to certain issues.

* If a custom checkout directory matches the agent work directory (its path is stored in the `teamcity.agent.work.dir` parameter), TeamCity will constantly wipe checkout directories of other configurations that use the same folder (for example, `<AGENT_WORK_DIR>/ProjectA`, `<AGENT_WORK_DIR>/ProjectB`, and so on). To avoid this, specify a custom checkout directory as a sub-folder of the agent working directory, not the agent working directory itself.

* When specifying sub-folders of an agent home/working directory, use either relative paths (`MyCustomFolder`) or absolute paths that substitute agent directories with [parameters](predefined-build-parameters.md) (`%\teamcity.agent.work.dir%/MyCustomFolder`). Otherwise, if the absolute path is a plain value (`/Users/John.Doe/TeamCityAgents/A1/work/MyCustomFolder`), TeamCity will continue using it even if you move the agent to another location.

</warning>

Note that the content of the checkout directory can be deleted by TeamCity under [certain circumstances](clean-checkout.md#Automatic+Clean+Checkout).

<anchor name="Automatic+Checkout+Directory+Cleaning"/>

### Finding a Checkout Directory

If you are investigating an issue and need to know the directory used by the build configuration, you can get the directory from the build log, or you can consult the generated file, `<[Agent_Work_Dir](agent-work-directory.md)>/directory.map`, which lists build configurations together with the directories they used last.

In your [build script](build-script-interaction-with-teamcity.md), you can refer to the effective value of the build checkout directory using the `teamcity.build.checkoutDir` [property](configuring-build-parameters.md) provided by TeamCity. By default, this is also the directory [where builds run](build-working-directory.md).


## Checkout Directory Expiration

With the [server-side](vcs-checkout-mode.md#server-checkout) and [agent-side checkout](vcs-checkout-mode.md#agent-checkout) modes, checkout directories are automatically deleted from the disk if not used (no builds were run on the agent using the directory as the checkout directory) for a specified period of time (8 days by default) or when another build requires more free disk space than available. With the [manual checkout](vcs-checkout-mode.md#do-not-checkout-files-automatically) mode, automatic directory cleaning is not performed unless the directory expiration period is configured.

It is recommended to use the [Free disk space](free-disk-space.md) build feature to ensure that the build gets enough disk free space on the build agent.

[//]: # (Internal note. Do not delete. "Build Checkout Directoryd30e211.txt")

The time frame for automatic directory expiration can be changed by specifying a new value (in hours) by either of the following ways:
* `teamcity.agent.build.checkoutDir.expireHours` agent property in the `buildAgent.properties` file
* `system.teamcity.build.checkoutDir.expireHours` [build configuration property](configuring-build-parameters.md)   
   * `0` will cause deleting the checkout directories right after the build finishes
   * `never` will let TeamCity know that the directory should never be deleted by TeamCity 
   * `default` will enforce using the default value

Expiration-based directory cleaning is performed in the background when the build agent is idle (no builds are running).

 <seealso>
        <category ref="admin-guide">
            <a href="configuring-vcs-settings.md">Configuring VCS Settings</a>
        </category>
</seealso>