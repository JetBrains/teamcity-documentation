[//]: # (title: Branch Filter)
[//]: # (auxiliary-id: Branch Filter)

If a VCS Root has branches configured, you can use the _Branch Filter_ option.

On this page:

<tag-list of="chapter" mode="tree" depth="4"/>

## Branch Filter Usage

The Branch Filter allows limiting branches used by TeamCity in various operations. Currently, the Branch Filter is available on the following TeamCity settings pages:

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
 
[Version Control Settings of a build configuration](configuring-vcs-settings.md)


</td>

<td>

Limit the set of branches available for the build configuration. This branch filter is applied before any other branch filter and limits branches shown in the custom build dialog, as well as branches visible to triggers and build features.

</td>

</tr>

<tr>

<td>

[Finish Build Triggers](configuring-finish-build-trigger.md)

</td>

<td>

 
Limit the set of branches to which builds the trigger should be applied to.

</td>

</tr>

<tr>

<td>

[VCS Triggers](configuring-vcs-triggers.md)

</td>

<td>

Limit the set of branches the trigger should be applied to.

</td>

</tr>

<tr>

<td>

[Schedule Triggers](configuring-schedule-triggers.md)

</td>

<td>

Limit the set of branches the trigger should be applied to.

* If "_Trigger build only if there are pending changes_" is turned ON, then the trigger will add a build to the queue for all branches matched by the trigger branch filter where pending changes exist
* If "_Trigger build only if there are pending changes_" is turned OFF, then the trigger will add a build to the queue for all branches matched by the trigger branch filter regardless of the presence of pending changes there.


</td>

</tr>

<tr>

<td>

[‘VCS labeling’ build feature](vcs-labeling.md)

</td>

<td>

Limit the set of branches to which builds the labels will be applied.

</td>

</tr>

<tr>

<td>

[‘Automatic merge’ build feature](automatic-merge.md#Automatic+Merge+Settings)

</td>

<td>

Limit the set of branches branches which builds’ sources will be merged.

</td>

</tr>

<tr>

<td>

[Notification rules](subscribing-to-notifications.md)

</td>

<td>

Set a filter to receive alerts only on the builds from the specified branches. By default, only the default branch is monitored.

</td>

</tr>

</table>

## Branch Filter Format

To filter branches, use a newline-delimited list of `+|-:logical_branch_name` rules, where `logical_branch_name` is the name displayed in the TeamCity UI (for example, `master`).

<chunk include-id="OR-syntax-tip">

<tip>

Here, the _pipe_ symbol `|` represents the __OR__ command, as in regular expressions: use `+` for including, __OR__ `-` for excluding.

</tip>

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
__ __