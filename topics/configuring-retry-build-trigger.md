[//]: # (title: Configuring Retry Build Triggers)
[//]: # (auxiliary-id: Configuring Retry Build Triggers)

The _retry build trigger_ automatically adds a new build to the queue if the previous build of the current build configuration has failed.

> To configure the _retry build trigger_ in Kotlin DSL, see [RetryBuildTrigger](https://www.jetbrains.com/help/teamcity/kotlin-dsl-documentation/triggers/retry-build-trigger/index.html).
>
{style="note"}

## Triggering Settings

The following settings are available for the retry build trigger:

<table>

<tr>

<td width="200">Setting</td>

<td>Description</td>

</tr>

<tr>

<td>Seconds to wait</td>

<td>Specify seconds to wait before adding a new build to the queue.</td>

</tr>

<tr>

<td>Number of attempts to retry the build</td>

<td>Specify how many times the trigger will try to rerun the failing build. Leave blank to retry unlimited times until the build is successful.</td>

</tr>

<tr>

<td>Trigger a new build with the same revisions</td>

<td>

With this option enabled, the retry trigger will rerun a failed build using the same source revisions.

This option helps identify build problems that do not depend on the build code: for example, if there are flaky tests in the build configuration or if there was some unforeseen agent compatibility issue.   
If the build with the trigger is a part of a build chain, all the successful builds from the previous chain run will be reused and all the failed dependency builds, that could have contributed to the failure of the dependent build, will be rebuilt on the same revision.   
If any build parameters or comments are specified in the custom build settings, they will be applied to the following build runs initiated by the retry trigger.
    
</td>

</tr>

<tr>

<td>Put the newly triggered builds to the queue top</td>

<td>

With this option enabled, retried builds will always be put to the queue top.

</td>

</tr>

<tr>

<td>Branch filter</td>

<td>

Apply a [branch filter](branch-filter.md) to rerun failed builds only in branches that match the specified criteria.

</td>

</tr>

</table>

## Triggered Build Customization

<include from="configuring-vcs-triggers.md" element-id="triggered-build-customization"/>
