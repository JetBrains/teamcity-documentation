[//]: # (title: Pull Requests)
[//]: # (auxiliary-id: Pull Requests)

The _Pull Requests_ [build feature](adding-build-features.md) lets you automatically load pull request\* information and run builds on pull request branches in [GitHub](#GitHub+Pull+Requests), [Bitbucket Server](#Bitbucket+Server+Pull+Requests), [Bitbucket Cloud](#Bitbucket+Cloud+Pull+Requests), [GitLab](#GitLab+Merge+Requests), [Azure DevOps](#Azure+DevOps+Pull+Requests), and [JetBrains Space](#JetBrains+Space+Merge+Requests).

\* Or _merge requests_ in case of GitLab and JetBrains Space.

> If your build configuration targets a repository where non-trusted users can push commits or create pull (merge) requests, do not configure [VCS triggers](configuring-vcs-triggers.md) and/or [Pull Requests build features](pull-requests.md) that automatically run builds with these changes. Instead, start new builds manually after you inspect and verify incoming changes. Otherwise, TeamCity can execute malicious code introduced in these changes (for example, handle a [service message](service-messages.md) sent from the source code or apply altered [project settings](storing-project-settings-in-version-control.md) from modified `.teamcity` folder files).
>
> See this section for more information about potential damage caused by users who can modify repository code: [](security-notes.md#manage-permissions).
>
{type="warning"}

When adding this build feature, you need to specify a VCS root and select a VCS hosting type.  
Other settings depend on the selected VCS hosting type.

This feature extends the original branch specification of [VCS roots](vcs-root.md), attached to the current build configuration, to include pull requests that match the specified filtering criteria.

<note>

* The branch specification of the VCS root __must not__ contain patterns matching pull request branches.
* If you want to trigger builds __only__ on pull requests, leave the branch specification of the VCS root empty.

</note>

You can find the pull request's details displayed on the __Overview__ tab of the __Build Results__:

<img src="pr-info.png" alt="Pull request details" width="706" border-effect="line"/>

In the case of a draft pull request, the icon is grayed-out and the **Draft** status appears before the pull request number:

<img src="pr-info2.png" alt="Pull request details" width="706" border-effect="line"/>

If you configure a [VCS trigger](configuring-vcs-triggers.md) for your build configuration, TeamCity will automatically run builds on changes detected in the monitored branches. For example, to auto-start building pull requests for [GitHub](#GitHub+Pull+Requests) repositories, add a new trigger (or modify the existing one) with the `+:pull/*` branch filter rule.

>For requests from GitHub and GitLab, you can set up TeamCity to automatically run a build on each request and merge the request if the build is successful.   
To achieve this, enable and configure the Pull Requests and [Automatic Merge](automatic-merge.md) build features.

See the [example](#Pull+Requests+workflow+example) on how to set up TeamCity to run builds on GitHub pull requests, or watch our **[video tutorial](https://www.youtube.com/watch?v=4yFck9PvXI4)**.

## VCS-specific settings

### GitHub Pull Requests

This feature supports [GitHub](https://github.com/) and [GitHub Enterprise](https://github.com/enterprise). It monitors builds only on the `refs/pull/*/head` branch.

The following parameters are available for the GitHub hosting type:

<table>
<tr>
<td width="150">

Parameter

</td>
<td width="150">

Option
    
</td>
<td>

Description

</td>
</tr>
<tr>
<td>

Authentication Type

</td>
<td>

Use VCS root credentials

</td>
<td>

TeamCity will try to extract username/password credentials or a personal access token/`x-oauth-basic` from the VCS root settings if the VCS root uses HTTP(S) fetch URL.

This option will not work if the VCS root employs anonymous authentication or SSH.

For a GitHub Enterprise repository, only the personal access token/`x-oauth-basic` pair will work.

</td>
</tr>
<tr>
<td></td>
<td>

Access token

</td>
<td>

Use a personal access token or obtain a token through an OAuth connection. It must have either the `public_repo` (for public repositories) or `repo` (for private repositories) scope.

</td>
</tr>


<tr>
<td></td>
<td>

GitHub App access token

</td>
<td>

A non-personal [short-lived token](git.md#refresh-token) issued via the GitHub App. This option is available only when the <b>VCS Root</b> setting points to the specific VCS root, and this root is configured using a [GitHub App connection](configuring-connections.md#GitHub).

</td>
</tr>

<tr>
<td>

By authors

_The filter applies to public repositories only._

</td>
<td>

Members of the same organization

</td>
<td>

Only detects pull requests submitted by [members of the same organization](https://help.github.com/en/articles/about-organization-membership) in GitHub.

</td>
</tr>
<tr>
<td></td>
<td>

Members and external collaborators

</td>
<td>

Only detects pull requests submitted by members of the same organization and [external collaborators](https://help.github.com/en/articles/adding-outside-collaborators-to-repositories-in-your-organization) in GitHub.

</td>
</tr>
<tr>
<td></td>
<td>

Everybody

</td>
<td>

Detects all pull requests. Be aware that selecting this option may allow arbitrary users to execute malicious code on your build agents.

</td>
</tr>
<tr>
<td>

By source branch

</td>
<td></td>
<td>

Define the [branch filter](branch-filter.md) to monitor pull requests only on source branches that match the specified criteria. If left empty, no filters apply.

</td>
</tr>
<tr>
<td>

By target branch

</td>
<td></td>
<td>

Define the [branch filter](branch-filter.md) to monitor pull requests only on target branches that match the specified criteria. If left empty, no filters apply.

</td>
</tr>
<tr>
<td>

Ignore Drafts

</td>
<td></td>
<td>

By default, the Pull Requests build feature loads the [GitHub draft pull requests](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests#draft-pull-requests)
information and runs builds on draft pull requests. The build page displays a grayed-out icon and the **Draft** status next to the merge request number.

Check the box to ignore GitHub draft pull requests. TeamCity will not load the draft pull request information until its status changes.

</td>
</tr>
<tr>
<td>

Server URL

</td>
<td></td>
<td>

Specify a GitHub URL for connection.

If left empty, the URL will be extracted from the VCS root fetch URL.

</td>
</tr>
</table>

### Bitbucket Server Pull Requests

The following parameters are available for the [Bitbucket Server](https://www.atlassian.com/software/bitbucket/enterprise/data-center) hosting type:

<table>
<tr>
<td width="150">

Parameter

</td>
<td width="150">

Options
    
</td>
<td>

Description

</td>
</tr>
<tr>
<td>

Authentication Type

</td>
<td>

Use VCS root credentials

</td>
<td>


TeamCity will try to extract username/password credentials from the VCS root settings if the VCS root uses HTTP(S) fetch URL.

This option will not work if the VCS root employs anonymous authentication.

</td>
</tr>
<tr>
<td></td>
<td>

Username/password

</td>
<td>

Specify a username and password for connection to Bitbucket Server.

You can submit an access token instead of the password. The token should have _Read_ permissions for projects and repositories.

</td>
</tr>
<tr>
<td>

By source branch

</td>
<td></td>
<td>

Define the [branch filter](branch-filter.md) to monitor pull requests only on source branches that match the specified criteria. If left empty, no filters apply.

</td>
</tr>
<tr>
<td>

By target branch

</td>
<td></td>
<td>

Define the [branch filter](branch-filter.md) to monitor pull requests only on target branches that match the specified criteria. If left empty, no filters apply.

</td>
</tr>
<tr>
<td>

Server URL

</td>
<td></td>
<td>

Specify a Bitbucket URL for connection.

If left empty, the URL will be extracted from the VCS root fetch URL.

</td>
  </tr>
<tr>
<td>

Use pull request branches

</td>
<td></td>
<td>

**This option is intended for backward compatibility only.** Enables detection of [officially unsupported Bitbucket](https://community.atlassian.com/t5/Bitbucket-questions/Current-Atlassian-position-regarding-refs-pull-requests-from/qaq-p/1376356#M54578) pull request branches (pull-requests/*) instead of source branches. 
Be careful: new builds might be triggered for changes committed within the last hour after switching.

</td>
  </tr>
</table>

### Bitbucket Cloud Pull Requests

<video href="M2wi6l0pZe4"
title="New in TeamCity 2020.2: Bitbucket Cloud Pull Request Support"/>

Since Bitbucket Cloud does not create dedicated branches for pull requests, this build feature monitors directly source branches in a source repository (forks are not supported).   
If more than one pull request is submitted from the same source branch at the moment of the build start, TeamCity will display all these requests in the build results. However, only commits from the open PRs matching the filtering criteria will be displayed as _Changes_ of the build.

Note that the branch specification of the VCS root __must not__ contain patterns matching pull request branches.

The following parameters are available for the [Bitbucket Cloud](https://bitbucket.org/) hosting type:

<table>
<tr>
<td>Setting</td>
<td>Description</td>
</tr>

<tr>

<td>Authentication Type</td>
<td>

* **Use VCS root credentials** — TeamCity will try to extract username/password credentials from the VCS root settings if the VCS root uses HTTP(S) fetch URL. This option will not work if the VCS root uses an SSH fetch URL or employs anonymous authentication.

* **Username/password** — Specify a username and password for connection to Bitbucket Cloud. We recommend using an [app password](https://support.atlassian.com/bitbucket-cloud/docs/app-passwords/) with the _Pull Requests | Read_ scope.

* **Refreshable access token** — Displays a list of configured Bitbucket Cloud [OAuth connections](configuring-connections.md#Bitbucket+Cloud). Click the **Acquire** button next to the connection that should be used to issue a short-lived OAuth token.
  <img src="dk-pullrequests-BBC-tokens.png" width="706" alt="PR Token for Bitbucket Cloud"/>
</td>
</tr>

<tr>
<td>By target branch</td>
<td>

Define the [branch filter](branch-filter.md) to monitor pull requests only on branches that match the specified criteria. If left empty, no filters apply.

</td>
</tr>

</table>

### GitLab Merge Requests

TeamCity processes GitLab [merge requests](https://docs.gitlab.com/ee/user/project/merge_requests/index.html) similarly to how it processes pull requests in other hosting services. Currently, TeamCity detects only merge requests submitted after this build feature is enabled.

This feature monitors builds only on the `refs/merge-requests/*/head` branch.

The following parameters are available for the [GitLab](https://gitlab.com/) hosting type:

<table>
<tr>
<td width="150">

Parameter

</td>

<td>

Description

</td>
</tr>
<tr>
<td>

Authentication Type

</td>

<td>

* **Use VCS root credentials** — TeamCity will try to extract login credentials or access token from the VCS root settings if the VCS root uses HTTP(S) fetch URL. This option will not work if the VCS root employs anonymous authentication.

* **Personal Access Token** — Use a personal access token issued in GitLab. It must have either the `api` scope.

* **GitLab Application Token** — Displays a list of configured [GitLab OAuth connections](configuring-connections.md#GitLab). Click the **Acquire** button next to the connection that should be used to issue a short-lived OAuth token.
<img src="dk-pullrequests-GitLabToken.png" width="706" alt="PR Token for GitLab"/>

</td>
</tr>
<tr>
<td>

By source branch

</td>
<td>

Define the [branch filter](branch-filter.md) to monitor merge requests only on source branches that match the specified criteria. If left empty, no filters apply.

</td>
</tr>
<tr>
<td>

By target branch

</td>
<td>

Define the [branch filter](branch-filter.md) to monitor merge requests only on target branches that match the specified criteria. If left empty, no filters apply.

</td>
</tr>
<tr>
<td>

Ignore Drafts

</td>
<td>

By default, the Pull Requests build feature loads the [GitLab draft merge requests](https://docs.gitlab.com/ee/user/project/merge_requests/drafts.html)
information and runs builds on draft merge requests. The build page displays a grayed-out icon and the **Draft** status next to the merge request number.

Check the box to ignore GitLab draft merge requests. TeamCity will not load draft merge request information and a merge request is ignored until its status changes to non-draft.

</td>
</tr>
<tr>
<td>

Server URL

</td>

<td>

Specify a GitLab URL for connection.

If left empty, the URL will be extracted from the VCS root fetch URL.

</td>
  </tr>
</table>

### Azure DevOps Pull Requests

This feature monitors builds only on the `refs/pull/*/merge` branch.

In case with [Azure DevOps](https://azure.microsoft.com/en-us/services/devops/), TeamCity detects requests on a merge branch — not on the pull request itself as with other VCSs. Each build will be launched on a virtual branch showing an actual result of the build after merging the PR. Thus, the build will contain both the commit with changes and the virtual merge commit.

Note that the feature ignores Azure DevOps draft pull requests.

The following parameters are available for the Azure DevOps hosting type:

<table>
<tr>
<td width="150">

Parameter

</td>

<td>

Description

</td>
</tr>
<tr>
<td>

Access token

</td>

<td>

Use a personal access token for connection. The token must have the `Code (read)` [scope](https://docs.microsoft.com/en-us/azure/devops/integrate/get-started/authentication/oauth?view=azure-devops#scopes).

</td>
</tr>
<tr>
<td>

By source branch

</td>
<td>

Define the [branch filter](branch-filter.md) to monitor pull requests only on source branches that match the specified criteria. If left empty, no filters apply.

</td>
</tr>
<tr>
<td>

By target branch

</td>
<td>

Define the [branch filter](branch-filter.md) to monitor pull requests only on target branches that match the specified criteria. If left empty, no filters apply.

</td>
</tr>
<tr>
<td>

Project URL

</td>

<td>

Specify a project URL for synchronization with the remote Azure DevOps server. This field is recommended for on-premises Azure DevOps installations. If left empty, the URL will be composed based on the VCS root fetch URL.

</td>
  </tr>
</table>

### JetBrains Space Merge Requests

>If you are looking for how to integrate your JetBrains Space instance with TeamCity, check out this **[full integration guide](how-to-configure-cicd-for-jetbrains-space.md)**!

This feature monitors merge requests directly in the source branches of an origin repository.  
If more than one merge request is submitted from the same source branch, TeamCity will display all these requests in the build results. However, only commits from the open requests matching the filtering criteria will be displayed as [Changes](build-results-page.md#Changes+Tab) of the build.

The following parameters are available for the [JetBrains Space](https://www.jetbrains.com/space/) hosting type:

<table>
<tr>
<td width="150">

Parameter

</td>

<td>

Description

</td>
</tr>
<tr>
<td>

Connection

</td>

<td>

Choose a preconfigured [connection to JetBrains Space](configuring-connections.md#jetbrains-space-connection).

</td>
</tr>

<tr>
<td>

By target branch

</td>

<td>

Define the [branch filter](branch-filter.md) to monitor merge requests only on branches that match the specified criteria. If left empty, no filters apply.
</td>
</tr>

</table>

If you want to run several parallel builds to pretest a request before merging it, the best solution is to:
1. Create a [composite build configuration](composite-build-configuration.md) and attach your JetBrains Space [VCS root](configuring-vcs-roots.md) with an empty branch specification to it.
2. Add the composite build configuration at the end of the build chain by configuring its [snapshot dependencies](build-dependencies-setup.md#Snapshot+Dependencies) on parallel builds with tests.
3. Add the _Pull Requests_ feature to each build configuration of the chain so that all builds can detect changes in a merge request branch. You can preconfigure all settings in a [build configuration template](build-configuration-template.md) and then create these build configurations based on it.
4. In the composite build configuration settings:
   * Add a [VCS trigger](configuring-vcs-triggers.md) to automatically run builds on changes detected in the merge request branch.
   * Add the [Commit Status Publisher](commit-status-publisher.md#JetBrains+Space) feature to send the build statuses to the commit details in JetBrains Space.  
   If you want other builds of the chain to report their statuses to JetBrains Space (for example, _deployment_ or _integration testing_ builds), add the _Commit Status Publisher_ feature to the corresponding build configurations.     

After that, TeamCity will automatically run builds on changes in a merge request branch submitted to your JetBrains Space repo and publish build statuses to the merge request timeline in Space:

<img src="space-timeline.png" alt="Space merge request timeline" width="700"/>

To protect a JetBrains Space branch from unverified merge requests, you can also configure [Quality Gates](https://www.jetbrains.com/help/space/branch-and-merge-restrictions.html#quality-gates-for-merge-requests) in your repository settings. If you set a TeamCity build as an external check, JetBrains Space will require the build on a merge request to finish successfully before allowing this request to be merged.

See known issues with processing JetBrains Space merge requests [here](known-issues.md#Known+issues+of+Pull+Requests+build+feature).

## Predefined build parameters for pull requests

TeamCity provides multiple [predefined build parameters](predefined-build-parameters.md) that expose valuable information on pull requests for builds with the enabled Pull Requests [feature](adding-build-features.md):
 
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

To configure the described workflow for the `web-app` build configuration in TeamCity:
1. __Add a [VCS root](vcs-root.md) to the build configuration__:   
   * Go to __Build Configuration Settings | Version Control Settings__ and click __Attach VCS root__.
   * Configure the root parameters:
      - __Type of VCS__: _Git_
      - __VCS root name__: _\<unique_root_name\>_
      - __Fetch URL__: _\<GitHub_repository_URL\>_
      - __Default branch__: the branch to be monitored; by default, _`refs/heads/master`_ (read more [about feature branches](working-with-feature-branches.md))
      - __Branch specification__: a filter for additional branches to be monitored (for example, _`+:refs/heads/*`_)
      - __Authentication parameters__ of the GitHub user that has access rights to the `web-app` repository
   * Test the connection and, if successful, click __Create__.
2. __Add the _Pull Requests_ [build feature](adding-build-features.md) to the build configuration__:
   * Go to __Build Configuration Settings | Build Features__ and click __Add build feature__.
   * Configure the feature parameters:
        * __VCS Root__: the VCS root created at Step 1
        * __VCS hosting type__: _GitHub_
        * __Authentication type__: _Use VCS root credentials_, or select _Access token_ to use a GitHub token instead        
        * __Pull Requests filtering__:
           * __By authors__: _Members of the same organization_
           * __By target branch__: leave empty to apply no filters and monitor all new pull requests in the repository, or explicitly specify the target branch (in this example, _`master`_)
   * Test the connection and, if successful, click __Save__.
3. Add a [VCS trigger](configuring-vcs-triggers.md) to the build configuration.

That's it! Now, whenever a member of your GitHub organization sends a pull request to the `master` branch, TeamCity acts as follows:
1. Detects the pull request sent to the `master` branch.
2. Runs the `web-app` build configuration: collects sources, builds and tests the app according to your predefined build steps.
3. Displays information about the processed pull request on the build configuration __Overview__ page. You can instantly see the pull request status (1) and refresh the information about its state (2).
   
   <img src="PullRequestOverviewInfo.png" width="500" alt="Pull Request information in Build Overview"/>

__Pro Tips__

You can automate your setup further, so TeamCity:
* Sends a build status back to GitHub after the build finishes, with the [Commit Status Publisher](commit-status-publisher.md) build feature.
* Merges the pull request in GitHub if the build finishes successfully, with the [Automatic Merge](automatic-merge.md) build feature.
* If you want to run a whole [build chain](build-chain.md) on a pull request, remember to add the Pull Requests feature to each build configuration of the chain. To simplify this procedure, you can set everything in a [build configuration template](build-configuration-template.md) and then create these build configurations based on it.

## Troubleshooting
{product="tc"}

TeamCity [writes events](teamcity-server-logs.md) related to the Pull Requests build feature to the `teamcity-pull-requests.log` file. Apply the "debug-pull-requests" preset to include DEBUG-level events to this log.

<seealso>
        <category ref="blog">
            <a href="https://blog.jetbrains.com/teamcity/2019/08/building-github-pull-requests-with-teamcity/">Building GitHub pull requests with TeamCity</a>
        </category>
        <category ref="examples">
            <a href="how-to-configure-cicd-for-jetbrains-space.md">How to Configure CI/CD for JetBrains Space</a>
        </category>
</seealso>