[//]: # (title: Pull Requests)
[//]: # (help-id: Pull Requests)

The _Pull Requests_ [build feature](adding-build-features.md) integrates TeamCity with pull (merge) requests in [GitHub](#GitHub+Pull+Requests), [Bitbucket Server](#Bitbucket+Server+Pull+Requests), [Bitbucket Cloud](#Bitbucket+Cloud+Pull+Requests), [GitLab](#GitLab+Merge+Requests), [Azure DevOps](#Azure+DevOps+Pull+Requests), and [JetBrains Space](#JetBrains+Space+Merge+Requests) repositories.

## Common Information

Adding the Pull Requests feature to a build configuration allows you to:

* View pull request branches and their pending changes on the build configuration's overview page.

   <img src="dk-pull-branches.png" width="706" alt="New pull branches on the main build configuration page"/>

* View pull request details on the **Overview** tab of the [build results page](build-results-page.md).

   <img src="pr-info.png" alt="Pull request details" width="706" border-effect="line"/>

   For a draft pull request, the icon is grayed-out and the **Draft** status appears before the pull request number:

   <img src="pr-info2.png" alt="Pull request details" width="706" border-effect="line"/>

* Filter which pull requests to monitor by their authors, target branches, and origin branches.

   <include from="branch-filter.md" element-id="vcs-branch-names-for-prs"/>

* Set up a workflow where developers work in local branches, and TeamCity only builds these changes once they are sent as a pull (merge) request — see [](#Interaction+with+VCS+Roots) below.

The Pull Requests feature **does not** automatically trigger new builds against pull (merge) request branches. To assess changes from pull request branches before they are merged into the main codebase, add a [VCS trigger](configuring-vcs-triggers.md) that targets the required branches (for example, `refs/pull/*` for GitHub). New build configurations created in the TeamCity UI already include a trigger with the `+:*` specification, which lets TeamCity build changes from pull (merge) request branches.

> If your build configuration targets a public repository where non-trusted users can push commits or create pull (merge) requests, building these changes means TeamCity can execute malicious code introduced in them. For example, TeamCity may handle a harmful [service message](service-messages.md) sent from the source code or apply altered [project settings](storing-project-settings-in-version-control.md) from modified `.teamcity` folder files.
>
> To prevent this, do not configure [VCS triggers](configuring-vcs-triggers.md) and [Pull Requests build features](pull-requests.md) in a way that lets builds with unverified pull (merge) request changes start automatically. Instead, either start new builds manually after inspecting and verifying incoming changes, or set up [](untrusted-builds.md) to require additional review for external pull requests.
>
> See this section for more information about potential damage caused by users who can modify repository code: [](security-notes.md#manage-permissions).
>
{style="warning"}

If your project targets a GitHub or GitLab repository, you can go further and let TeamCity build pull request branches and merge those requests that yield successful builds. To do this, add the [Automatic Merge](automatic-merge.md) build feature alongside **Pull Requests**.

## Interaction with VCS Roots

The Pull Requests feature extends the original branch specification of [VCS roots](configuring-vcs-roots.md) attached to the current build configuration. Because of this, branch specifications of a VCS root **must not** contain patterns that match pull request branches, to avoid ambiguous and unexpected behavior.

If a build configuration should build **only** pull requests, clear the branch specification of its parent VCS root:

```Kotlin
object MyRepoRoot : GitVcsRoot({
    name = "MyRoot"
    url = "https://github.com/username/reponame"
    branch = "refs/heads/main"
    // Note the "branchSpec = ..." parameter is missing
})
```

If this VCS root is shared with other configurations, use configuration [branch filters](branch-filter.md) instead, to leave only pull request branches available:

```Kotlin
project {
    vcsRoot(SharedVcsRoot)
    buildType(PullRequestsConfig)
}

// VCS root with a branch spec
object SharedVcsRoot : GitVcsRoot({
    name = "shared-vcs-root"
    branch = "main"
    branchSpec = """
        refs/heads/main
        refs/heads/sandbox
        refs/heads/dev-*
    """.trimIndent()
})

// Build configuration that nullifies root's branch spec
object PullRequestsConfig : BuildType({
    name = "Pull Requests Config"

    vcs {
        root(SharedVcsRoot)

        branchFilter = """
            -:*
            +:refs/pull/*   
        """.trimIndent()
        /* exclude all branches and re-add pull request ones */
    }
})
```

The sample below shows how to set up a TeamCity project so that the root branch specifications and the Pull Requests feature complement each other to implement the following workflow:

* TeamCity tracks only the `main`, `production`, and `sandbox` branches, as well as all branches starting with "release-" (for example, `release-2077.02`).

* Developers can create and work with local branches without any exposure to TeamCity.

* When a developer is ready to publish their changes, they create a request to merge commits from a personal (untracked) branch into a core (tracked) one. This results in a new pull branch (for example, `refs/pull/54` on GitHub). The Pull Requests feature detects this new branch, making it possible to build and test the changes in TeamCity before they are merged.

* Thanks to the feature's **Filter by author** setting, TeamCity ignores similar `refs/pull/<Int>` branches created by unauthorized (external) users.

```Kotlin
project {
    vcsRoot(MyRepoRoot)
    buildType(Build)
}

object Build : BuildType({
    name = "Build"
    vcs { root(MyRepoRoot) }

    // ...

    features {
        pullRequests {
            vcsRootExtId = "${MyRepoRoot.id}"
            provider = github {
                authType = vcsRoot()
                filterAuthorRole = PullRequests.GitHubRoleFilter.MEMBER
            }
        }
    }
})

object MyRepoRoot : GitVcsRoot({
    name = "MyRoot"
    url = "https://github.com/username/reponame"
    branch = "refs/heads/main"
    branchSpec = """
        refs/heads/main
        refs/heads/production
        refs/heads/sandbox
        refs/heads/release-*
    """.trimIndent()
})
```

<!--

## Pull Request Branch Filters

After you configure the **Pull Requests** feature in your build configurations, you can add `+|-pr <attribute>=<value>` expressions to branch filters of triggers, notifications, configuration Version Control Settings, and more. These expressions allow corresponding objects to work only with specific pull (merge) requests. For example, you can set up the triggers to start new builds only for non-draft requests coming from your organization members. Refer to this section for more information: [](branch-filter.md#Pull+Request+Branch+Filters).

-->

## VCS-specific settings

### GitHub Pull Requests

This feature supports [GitHub](https://github.com/) and [GitHub Enterprise](https://github.com/enterprise). It monitors builds only on the `refs/pull/*/head` branch.

The following parameters are available for the GitHub hosting type:

<deflist type="medium">

<def title="Authentication Type">

* **Use VCS root credentials** — TeamCity tries to extract username/password credentials or a personal access token/`x-oauth-basic` from the VCS root settings if the VCS root uses an HTTP(S) fetch URL. This option does not work if the VCS root employs anonymous authentication or SSH. For a GitHub Enterprise repository, only the personal access token/`x-oauth-basic` pair works.

* **Access token** — if you have a [configured OAuth connection](configuring-connections.md#GitHub) to GitHub, click the magic wand button to let TeamCity automatically retrieve the corresponding access token.

    <img src="dk-CSP-GitHubToken.png" width="708" alt="Acquire access token for GitHub"/>

    Otherwise, if you insert a token manually issued on the GitHub side, make sure it has the following permissions or scopes:

    * Classic GitHub tokens: `public_repo` for public repositories, `repo` for private repositories. See also: [Scopes for OAuth apps](https://docs.github.com/en/apps/oauth-apps/building-oauth-apps/scopes-for-oauth-apps).
    * Fine-grained tokens: add the `Pull requests` permission with the "Read-only" access type. This permission can only be added for tokens with the "All repositories" or "Only select repositories" access type. See also: [Permissions required for fine-grained personal access tokens](https://docs.github.com/en/rest/authentication/permissions-required-for-fine-grained-personal-access-tokens).

* **GitHub App access token** — a non-personal [short-lived token](git.md#refresh-token) issued via the GitHub App. Available only when the **VCS Root** setting points to a specific VCS root configured using a [GitHub App connection](configuring-connections.md#GitHub).

</def>


<def title="Matching mode">

Determines how TeamCity identifies a pull request across a [build chain](build-chain.md) that spans multiple repositories, so that each configuration in the chain builds the correct, corresponding change.

* **Pull request refs only** (default) — TeamCity identifies a pull request by its `refs/pull/N/head` reference and displays it as `pull/N`. Because pull request numbers are assigned independently within each repository, identical numbers may correspond to unrelated pull requests across repositories. Consider a change that spans two repositories, _Plugin A_ and _Plugin B_: it may create `refs/pull/20/head` in _Plugin A_, while the more active _Plugin B_ repository assigns it number 50, creating `refs/pull/50/head`. To preserve consistency across the chain, TeamCity builds _Plugin B_ on the same `pull/20` branch used for _Plugin A_. However, this branch corresponds to a different, unrelated pull request in _Plugin B_, causing the build to run against outdated or irrelevant changes.

* **Match by source branch** — TeamCity identifies a pull request by the name of its source branch rather than its reference, and displays it using that branch name instead of `pull/N`. For example, in the scenario above, both pull requests originate from the same `sandbox` branch in their respective repositories. TeamCity uses this shared branch name to match the two pull requests, allowing each repository in the chain to build its own correct, corresponding change.

</def>


<def title="By authors">

Filters which pull requests TeamCity monitors by their author. _Applies to public repositories only._

* **Members of the same organization** — only detects pull requests submitted by [members of the same organization](https://help.github.com/en/articles/about-organization-membership).
* **Members and external collaborators** — only detects pull requests submitted by members of the same organization and [external collaborators](https://help.github.com/en/articles/adding-outside-collaborators-to-repositories-in-your-organization).
* **Everybody** — detects all pull requests. Be aware that this option may allow arbitrary users to execute malicious code on your build agents.

</def>

<def title="By source branch">

Restricts monitored pull requests to source branches matching this [branch filter](branch-filter.md). Leave empty to apply no filter.

</def>

<def title="By target branch">

Restricts monitored pull requests to target branches matching this [branch filter](branch-filter.md). Leave empty to apply no filter.

</def>

<def title="Ignore Drafts">

By default, the Pull Requests build feature loads [GitHub draft pull requests](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests#draft-pull-requests) information and runs builds on them — the build page shows a grayed-out icon and the **Draft** status next to the pull request number.

Check the box to ignore GitHub draft pull requests. TeamCity will not load draft pull request information until its status changes.

</def>

<def title="Server URL">

A GitHub URL for connection. If left empty, the URL is extracted from the VCS root fetch URL.

</def>

</deflist>

<anchor name="Bitbucket+Server+Pull+Requests"/>

### Bitbucket Server/Data Center Pull Requests

The following parameters are available for the [Bitbucket Server/Data Center](https://www.atlassian.com/software/bitbucket/enterprise/data-center) hosting type:

<deflist type="medium">

<def title="Authentication Type">

* **Use VCS root credentials** — TeamCity tries to extract username/password credentials from the VCS root settings if the VCS root uses an HTTP(S) fetch URL. This option does not work if the VCS root uses an SSH fetch URL or employs anonymous authentication.
* **Username/password** — specify a username and password for connection to Bitbucket Server/Data Center. You can submit an access token instead of the password; the token should have _Read_ permissions for projects and repositories.
* <include from="common-templates.md" element-id="rat-single"/>

</def>

<def title="By source branch">

Restricts monitored pull requests to source branches matching this [branch filter](branch-filter.md). Leave empty to apply no filter.

</def>

<def title="By target branch">

Restricts monitored pull requests to target branches matching this [branch filter](branch-filter.md). Leave empty to apply no filter.

</def>

<def title="Server URL">

A Bitbucket URL for connection. If left empty, the URL is extracted from the VCS root fetch URL.

</def>

<def title="Use pull request branches">

**Intended for backward compatibility only.** Enables detection of [officially unsupported Bitbucket](https://community.atlassian.com/t5/Bitbucket-questions/Current-Atlassian-position-regarding-refs-pull-requests-from/qaq-p/1376356#M54578) pull request branches (`pull-requests/*`) instead of source branches.

Be careful: new builds might be triggered for changes committed within the last hour after switching.

</def>

</deflist>

### Bitbucket Cloud Pull Requests

<video src="https://youtu.be/M2wi6l0pZe4"
title="New in TeamCity 2020.2: Bitbucket Cloud Pull Request Support"/>

Since Bitbucket Cloud does not create dedicated branches for pull requests, this build feature monitors source branches directly in a source repository (forks are not supported).
If more than one pull request is submitted from the same source branch at the moment the build starts, TeamCity displays all these requests in the build results. However, only commits from open PRs matching the filtering criteria are displayed as _Changes_ of the build.

Note that the branch specification of the VCS root __must not__ contain patterns matching pull request branches.

The following parameters are available for the [Bitbucket Cloud](https://bitbucket.org/) hosting type:

<deflist type="medium">

<def title="Authentication Type">

* **Use VCS root credentials** — TeamCity tries to extract username/password credentials from the VCS root settings if the VCS root uses an HTTP(S) fetch URL. This option does not work if the VCS root uses an SSH fetch URL or employs anonymous authentication.
* **Username/password** — specify a username and password for connection to Bitbucket Cloud. We recommend using an [app password](https://support.atlassian.com/bitbucket-cloud/docs/app-passwords/) with the _Pull Requests | Read_ scope.
* <include from="common-templates.md" element-id="rat-single"/>
* **Permanent Access Token** — enter a Bitbucket [Repository Access Token](https://support.atlassian.com/bitbucket-cloud/docs/create-a-repository-access-token/), [Project Access Token](https://support.atlassian.com/bitbucket-cloud/docs/using-project-access-tokens/), or [Workspace Access Token](https://support.atlassian.com/bitbucket-cloud/docs/using-workspace-access-tokens/) for long-lived access to a repository, workspace, or project. The token must have the _Pull Requests | Read_ scope.

</def>

<def title="By target branch">

Restricts monitored pull requests to branches matching this [branch filter](branch-filter.md). Leave empty to apply no filter.

</def>

</deflist>

### GitLab Merge Requests

TeamCity processes GitLab [merge requests](https://docs.gitlab.com/ee/user/project/merge_requests/index.html) similarly to how it processes pull requests in other hosting services. Currently, TeamCity only detects merge requests submitted after this build feature is enabled.

This feature monitors builds only on the `refs/merge-requests/*/head` branch.

The following parameters are available for the [GitLab](https://gitlab.com/) hosting type:

<deflist type="medium">

<def title="Authentication Type">

* **Use VCS root credentials** — TeamCity tries to extract login credentials or an access token from the VCS root settings if the VCS root uses an HTTP(S) fetch URL. This option does not work if the VCS root employs anonymous authentication.
* **Personal Access Token** — use a personal access token issued in GitLab. It must have the `api` scope.
* <include from="common-templates.md" element-id="rat-single"/>

</def>

<def title="By source branch">

Restricts monitored merge requests to source branches matching this [branch filter](branch-filter.md). Leave empty to apply no filter.

</def>

<def title="By target branch">

Restricts monitored merge requests to target branches matching this [branch filter](branch-filter.md). Leave empty to apply no filter.

</def>

<def title="Ignore Drafts">

By default, the Pull Requests build feature loads [GitLab draft merge requests](https://docs.gitlab.com/ee/user/project/merge_requests/drafts.html) information and runs builds on them — the build page shows a grayed-out icon and the **Draft** status next to the merge request number.

Check the box to ignore GitLab draft merge requests. TeamCity will not load draft merge request information, and a merge request is ignored until its status changes to non-draft.

</def>

<def title="Server URL">

A GitLab URL for connection. If left empty, the URL is extracted from the VCS root fetch URL.

</def>

</deflist>

### Azure DevOps Pull Requests

This feature monitors builds only on the `refs/pull/*/merge` branch.

For [Azure DevOps](https://azure.microsoft.com/en-us/services/devops/), TeamCity detects requests on a merge branch — not on the pull request itself as with other VCSs. Each build is launched on a virtual branch showing the actual result of the build after merging the PR, so the build contains both the commit with changes and the virtual merge commit.

Note that the feature ignores Azure DevOps draft pull requests.

<deflist type="medium">

<def title="Authentication Type">

* **Personal Access Token** — a static token that you can [issue in your Azure DevOps account settings](https://www.visualstudio.com/en-us/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate). Your issued token should have the `Code (read)` scope to allow Pull Requests to retrieve the required information.
* <include from="common-templates.md" element-id="rat-single"/>

</def>

<def title="By source branch">

Restricts monitored pull requests to source branches matching this [branch filter](branch-filter.md). Leave empty to apply no filter.

</def>

<def title="By target branch">

Restricts monitored pull requests to target branches matching this [branch filter](branch-filter.md). Leave empty to apply no filter.

</def>

<def title="Project URL">

A project URL for synchronization with the remote Azure DevOps server. Recommended for on-premises Azure DevOps installations. If left empty, the URL is composed based on the VCS root fetch URL.

</def>

</deflist>

### JetBrains Space Merge Requests

>If you are looking for how to integrate your JetBrains Space instance with TeamCity, check out this **[full integration guide](how-to-configure-cicd-for-jetbrains-space.md)**!

This feature monitors merge requests directly in the source branches of an origin repository.
If more than one merge request is submitted from the same source branch, TeamCity displays all these requests in the build results. However, only commits from open requests matching the filtering criteria are displayed as [Changes](build-results-page.md#Changes+Tab) of the build.

The following parameters are available for the [JetBrains Space](https://www.jetbrains.com/space/) hosting type:

<deflist type="medium">

<def title="Connection">

Choose a preconfigured [connection to JetBrains Space](configuring-connections.md#jetbrains-space-connection).

</def>

<def title="By target branch">

Restricts monitored merge requests to branches matching this [branch filter](branch-filter.md). Leave empty to apply no filter.

</def>

</deflist>

If you want to run several parallel builds to pretest a request before merging it, the best solution is to:
1. Create a [composite build configuration](composite-build-configuration.md) and attach your JetBrains Space [VCS root](configuring-vcs-roots.md) with an empty branch specification to it.
2. Add the composite build configuration at the end of the build chain by configuring its [snapshot dependencies](configuring-dependencies.md#Snapshot+dependencies) on parallel builds with tests.
3. Add the _Pull Requests_ feature to each build configuration of the chain so that all builds can detect changes in a merge request branch. You can preconfigure all settings in a [build configuration template](build-configuration-template.md) and then create these build configurations based on it.
4. In the composite build configuration settings:
   * Add a [VCS trigger](configuring-vcs-triggers.md) to automatically run builds on changes detected in the merge request branch.
   * Add the [Commit Status Publisher](commit-status-publisher.md) feature to send the build statuses to the commit details in JetBrains Space.
   If you want other builds of the chain to report their statuses to JetBrains Space (for example, _deployment_ or _integration testing_ builds), add the _Commit Status Publisher_ feature to the corresponding build configurations.

After that, TeamCity automatically runs builds on changes in a merge request branch submitted to your JetBrains Space repo and publishes build statuses to the merge request timeline in Space:

<img src="space-timeline.png" alt="Space merge request timeline" width="700"/>

To protect a JetBrains Space branch from unverified merge requests, you can also configure [Quality Gates](https://www.jetbrains.com/help/space/branch-and-merge-restrictions.html#quality-gates-for-merge-requests) in your repository settings. If you set a TeamCity build as an external check, JetBrains Space requires the build on a merge request to finish successfully before allowing this request to be merged.

See known issues with processing JetBrains Space merge requests [here](known-issues.md#Known+issues+of+Pull+Requests+build+feature).

## Predefined build parameters for pull requests

TeamCity provides several [predefined build parameters](predefined-build-parameters.md) that expose valuable pull request information for builds with the Pull Requests [feature](adding-build-features.md) enabled:

```Text
teamcity.pullRequest.number //pull request number
teamcity.pullRequest.title //pull request title
teamcity.pullRequest.source.branch //VCS name of the source branch; provided only if the source repository is the same as the target one
teamcity.pullRequest.target.branch //VCS name of the target branch

```

You can use these parameters in the settings of a build configuration or in build scripts.

## Pull Requests workflow example

Let's say you have the following environment set up:
* Public GitHub repository `web-app` with the default branch `master`.
* TeamCity project.
    * Build configuration `web-app` that uses files from the `web-app` repository to build a web application.

The members of your [organization](https://help.github.com/en/articles/about-organizations) propose changes to the sources by sending pull requests to the `master` branch, and you want these changes to be automatically built and tested in TeamCity before you merge them.
TeamCity can detect each pull request sent to the `master` branch and build the web application based on the updated sources.

To configure this workflow for the `web-app` build configuration in TeamCity:

1. Add a [VCS root](configuring-vcs-roots.md) to the build configuration:

    * <include from="common-templates.md" element-id="open-configuration-settings-tab"><var name="configuration-tab-name" value="Version Control Settings"/></include>
   * Click __Attach VCS root__.
   * Configure the root parameters:
      - __Type of VCS__: _Git_
      - __VCS root name__: _\<unique_root_name\>_
      - __Fetch URL__: _\<GitHub_repository_URL\>_
      - __Default branch__: the branch to be monitored; by default, _`refs/heads/master`_ (read more [about feature branches](working-with-feature-branches.md))
      - __Branch specification__: a filter for additional branches to be monitored (for example, _`+:refs/heads/*`_)
      - __Authentication Settings__ of the GitHub user that has access rights to the `web-app` repository
   * Test the connection and, if successful, click __Create__.
2. Add the _Pull Requests_ [build feature](adding-build-features.md) to the build configuration:

    * <include from="common-templates.md" element-id="open-configuration-settings-tab"><var name="configuration-tab-name" value="Build Features"/></include>
    * Click __Add build feature__.
    * Configure the feature parameters:
        * __VCS Root__: the VCS root created at Step 1
        * __VCS hosting type__: _GitHub_
        * __Authentication Type__: _Use VCS root credentials_, or select _Access token_ to use a GitHub token instead
        * __Pull Requests filtering__:
           * __By authors__: _Members of the same organization_
           * __By target branch__: leave empty to apply no filters and monitor all new pull requests in the repository, or explicitly specify the target branch (in this example, _`master`_)
    * Test the connection and, if successful, click __Save__.
3. Add a [VCS trigger](configuring-vcs-triggers.md) to the build configuration.

With this integration in place, whenever a member of your GitHub organization sends a pull request to the `master` branch, TeamCity does the following:

1. Detects the pull request sent to the `master` branch.
2. Runs the `web-app` build configuration: collects sources, builds and tests the app according to your predefined build steps.
3. Displays information about the processed pull request on the build configuration __Overview__ page. You can instantly see the pull request status (1) and refresh the information about its state (2).

   <img src="PullRequestOverviewInfo.png" width="500" alt="Pull Request information in Build Overview"/>

__Pro Tips__

You can automate your setup further, so TeamCity:
* Sends a build status back to GitHub after the build finishes, with the [Commit Status Publisher](commit-status-publisher.md) build feature.
* Merges the pull request in GitHub if the build finishes successfully, with the [Automatic Merge](automatic-merge.md) build feature.
* If you want to run a whole [build chain](build-chain.md) on a pull request, remember to add the Pull Requests feature to each build configuration of the chain. To simplify this, you can set everything up in a [build configuration template](build-configuration-template.md) and then create these build configurations based on it.

## Troubleshooting
{instance="tc"}

TeamCity [writes events](teamcity-server-logs.md) related to the Pull Requests build feature to the `teamcity-pull-requests.log` file. Apply the "debug-pull-requests" preset to include DEBUG-level events in this log.

<seealso>
        <category ref="blog">
            <a href="https://blog.jetbrains.com/teamcity/2019/08/building-github-pull-requests-with-teamcity/">Building GitHub pull requests with TeamCity</a>
        </category>
        <category ref="examples">
            <a href="how-to-configure-cicd-for-jetbrains-space.md">How to Configure CI/CD for JetBrains Space</a>
        </category>
</seealso>
