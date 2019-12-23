[//]: # (title: Working with Feature Branches)
[//]: # (auxiliary-id: Working with Feature Branches)

Feature Branches in distributed version control systems (DVCS) allow you to work on a feature independently from the main development and commit all the changes for the feature onto the branch, merging the changes into the main branch when your feature is complete. This approach brings a number of advantages to software development teams; however, in continuous integration servers that do not have dedicated support for it, it also causes a number of problems, like constant build configurations duplication, poor visibility, and, in the end, loss of control over the process.

TeamCity support for feature branches is continuously increasing and, among other features, includes [Branch Remote Run Trigger](branch-remote-run-trigger.md) starting a new personal build each time TeamCity detects changes in a particular branches of the VCS roots of the build configuration and [Automatic Merge](automatic-merge.md) to merge a branch into another after a successful build.

On this page:

<tag-list of="chapter" mode="tree" depth="4"/>


## Supported version control systems

[Git](git.md) and [Mercurial](mercurial.md) feature branches are supported as well as Perforce [branch streams support](perforce.md#branch-support).

<anchor name="branchSpec"/>

## Configuring branches

To start working with DVCS branches, you need to configure which branches need to be watched for changes. This is done in the __General Settings__ section of a [Git](git.md) or [Mercurial](mercurial.md) VCS root via the __Branch Specification__ field.    
With Perforce, check the corresponding box to enable feature branches support, which will display the branch specification field. The field accepts a list of branch names or patterns. TeamCity monitors the branches matched by the branch specification in addition to the [default branch](#Default+branch).

Once you've configured the branch specification, TeamCity will start to monitor these branches for changes. If your build configuration has [a VCS trigger and a change is found in some branch](configuring-vcs-triggers.md#Branch+Filter), TeamCity will trigger a build in this branch. From the build configuration home page you'll also be able to filter the history, change log, pending changes and issue log by the branch name. Branch names will also appear in the custom build dialog, so you'll be able to manually trigger a custom build on a branch too.

The syntax of the branch specification field is newline-delimited list of `+|-:branch_name` rules.       
<include src="branch-filter.md" include-id="OR-syntax-tip"/>

Each rule can have one optional `*` placeholder which matches one or more characters.    
For example, `+:refs/heads/teamcity*` matches all branches starting with `refs/heads/teamcity` __and at least one additional character__.   
The branch with `refs/heads/teamcity` will not be matched. 
  
The `branch_name` parameter is VCS-specific, i.e. `refs/heads/master` in Git: ![branchSpec.png](branchSpec.png) 

The part of the branch name matched by the asterisk (`*`) wildcard becomes the short branch name to be displayed in the TeamCity user-level interface (also known as the [logical branch name](#Logical+branch+name)). The line can also contain optional parentheses which, when present, denote the part of the pattern to be used as the logical name instead of just *-matched symbols.

You can use parameters in the branch specification.

When a single VCS branch is matched by several lines of the branch specification, the most specific (least characters matched by pattern) last rule applies.

That is, if the specification contains an exact pattern matching the branch (i.e. a pattern without the `*` wildcard), then the last such pattern is used. So if you have a specification like this:


```Plain Text
+:refs/heads/release-v1
-:refs/heads/release-v1
```

then the last pattern will win and the branch will be excluded.
    
If a branch specification has several patterns with the `*` wildcard, then TeamCity selects the pattern producing the shortest logical name. This branch specification:


```Plain Text
+:refs/heads/*/hotfix
-:refs/heads/v1/*
```



will include the `refs/heads/v1/hotfix` branch (because "`v1`" is shorter than "`hotfix`").    
If 2 patterns with `*` wildcard produce logical names of the same length, then the last pattern wins.

The branch specification supports comments as lines beginning with "`#"`.

There is also a special escaping syntax defined via `#! escape: CHARACTER` syntax: for example, to use round brackets in a branch name, you need to escape them. Let's say you want to track the `release-(7.1)` branch: to do that, specify an escaping symbol as the first line in the specification. For Mercurial, the following branch specification does that:


```Plain Text
#! escape: \
+:release-\(7.1\)
```

<tip>

To run builds on GitHub pull request branches, use the [Pull Requests](pull-requests.md) build feature.
</tip>

## Default branch

When configuring a VCS root for DVCS, you need to specify the branch name to be used as the default one. The default branch has special meaning:
* It is a fallback branch to use when the branch is not specified or the specified branch is not included by the branch specification (for example, when someone just clicks on the __Run__ button without selecting a branch).
* It can be used when displaying sequences of builds and changes and reaching the moment of the branch creation.
* The default branch allows using different branches in different VCS roots (for example, if one of the roots is Git and another is Mercurial) and in different builds when they are linked by a snapshot dependency. When the top chain build is triggered in the default branch, all its dependencies will be built in their respective default branches as well.

Unless disabled via a [branch filter](branch-filter.md), the default branch is always implicitly included into the branch specification. In the TeamCity UI the default branch is marked with the darker background of the branch marker.

## My branches

TeamCity can identify and group branches, based on the commits of the current TeamCity user.

Select the _My Branches_ group in the branch filter to display all active branches whose last 100 changes include your commits, based on the defined VSC usernames.

 <anchor name="logicalBranchName"/>

## Logical branch name

A logical branch name is a branch name shown in the user interface for the builds and on build configuration level. A logical branch name is regularly a part of the full VCS\-specific branch name. It is calculated by applying a [branch specification](#Configuring+branches) to the branch name from the version control.

For example, if the branch specification is defined like this:


```Plain Text
+:refs/heads/*
```



then the part matched by `*` (for example, `master`) is a logical branch name. 

If the branch specification pattern uses parentheses, the logical name then is made up of the part of the name within the parentheses; to see the `v8.1/feature1` logical name displayed in the UI for the VCS branch `refs/heads/v8.1/feature1`, use this:


```Plain Text
+:refs/heads/(v8.1/*)
```



You do not need to include the default branch into the branch specification as it is already included there implicitly. But, if you want to have some short logical branch name for the default branch in the UI, for example, `master`, you can include it in the branch specification and use the parentheses:


```Plain Text
+:refs/heads/(master)
```



## Builds

Builds from branches are easily recognizable in the TeamCity UI, because they are marked with a special label:

![branches.png](branches.png)

You can also filter history by a branch name if you're interested in a particular branch. TeamCity assigns a branch label to the builds from the default branch too.

## Changes

For each build TeamCity shows changes included in it. For builds from branches the changes calculation process takes the branch into account and presents you with the changes relevant to the build branch. The changes for a build in a branch are calculated as the changes from the build's revision to the previous build in the branch or a build in the default branch.   
The change log with its graph of commits will help you understand what is going on in the monitored branches.

![branchesChange.png](branchesChange.png)

With the __Show graph__ option enabled by default TeamCity displays build markers on the graph.

  <anchor name="ActiveBranches"/>

## Active branches

In a build configuration with configured branches, most UI pages show active branches by default.

An _active branch_ is a branch with the recent activity: it has recent builds or exists in the repository with recent commits.    

The threshold for activity can be configured via build configuration parameters. The parameters can be changed either in a build configuration (this will affect one build configuration only), project, or in the [internal properties](configuring-teamcity-server-startup-properties.md#TeamCity+internal+properties) (this defines defaults for the entire server). A parameter in the configuration overrides a parameter in the [internal properties](configuring-teamcity-server-startup-properties.md#TeamCity+internal+properties).

A branch is considered active if:
* it is present in the VCS repository and has recent commits (i.e. commits with the age less than the value of the `teamcity.activeVcsBranch.age.days` parameter, 7 days by default).
* or it has recent builds (i.e. builds with the age less than the value of `teamcity.activeBuildBranch.age.hours` parameter, 24 hours by default).   
A closed VCS branch with builds will still be displayed as active during 24 hours after last build. To remove closed branches from display, set `teamcity.activeBuildBranch.age.hours=0`.

## Tests

TeamCity tries to detect new failing tests in a build, and for those tests which are not new, you can see in which build the test started to fail. This functionality is aware of branches too, i.e. when the first build is calculated, TeamCity traverses builds from the same branch.

Additionally, a [branch filter](branch-filter.md) is available on the test details page and you can see a history of test passes or failures in a single branch.

## Failure Conditions

If a build failure condition is configured as follows:  _build metric has changed comparing to a last successful/finished/pinned build_, then the build from the same branch will be used. If there is no suitable build on the same branch, then build from default branch is used and the corresponding message is added to the build log.

## Triggers

The VCS trigger is fully aware of branches and will trigger a build once a check\-in is detected in a branch. All VCS trigger options like per\-checkin triggering, quiet period, and triggering rules are directly available for builds from branches. By default, the Schedule and Finish build trigger will watch for builds in the default branch only.

Additionally, a [branch filter](branch-filter.md) can be specified for the VCS, Schedule and Finish build triggers.

## Dependencies

If a build configuration with branches has snapshot dependencies on other build configurations with branches, then when a build in a branch is triggered, the other builds in the chain will also get the branch associated, if the branches in the VCS roots of the builds have the same [logical name](#Logical+branch+name) and this branch is not excluded by the branch specification. The VCS roots of the builds can point to different repositories, but the logical branch name must be the same.

If this condition is met, the branches with this name will be checked out and all the builds down the chain (which the build triggered depends on) and all the builds up the chain (depending on the triggered build) will be marked with the same branch. Otherwise, the default branch will be checked out.

It is possible to configure artifact dependencies to retrieve artifacts from a build from a specific branch: artifact dependencies will use builds from the branch specified. The same applies to the [Schedule](configuring-schedule-triggers.md) and [Finish Build](configuring-finish-build-trigger.md) triggers.

## Notifications

All notification rules except "My changes" will only notify you on builds from the default branch. At the same time, the "My changes" rule will work for builds from all available branches.

## Build configuration status

The Build Configuration status is calculated based on the builds from the default branch only. Consequently, per-configuration investigation works for builds from the default branch. For example, a successful build from a non\-default branch will not remove a per-configuration investigation, but a successful build from the default branch will.

## Multiple VCS roots

If your build configuration uses more than one VCS root and you specified branches to monitor in both VCS roots, the way the builds are triggered is more complicated.

The VCS trigger groups branches from several VCS roots by [logical branch names](#Logical+branch+name). When some root does not have a branch from the other root, its default branch is used. For example, you have 2 VCS roots, both have the default branch `refs/heads/master`, the first root has the branch specification `refs/heads/7.1/*` and changes in branches `refs/heads/7.1/feature1` and `refs/heads/7.1/feature2`, the second root has the specification `refs/heads/devel/*` and changes in branch `refs/heads/devel/feature1`. In this case VCS trigger runs 3 builds with revisions from following branches combinations:

<table><tr>

<td>

root1

</td>

<td>

root2

</td>

</tr><tr>

<td>

refs/heads/master

</td>

<td>

refs/heads/master

</td>

</tr><tr>

<td>

refs/heads/7.1/feature1

</td>


<td>

refs/heads/devel/feature1

</td>

</tr><tr>

<td>

refs/heads/7.1/feature2

</td>



<td>

refs/heads/master

</td>

</tr></table>

## Build parameters

If you need to get the branch name in the build script or use it in other build configuration settings as a parameter, refer to [Predefined Build Parameters](predefined-build-parameters.md).

## Clean-up

Clean\-up rules are applied [independently](clean-up.md#Base+Rule+Behavior+for+Build+Configurations+with+Feature+Branches) to each [active branch](#Active+branches).

## Manual branch merging

You can merge branches in TeamCity manually, for example, if you want to merge branches only after a code review / approval, or if you want to perform the merge despite the tests failure in a branch.

__To merge sources manually__: 

Open the [build results page](working-with-build-results.md), click the __Actions__ menu in the upper right corner and select "__Merge this build sources__".   
 The dialog that appears enables you to select the destination branch and add a commit message (required).

It is also possible to merge branches [automatically](automatic-merge.md).

__  __

__See also:__

__Administrator's Guide__: [Git](git.md) | [Mercurial](mercurial.md)

__ __