[//]: # (title: Branch Filter)
[//]: # (auxiliary-id: Branch Filter)

If a [VCS root](vcs-root.md) has [branches specified](working-with-feature-branches.md#Configuring+Branches), the _branch filter_ option becomes available for various operations in TeamCity.

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

Limit the set of branches to whose builds the labels will be applied.

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

Filters for this feature should reference branches by their fully clarified VCS names (`refs/heads/branch-*`) instead of shortened logical names (`branch-*`).

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
1. The [branch specification](working-with-feature-branches.md#Configuring+Branches) in the VCS root settings defines the initial set of the monitored branches.
2. If specified, a branch filter in the __Version Control Settings__ of a build configuration can narrow down the initial set of branches.
3. If specified, a branch filter in the settings of a build trigger applies to the subset declared by the filter (2). 

## Branch Filter Format

To filter branches, use a newline-delimited list of `+|-:logical_branch_name` rules, where `logical_branch_name` is the name displayed in the TeamCity UI (for example, `master`). The name is case-sensitive.

<snippet include-id="OR-syntax-tip">

> Here, the _pipe_ symbol `|` represents the __OR__ command, as in regular expressions: use `+` for including, __OR__ `-` for excluding.
>
{style="tip"}

</snippet>

<snippet include-id="vcs-branch-names-for-prs">

> Note that branch filters for the [](pull-requests.md) build feature accept fully clarified VCS branch names only, regardless of how these branches are displayed in TeamCity UI.
> 
> For example, if a user creates a new pull request by executing the `gh pr create --base source-branch --head master --assignee "@johndoe"` GitHub CLI command, use `refs/heads/source-branch` and/or `refs/heads/master` names to point the Pull Requests feature to the related target/source branches. Omitting `refs/heads/` and using shortened logical branch names (`[+|-]:source-branch`, `[+|-]:master`, `[+|-]:pull/10`) will result in faulty filters that do not target required branches.
> 
{style="note"}

</snippet>


 
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

<!--

## Pull Request Branch Filters

Use the `+|-pr: <parameter1>=<value1> <parameter2>=<value2> ...` syntax to create filters that target specific pull (merge) request branches. This universal syntax allows you to define fine-grained VCS-agnostic filter expressions that consider more specific parameters than just logical branch names.

> For the `+|-pr:...` filters to have any effect, the related VCS root must first be able to detect pull request branches. To do so, configure the [](pull-requests.md) build feature.
>
{style="tip"}

The `<parameter>=<value>` expressions are combined using the logical `AND` operator, meaning a pull (merge) request branch must satisfy all conditions to pass the filter. Currently, the following parameters and values are supported:

* `target` and `source` — allow you to filter requests by their incoming and outgoing branches. The `source` parameter is always `false` if a request originates from a forked repository. Supported values: logical branch names with optional [wildcards](#Wildcards+and+Patterns).

* `sourceRepo` — allows you to target only pull requests from the same repository (for example, when merging one repository branch into another), from forked repositories, or both. Supported values: `same`, `fork`, `any`.

* `draft` — specifies whether draft pull requests are accepted. This parameter is in effect only for GitHub and GitLab. Supported values: `true` to accept **only** requests labeled as drafts; `false` to accept **only** non-draft requests.

* `author` — allows you to filter pull (merge) requests by their authors. Note that the actual author of incoming changes may be different from a person who sent a request. Supported values: usernames.

* `github_role` — allows you to filter pull (merge) requests by author roles (relative to the repository organization). Supported values: `collaborator`, `contributor`, `member`, `owner`, `none` (not case-sensitive).

> Currently, joint parameter values are not supported. To enumerate multiple accepted values, add multiple stand-alone filter expressions (for example, `+pr: github_role=COLLABORATOR` and `+pr: github_role=CONTRIBUTOR`).
>
{style="note"}

-->

### Wildcards and Patterns

Use the asterisk ("*") as a wildcard for any string. For example, the `+pr:*` and `-pr:*` rules allow an object (a trigger or an [](automatic-merge.md) feature) accept or ignore all incoming requests.

The following rule allows an object to accept only those requests whose target branch starts with "dev/":

```Plain Text
+pr:target=dev/*
```

### Filter Priority

Pull request filter expressions are applied in the same manner as regular `+|-:<branch_name>` expressions: one by one starting with the first one. This means in case of conflicting expressions, the last one has the highest priority. For example, the following ruleset allows its parent object to accept all available branches, then excludes all pull request branches, and finally re-enables pull requests authored by organization members.

```Plain Text
+:*
-pr:*
+pr:github_role=member
```

The following combination of filters rejects pull requests coming from forked repositories even if they target the `main` branch (since the `sourceRepo` condition comes last).

```Plain Text
+pr:target=main
-pr:sourceRepo=fork
```


### Example

The following [Kotlin DSL](kotlin-dsl.md) sample illustrates how to combine [VCS](configuring-vcs-triggers.md) and [Schedule](configuring-schedule-triggers.md) triggers to implement the following setup:

* Changes in existing branches are build as soon as TeamCity detects them;
* Non-draft pull (merge) requests authored by organization members are built as soon as TeamCity collects information about them;
* Non-draft pull (merge) requests from collaborators and contributors are built nightly at 3:00 AM if these requests target one of the two stable repository branches.


```Kotlin
object Build : BuildType({
  triggers {
    vcs {
        branchFilter = """
            +:*
            -pr:*
            +pr: draft=false github_role=MEMBER
        """.trimIndent()
    }
    schedule {
        schedulingPolicy = daily { hour = 3 }
        branchFilter = """
            +pr: draft=false github_role=COLLABORATOR target=main
            +pr: draft=false github_role=COLLABORATOR target=development
            +pr: draft=false github_role=CONTRIBUTOR target=main
            +pr: draft=false github_role=CONTRIBUTOR target=development
        """.trimIndent()
        triggerBuild = always()
    }
  }
  features {
    pullRequests {
      vcsRootExtId = "${MyRoot.id}"
      provider = github {
        authType = vcsRoot()
        filterAuthorRole = PullRequests.GitHubRoleFilter.EVERYBODY
      }
    }
  }
})
```




