[//]: # (title: Configuring VCS Triggers)
[//]: # (auxiliary-id: Configuring VCS Triggers)

VCS triggers automatically start a new build each time TeamCity detects new changes in the configured [VCS roots](vcs-root.md) and displays the change in the pending changes.

A new VCS trigger with the default settings triggers a build once there are pending changes in the build configuration: the version control is polled for changes according to the [checking for changes interval](configuring-vcs-roots.md#Common+VCS+Root+Properties) of a VCS root honoring a [VCS commit hook](configuring-vcs-post-commit-hooks-for-teamcity.md) if configured. Only the changes matched by the [checkout rules](vcs-checkout-rules.md) are displayed as pending and thus are processed by the trigger. If several check-ins are made within short time frame and discovered by TeamCity together, only __one build will be triggered__.

After the last change is detected, a [quiet period](#Quiet+Period+Settings) can be configured to wait for some time without changes before the build is queued.

The global default value for both options is 60 seconds and can be configured for the server on the __Administration | Global Settings__ page.

You can adjust a VCS trigger to your needs using the options described below:

<anchor name="ConfiguringVCSTriggers-Triggerabuildonchangesinsnapshotdependencies"/>

## Trigger a build on changes in snapshot dependencies

If you have a [build chain](build-chain.md) (i.e. a number of builds interconnected by [snapshot dependencies](dependent-build.md#Snapshot+Dependency)),  the triggers are to be configured in the final build in the chain. This is _pack setup_ in the image below.

<include src="build-dependencies-setup.md" include-id="trigger-on-ssdep-chngs"/>

If triggering rules are specified (described [below](#vcs-trigger-rules-1)), they are applied to all the changes (including changes from snapshot dependencies) and only the changes matching the rules trigger the build chain.

See also details at the [Build Dependencies](build-dependencies-setup.md) page.

## Per-check-in Triggering

When this option is __not__ enabled, several check-ins by different committers can be made; and once they are detected, TeamCity will add only one build to the queue with all of these changes.

If you have fast builds and enough build agents, you can make TeamCity launch a new build __for each check-in__ ensuring that no other changes get into the same build.    
To do that, select the __Trigger a build on each check-in__ option. If you select the __Include several check-ins in build if they are from the same committer__ option, and TeamCity will detect a number of pending changes, it will group them by user and start builds having single user changes only.

This helps to figure out whose change broke a build or caused a new test failure, should such issue arise.

<anchor name="quietPeriod"/>
<anchor name="ConfiguringVCSTriggers-quietPeriod"/>

### Quiet Period Settings

By specifying the quiet period you can ensure the build is not triggered in the middle of non-atomic check-ins consisting of several VCS check\-ins.

A __quiet period__ is a period (in seconds) that TeamCity maintains between the moment the last VCS change is detected and a build is added into the queue. If new VCS change is detected in the Build Configuration within the period, the period starts over from the new change detection time. The build is added into the queue only if there were no new VCS changes detected within the quiet period.

[//]: # (Internal note. Do not delete. "Configuring VCS Triggersd93e129.txt")

Note that the actual quiet period will not be less than the maximum [checking for changes interval](configuring-vcs-roots.md#Common+VCS+Root+Properties) among the VCS roots of a build configuration, as TeamCity must ensure that changes were collected at least once during the quiet period. 

The quiet period can be set to the default value (60 seconds, can be changed globally at the __Administration__ | __Global Settings__ page) or to a custom value for a build configuration.

<note>

Note that when a build is triggered by a trigger with the VCS quiet period set, the build is put into the queue with fixed VCS revisions. This ensures the build will be started with only the specific changes included. Under certain circumstances this build can later become a [History Build](history-build.md).
</note>

<include src="configuring-schedule-triggers.md" include-id="queue-optimization"/>

<anchor name="buildTriggerRules"/>

<anchor name="ConfiguringVCSTriggers-buildTriggerRules"/>

## VCS Trigger Rules
{id="vcs-trigger-rules-1"}

<chunk include-id="vcs-trigger-rules">

If no trigger rules are specified, a build is triggered upon any change detected for the build configuration. You can control what changes are detected by changing the VCS root settings and specifying [Checkout Rules](vcs-checkout-rules.md).

To limit the changes that trigger the build, use the VCS trigger rules. You can add these rules manually in the text area (one per line), or use the __Add new rule__ option to generate them.

<img src="addRule.png" width="450" alt="Adding trigger rule"/>

Each rule is ether an "include" (starts with `+`) or an "exclude" (starts with `-`).

</chunk>

### General Syntax

<chunk include-id="general-syntax">

The general syntax for a single rule is:


```Shell

+|-[:[user=VCS_username;][root=VCS_root_id;][comment=VCS_comment_regexp]]:Ant_like_wildcard

```

<include src="branch-filter.md" include-id="OR-syntax-tip"/>

where:
* `Ant_like_wildcard`: A [wildcard](wildcards.md) to match the changed file path. Only `*` and `**` patterns are supported, the `?` pattern is __not__ supported. The file paths in the rule can be relative (not started with `/` or `\`) to match resulting paths on the agent or absolute (started with `/`) to match VCS paths relative to a VCS root. For each file in a change the most specific rule is found (the rule matching the longest file path). The build is triggered if there is at least one file with a matching "include" rule or a file with no matching "exclude" rules.   
* [`VCS_username`](managing-users-and-user-groups.md): if specified, limits the rule only to the changes made by a user with the corresponding [VCS username](managing-users-and-user-groups.md#VCS+Usernames).
* `VCS_root_id`: if specified, limits the rule only to the changes from the corresponding VCS root.
* `VCS_comment_regexp`: if specified, limits the rule only to the changes that contain specified text in the VCS comment. Use the [Java Regular Expression](http://docs.oracle.com/javase/7/docs/api/java/util/regex/Pattern.html#sum) pattern for matching the text in a comment (see examples below). The rule matches if the comment text contains a matched text portion; to match the entire text, include the `^` and `$` special characters.

<tip>

When specifying the rules, note that as soon as you enter any `+` rule, TeamCity will change the implicit default from "include all" to "exclude all".    
To include all the files, use `+:.` rule.

Also, rules are sorted according to path specificity. If you have an explicit inclusion rule for `/some/path`, and exclusion rule `-:user=some_user:.` for all paths, commits to the `/some/path` from `some_user` will be __included__ unless you add a specific exclusion rule for this user and this path at once, like `-:user=some_user:/some/path/**`
</tip>

</chunk>

#### Trigger Rules Examples
{id="trigger-rules-examples-1"}

<chunk include-id="trigger-rules-examples">

<table>
<tr>
 
<td width="200">
 
Example

</td>
 
<td>
 
Description

</td></tr><tr>
 
<td>
 
+:.
 
</td>
 
<td>
 
Includes all files
 
</td></tr><tr>
 
<td>
 
-:\*\*.html
 
</td>
 
<td>
 
Excludes all `.html` files from triggering a build.
 
</td></tr>
 
<tr>
 
<td>
 
-:user=techwriter;root=InternalSVN:/misc/doc/\*.xml 
 
</td>
 
<td>
 
Excludes builds being triggered by `.xml` files checked in by the [VCS user](managing-users-and-user-groups.md#VCS+Usernames) "techwriter" to the `misc/doc` directory of the VCS root named _Internal SVN_ (as defined in the VCS Settings). Note that the path is absolute (starts with "/"), thus the file path is matched from the VCS root.
 
</td></tr>
 
<tr>
 
<td>
 
-:lib/\*\*
 
 </td>
 <td>
 
Prevents the build from triggering by updates to the `lib` directory of the build sources (as it appears on the agent). Note that the path is relative, so all files placed into the directory (by processing VCS root [checkout rules](vcs-checkout-rules.md)) will not cause the build to be triggered.

</td></tr><tr>
 
<td>
 
-:comment=minor:\*\* 

</td>
<td>
 
Prevents the build from triggering, if the changes check contains the word "minor" in the comment.

</td></tr><tr>
<td>
 
-:comment=^oops$:\*\*

</td>
<td>
 
No triggering if the comment consists of the word "oops" only (according to [Java Regular Expression](http://docs.oracle.com/javase/7/docs/api/java/util/regex/Pattern.html#sum) principles `^` and `$` in pattern stand for string beginning and ending).

</td></tr>
</table>

</chunk>

### Branch Filter
{id="branch-filter-1"}

Read more in [Branch Filter](branch-filter.md).

### Trigger Rules and Branch Filter Combined
{id="trigger-rules-and-branch-filter-combined-1"}

Trigger rules and branch filter are combined by __AND__, which means that the build is triggered __only when both conditions are satisfied__.

For example, if you specify a comment text in the trigger rules field and provide the branch specification, the build will be triggered only if a commit has the special text and is also in a branch matched by branch filter.

### Triggering a Build on Branch Merge

The VCS trigger is fully aware of branches and will trigger a build once a check-in is detected in a branch.

When changes are merged / fast-forwarded from one branch to another, strictly speaking there are no actual changes in the code. By default, the VCS trigger behaves in the following way:
* When merging/fast forwarding of two non-default branches: the changes in a build are calculated with regard to previous builds in the same branch, so if there is a build on same commit in a different branch, the trigger will start a build in another branch pointing to the same commit.
*  If the default branch is one of the branches in the merging/fast-forwarding, the changes are always calculated against the default branch, if there is a build on same revision in the default branch, TeamCity will not run a new build on the same revision.
