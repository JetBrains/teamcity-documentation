[//]: # (title: Configuring Finish Build Trigger)
[//]: # (auxiliary-id: Configuring Finish Build Trigger)

The _finish build trigger_ starts a build of the current build configuration when a build of the selected build configuration is finished. If the "_Trigger after successful build only_" checkbox is enabled, a build is triggered only after a successful build of the selected configuration.

To monitor builds in other build configurations and trigger a build if these builds change, please see [this option](configuring-schedule-triggers.md#Build+Changes) of the schedule build trigger.

In most of the cases, the finish build trigger should be used with snapshot dependencies, i.e. the current build configuration where the trigger is defined should have a direct or an indirect snapshot dependency on the build configuration selected in the trigger. If there is no snapshot dependency, the following limitations exist:
* It is likely that a build of the build configuration being triggered will not have the same revisions as the finished build even if both configurations have the same VCS settings.
* If a build configuration with the finish build trigger has an artifact dependency on the last finished build of the build configuration specified in the trigger settings, there is no guarantee that artifacts of a build which caused build triggering will be used, because, while the triggered build sits in the build queue, another build may finish.
* The build triggered by the finish build trigger will always be triggered in the default branch even if the finished build has some other branch.

All these limitations __do not apply__ if a build configuration with the finish build trigger has a snapshot dependency on the selected build configuration. In this case, the trigger will run build on the same revisions and will attach the build to the chain. It will also use consistent artifacts if they are produced by dependencies.

### Branch Filter

In a build configuration with branches, you can use the [branch filter](branch-filter.md) to limit the branches in which finished builds will trigger new builds of the current configuration.

__ __