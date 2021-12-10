[//]: # (title: Pull Requests)
[//]: # (auxiliary-id: Pull Requests)

The _Pull Requests_ [build feature](adding-build-features.md) lets you automatically load pull request\* information and run builds on pull request branches in [GitHub](#GitHub+Pull+Requests), [Bitbucket Server](#Bitbucket+Server+Pull+Requests), [Bitbucket Cloud](#Bitbucket+Cloud+Pull+Requests), [GitLab](#GitLab+Merge+Requests), and [Azure DevOps](#Azure+DevOps+Pull+Requests).

\* Or _merge requests_ in case of GitLab.

When adding this build feature, you need to specify a VCS root and select a VCS hosting type.  
Other settings depend on the selected VCS hosting type.

This feature extends the original branch specification of [VCS roots](vcs-root.md), attached to the current build configuration, to include pull requests that match the specified filtering criteria.

<note>

* The branch specification of the VCS root __must not__ contain patterns matching pull request branches.
* If you want to trigger builds __only__ on pull requests, leave the branch specification of the VCS root empty.

</note>

You can find the pull request's details displayed on the __Overview__ tab of the __Build Results__:

<img src="pr-info.png" alt="Pull request details" width="700"/>

If you configure a [VCS trigger](configuring-vcs-triggers.md) for your build configuration, TeamCity will automatically run builds on changes detected in the monitored branches.

>For requests from GitHub and GitLab, you can set up TeamCity to automatically run a build on each request and merge the request if the build is successful.   
To achieve this, enable and configure the Pull Requests and [Automatic Merge](automatic-merge.md) build features.

See the [example](#Pull+Requests+workflow+example) on how to set up TeamCity to run builds on GitHub pull requests.

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

Define the [branch filter](branch-filter.md) to monitor pull requests only on source branches that match the specified criteria. If left blank, no filters apply.

</td>
</tr>
<tr>
<td>

By target branch

</td>
<td></td>
<td>

Define the [branch filter](branch-filter.md) to monitor pull requests only on target branches that match the specified criteria. If left blank, no filters apply.

</td>
</tr>
<tr>
<td>

Server URL

</td>
<td></td>
<td>

Specify a GitHub URL for connection.

If left blank, the URL will be extracted from the VCS root fetch URL.

</td>
  </tr>
</table>

### Bitbucket Server Pull Requests

This feature monitors builds only on the `refs/pull-requests/*/from` branch.

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

Define the [branch filter](branch-filter.md) to monitor pull requests only on source branches that match the specified criteria. If left blank, no filters apply.

</td>
</tr>
<tr>
<td>

By target branch

</td>
<td></td>
<td>

Define the [branch filter](branch-filter.md) to monitor pull requests only on target branches that match the specified criteria. If left blank, no filters apply.

</td>
</tr>
<tr>
<td>

Server URL

</td>
<td></td>
<td>

Specify a Bitbucket URL for connection.

If left blank, the URL will be extracted from the VCS root fetch URL.

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

This option will not work if the VCS root uses an SSH fetch URL or employs anonymous authentication.

</td>
</tr>
<tr>
<td></td>
<td>

Username/password

</td>
<td>

Specify a username and password for connection to Bitbucket Cloud. We recommend using an [app password](https://support.atlassian.com/bitbucket-cloud/docs/app-passwords/) with the _Pull Requests | Read_ scope.

</td>
</tr>
<tr>
<td>

By target branch

</td>
<td></td>
<td>

Define the [branch filter](branch-filter.md) to monitor pull requests only on branches that match the specified criteria. If left blank, no filters apply.

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

Access token

</td>

<td>

Use a personal access token for connection. The token must have the `api` scope.

</td>
</tr>
<tr>
<td>

By source branch

</td>
<td></td>
<td>

Define the [branch filter](branch-filter.md) to monitor merge requests only on source branches that match the specified criteria. If left blank, no filters apply.

</td>
</tr>
<tr>
<td>

By target branch

</td>
<td></td>
<td>

Define the [branch filter](branch-filter.md) to monitor merge requests only on target branches that match the specified criteria. If left blank, no filters apply.

</td>
</tr>
<tr>
<td>

Server URL

</td>

<td>

Specify a GitLab URL for connection.

If left blank, the URL will be extracted from the VCS root fetch URL.

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
<td></td>
<td>

Define the [branch filter](branch-filter.md) to monitor pull requests only on source branches that match the specified criteria. If left blank, no filters apply.

</td>
</tr>
<tr>
<td>

By target branch

</td>
<td></td>
<td>

Define the [branch filter](branch-filter.md) to monitor pull requests only on target branches that match the specified criteria. If left blank, no filters apply.

</td>
</tr>
<tr>
<td>

Project URL

</td>

<td>

Specify a project URL for synchronization with the remote Azure DevOps server. This field is recommended for on-premises Azure DevOps installations. If left blank, the URL will be composed based on the VCS root fetch URL.

</td>
  </tr>
</table>

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
           * __By target branch__: leave blank to apply no filters and monitor all new pull requests in the repository, or explicitly specify the target branch (in this example, _`master`_)
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

<seealso>
        <category ref="blog">
            <a href="https://blog.jetbrains.com/teamcity/2019/08/building-github-pull-requests-with-teamcity/">Building GitHub pull requests with TeamCity</a>
        </category>
</seealso>