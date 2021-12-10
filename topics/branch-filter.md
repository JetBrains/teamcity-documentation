[//]: # (title: Branch Filter)
[//]: # (auxiliary-id: Branch Filter)

If a [VCS root](vcs-root.md) has [branches specified](working-with-feature-branches.md#Configuring+branches), the _branch filter_ option becomes available for various operations in TeamCity.

## Branch Filter Usage

Currently, branch filters can be configured on the following TeamCity settings pages:

<table>

<tr>

<td>

Settings

</td>

<td>

Branch filter description

</td>

</tr>

<tr>

<td>
 
[Version Control Settings](configuring-vcs-settings.md) of a build configuration

</td>

<td>

Limit the set of branches available for the build configuration. This branch filter is applied before any other branch filter and limits branches shown in the custom build dialog, as well as branches visible to triggers and build features.

</td>

</tr>

<tr>

<td>
 
[Artifact dependency](artifact-dependencies.md)

</td>

<td>

Limit dependency builds, whose artifacts will be used in the builds of the current configuration, to those in the matching branches.

</td>

</tr>

<tr>

<td>

[Finish build triggers](configuring-finish-build-trigger.md)

</td>

<td>

Limit the set of branches where builds will be monitored by this finish build trigger.

</td>

</tr>

<tr>

<td>

[VCS triggers](configuring-vcs-triggers.md)

</td>

<td>

Limit the set of branches in which builds can be triggered by this VCS trigger.

</td>

</tr>

<tr>

<td>

[Schedule triggers](configuring-schedule-triggers.md)

</td>

<td>

Limit the set of branches the trigger should be applied to.

* If "_Trigger build only if there are pending changes_" is turned ON, then the trigger will add a build to the queue for all matching branches where pending changes exist.
* If "_Trigger build only if there are pending changes_" is turned OFF, then the trigger will add a build to the queue for all matching branches regardless of the presence of pending changes there.

</td>

</tr>

<tr>

<td>

[Retry build triggers](configuring-retry-build-trigger.md)

</td>

<td>

Set a branch filter to rerun failed builds only in branches that match the specified criteria.

</td>

</tr>

<tr>

<td>

[VCS labeling](vcs-labeling.md) build feature

</td>

<td>

Limit the set of branches to which builds the labels will be applied.

</td>

</tr>

<tr>

<td>

[Automatic merge](automatic-merge.md#Automatic+Merge+Settings) build feature

</td>

<td>

Limit the set of branches which builds’ sources will be merged.

</td>

</tr>

<tr>

<td>

[Notification rules](adding-notification-rules.md) and the [Notifications](notifications.md) build feature

</td>

<td>

Set a filter to receive alerts only on the builds from the specified branches. By default, only the default branch is monitored.

</td>

</tr>

<tr>

<td>

[Pull Requests](pull-requests.md) build feature

</td>

<td>

Specify on which branches to monitor and trigger pull requests.

</td>

</tr>

<tr>

<td>

[Keep rules for clean-up](teamcity-data-clean-up.md#Keep+Rule)

</td>

<td>

Specify a naming pattern for branches to which the clean-up rule will apply. Note that, depending on the "_Apply rule_" settings, it could either apply to a selected number of builds per each matching branch or to a selected number of builds once per set of matching branches.

</td>

</tr>

</table>

If there are multiple branch filters configured atop a single root, the following order of priority is applied:
1. The [branch specification](working-with-feature-branches.md#Configuring+branches) in the VCS root settings defines the initial set of the monitored branches.
2. If specified, a branch filter in the __Version Control Settings__ of a build configuration can narrow down the initial set of branches.
3. If specified, a branch filter in the settings of a build trigger applies to the subset declared by the filter (2). 

## Branch Filter Format

To filter branches, use a newline-delimited list of `+|-:logical_branch_name` rules, where `logical_branch_name` is the name displayed in the TeamCity UI (for example, `master`). The name is case-sensitive.

<chunk include-id="OR-syntax-tip">

>Here, the _pipe_ symbol `|` represents the __OR__ command, as in regular expressions: use `+` for including, __OR__ `-` for excluding.

</chunk>
 
The `+:` rules include the matching branches into the list of accepted branches, the `-:` rules exclude the branches from the list.

Each rule can have one optional wildcard `*` placeholder that matches one or more characters: `+|-:name*` will match the branch `name1` but __will not__ match the branch `name`, which will need to be added explicitly.

You can use parameter references in the branch filter.

When a single branch is matched by several lines of the branch filter, the most specific (least characters matched by a pattern) last rule applies. That is, if the filter contains an exact pattern matching the branch (i.e. a pattern without the `*` wildcard), then the last such pattern is used.

Other examples:

* Only the default branch is accepted:   
   ```Plain Text  
   +:<default>

   ```

* All branches except the default one are accepted:   
   ```Plain Text  
   +:*
   -:<default>

   ```
   
* Only branches with the `feature-` prefix are accepted:   
   ```Plain Text   
   +:feature-*

   ```
   
   
* Empty branch filter (all branches are accepted):   
   ```Plain Text   
   +:*

   ```
