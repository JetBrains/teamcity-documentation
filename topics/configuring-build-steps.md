[//]: # (title: Configuring Build Steps)
[//]: # (auxiliary-id: Configuring Build Steps)

When creating a build configuration, it is important to configure the sequence of _build steps_ to be executed.

Build steps are configured in the __Build Steps__ section of the [__Build Configuration Settings__](creating-and-editing-build-configurations.md) page. Here, you can:
* add new steps manually;
* autodetect steps by scanning the source VCS repository;
* copy and delete steps;
* temporarily enable/disable steps.

Each build step is represented by a [build runner](build-runner.md) and provides integration with a specific build or test tool. For example, call a NAnt script before compiling VS solutions. You can add as many build steps to your build configuration as needed.

Build steps are invoked sequentially.

The decision whether to run the next build step may depend on the exit status of the previous build steps, the current build status, or [execution conditions](build-step-conditions.md).

The build step is considered _failed_ if (1) the build process returned a non-zero exit code and (2) the "_Fail build if build process exit code is not zero_" build failure condition is enabled (see [Build Failure Conditions](build-failure-conditions.md)); otherwise, the build step is considered _successful_.

Note that the status of a build step and the build itself can be different. All build steps can be successful, but the build can fail because of another build failure condition – not based on the exit code (like failing a test). On the other hand, if a build step has failed, the build will fail too.

## Execution policy

You can specify the step execution policy via the __Execute step__ option:
* __Only if build status is successful__: before starting the step, the build agent requests the build status from the server, and skips the step if the status is failed. This considers the failure conditions processed by the server, like failure on test failures or on metric change. Note that this still can be not exact as some failure conditions are processed on the server asynchronously ([TW-17015](https://youtrack.jetbrains.com/issue/TW-17015))
* __If all previous steps finished successfully__: the build analyzes only the build step status on the build agent, and doesn't send a request to the server to check the build status and considers only important step failures.
* __Even if some of the previous steps failed__: select to make TeamCity execute this step regardless of the status of previous steps and status of the build.
* __Always, even if build stop command was issued__: select to ensure this step is always executed, even if the build was canceled by a user. For example, if you have two steps with this option configured, stopping the build during the first step execution will interrupt this step, while the second step will still run. Issuing the stop command for the second time will result in ignoring the execution policy: the build will be terminated.

<tip>

__Tips:__

* You can copy a build step from one build configuration to another from the original build configuration settings page.
* You can reorder build steps as needed. Note, that if you have a build configuration inherited from a template, you cannot reorder inherited build steps. However, you can insert custom build steps (not inherited) at any place and in any order, even before or between inherited build steps. Inherited build steps can be reordered in the original template only.
* You can disable a build step temporarily or permanently, even if it is inherited from a build configuration template, using the corresponding option in the last column of the __Build Steps__ list.

</tip>

<anchor name=""/>

Since TeamCity 2020.1, you can also add granular [execution conditions](build-step-conditions.md) for build steps.

For the details on configuring individual build steps, refer to the respective pages inside this section.

 <seealso>
        <category ref="concepts">
            <a href="build-runner.md">Build Runner</a>
        </category>
</seealso>