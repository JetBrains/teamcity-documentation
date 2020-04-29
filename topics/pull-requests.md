[//]: # (title: Pull Requests)
[//]: # (auxiliary-id: Pull Requests)

The _Pull Requests_ build feature lets you automatically load pull request (or _merge requests_ in case of GitLab) information and run builds on pull request branches in [GitHub](#GitHub+Pull+Requests), [Bitbucket Server](#Bitbucket+Server+Pull+Requests), and [GitLab](#GitLab+Merge+Requests).

The feature extends the original branch specification of the VCS roots, attached to the current build configuration, to include pull requests that match the specified filtering criteria. It monitors builds only on `head` branches:
* For GitHub: `refs/pull/*/head`
* For Bitbucket Server: `refs/pull-requests/*/from`
* For GitLab: `refs/merge-requests/*/head`

If you configure a [VCS trigger](configuring-vcs-triggers.md) for your build configuration, TeamCity will automatically run builds on changes detected in the monitored branches.

You can find the pull request's details displayed on the __Overview__ tab of the __Build Results__:

<img src="pr-info.png" alt="Pull request details" width="700"/>

When adding this build feature, you need to specify a VCS root and select a VCS hosting type.

<note>

The branch specification of the VCS root __must not__ contain patterns matching pull request branches.

</note>

The build feature parameters depend on the selected VCS hosting type.

<tip>

For requests from GitHub and GitLab, you can set up TeamCity to automatically run a build on each request and merge the request if the build is successful.   
To achieve this, enable and configure the Pull Requests and [Automatic Merge](automatic-merge.md) build features.

</tip>

See the [example](#Workflow+example+for+the+Pull+Requests+build+feature) on how to set up TeamCity to run builds on pull requests.

## VCS-specific settings

### GitHub Pull Requests

TeamCity supports [GitHub](https://github.com/) and [GitHub Enterprise](https://github.com/enterprise).

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

The '_Token_' field appears.

Use a personal access token or obtain a token through an OAuth connection. It must have either the `public_repo` (for public repositories) or `repo` (for private repositories) scope.

</td>
</tr>
<tr>
<td>

By authors


<note>

The filter applies to public repositories only.

</note>

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

By target branch

</td>
<td></td>
<td>

Define the [branch filter](branch-filter.md) to monitor pull requests only on branches that match the specified criteria. If left blank, no filters apply.

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

The '_Username_' and '_Password_' fields appear.

Specify a username and password for connection to Bitbucket Server.


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


### GitLab Merge Requests

TeamCity processes GitLab [merge requests](https://docs.gitlab.com/ee/user/project/merge_requests/index.html) similarly to how it processes pull requests in other hosting services. Currently, TeamCity detects only merge requests submitted after this build feature is enabled.

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

By target branch

</td>

<td>

Define the [branch filter](branch-filter.md) to monitor merge requests only on branches that match the specified criteria. If left blank, no filters apply.

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

## Pull Requests workflow example

Let's say you have the following environment set up:
* public GitHub repository `web-app` with the default branch `master`
* TeamCity project
    * build configuration `web-app` that uses files from the `web-app` repository to build a web application

The members of your [organization](https://help.github.com/en/articles/about-organizations) propose changes to the sources by sending pull requests to the `master` branch, and you want these changes to be automatically built and tested in TeamCity before you merge them.   
TeamCity can detect each pull request sent to the `master` branch and build the web application based on the updated sources.

<note>

The `web-app` build configuration must have a VCS trigger enabled.

</note>

To configure the described pipeline for the `web-app` build configuration in TeamCity:
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

That's it! Now, when a member of your GitHub organization sends a pull request to the `master` branch, TeamCity acts as follows:
1. Detects the pull request sent to the `master` branch.
2. Runs the `web-app` build configuration: collects sources, builds and tests the app according to your predefined build steps.
3. Displays information about the processed pull request on the build configuration __Overview__ page. You can instantly see the pull request status (1) and refresh the information about its state (2).   
   
   <img src="PullRequestOverviewInfo.png" width="500" alt="Pull Request information in Build Overview"/>

__Pro Tip__

You can automate your setup further, so TeamCity:
* sends a build status back to GitHub after the build finishes, with the [Commit Status Publisher](commit-status-publisher.md) build feature
* merges the pull request in GitHub if the build finishes successfully, with the [Automatic Merge](automatic-merge.md) build feature

## Predefined build parameters for pull requests

TeamCity provides multiple [predefined build parameters](predefined-build-parameters.md) that expose valuable information on pull requests for builds with the enabled Pull Requests [feature](adding-build-features.md):
 
```Text
teamcity.pullRequest.number //pull request number
teamcity.pullRequest.title //pull request title
teamcity.pullRequest.source.branch //VCS name of the source branch; provided only if the source repository is the same as the target one
teamcity.pullRequest.target.branch //VCS name of the target branch

```

You can use these parameters in the settings of a build configuration or in build scripts.

<seealso>
        <category ref="blog">
            <a href="https://blog.jetbrains.com/teamcity/2019/08/building-github-pull-requests-with-teamcity/">Building GitHub pull requests with TeamCity</a>
        </category>
</seealso>