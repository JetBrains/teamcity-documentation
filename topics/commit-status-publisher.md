[//]: # (title: Commit Status Publisher)
[//]: # (auxiliary-id: Commit Status Publisher)

Commit Status Publisher is a [build feature](adding-build-features.md) which allows TeamCity 
to automatically send build statuses of your commits to an external system. 
The feature is implemented as an [open-source plugin](https://github.com/JetBrains/commit-status-publisher) bundled with TeamCity.

Supported systems:
* GitHub (the build statuses for pull requests are supported as well)
* GitLab
* Azure DevOps (supported statuses: Pending, Succeeded, Failed, Error)
* Bitbucket Server and Bitbucket Cloud
* JetBrains Space
* JetBrains Upsource
* Gerrit Code Review tool 2.6+
* Perforce Helix Swarm

Starting from version 2022.04, Commit Status Publisher updates the commit status in the version control system as soon as 
the build is added to the queue, providing you with the most up-to-date information.
GitHub, GitLab, Space, Bitbucket Server and Bitbucket Cloud, Perforce Helix Swarm, and Azure DevOps are supported.

>See our **video guide** on how to [send build information to external systems](https://www.youtube.com/watch?v=o0oj7mOcNvc).

## Provider-specific Configuration

### GitHub

Commit Status Publisher supports the GitHub URL in the following format:
* For GitHub.com: `https://api.github.com`
* For GitHub Enterprise: `http[s]://<host>[:<port>]/api/v3`

For connection, select one of the available authentication types:
* **Access Token** — use a personal access token or obtain a token through an OAuth connection. The token must have the following scopes:
    * for public repositories: `public_repo` and `repo:status`
    * for private repositories: `repo`
  
  If you have a [configured OAuth connection](configuring-connections.md#GitHub) to GitHub, you can click the magic wand button to let TeamCity automatically retrieve the corresponding access token.
  
  <img src="dk-CSP-GitHubToken.png" width="708" alt="Acquire access token for GitHub"/>
* **Password** — Provide the GitHub username and password.

Note that the password authentication will not work if connecting to a GitHub Enterprise repository or if the user's GitHub account is protected with a two-factor authentication. In these cases, use an access token instead.

>To protect a branch and ensure that only verified pull requests are merged into it, you can create a [branch protection rule](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/defining-the-mergeability-of-pull-requests/managing-a-branch-protection-rule) in your GitHub repository settings. If you set a TeamCity build as a required status check, GitHub will not allow a pull request to be merged until the build on requested changes finishes successfully. 


### GitLab

If you use a recent version of GitLab (\<= 9.0), it is recommended to use the GitLab URL of the following format: `http[s]://<hostname>[:<port>]/api/v4` as GitLab [stops supporting](https://about.gitlab.com/2018/01/22/gitlab-10-4-released/#api-v3) the v3 API in GitLab 11. If you have `/api/v3` in your current TeamCity configurations, they may stop working with GitLab 11+, so consider changing the server URL to `api/v4`.

For older versions of GitLab, use the GitLab URL of the format `http[s]://<hostname>[:<port>]/api/v3`.

The GitLab credentials for Commit Status Publisher must belong to a user with a Developer, Maintainer, or Owner role for the project. In addition, to change a commit status for a protected branch, the GitLab user must be included in the **Allowed to push** list.

### Bitbucket Cloud

To be able to connect to Bitbucket Cloud, make sure the [TeamCity server URL](configuring-server-url.md) is a fully qualified domain name (FQDN): for example, [`http://myteamcity.domain.com:8111`](http://myteamcity.domain.com:8111){nullable="true"}. Short names, such as [`http://myteamcity:8111`](http://myteamcity:8111){nullable="true"}, are rejected by the Bitbucket API.
{product="tc"}

In the Commit Status Publisher settings, specify a username and [app password](https://support.atlassian.com/bitbucket-cloud/docs/app-passwords/) for authentication. For Bitbucket Cloud team accounts, it is possible to use the team name as the username and the API key as the password.

If you switch the **Authentication Type** to "Access Token", TeamCity will display a list of [configured OAuth connections](configuring-connections.md#Bitbucket+Cloud) to Bitbucket Cloud. Click the **Acquire** button next to a required connection to obtain an access token.

<img src="dk-CSP-BBCloudToken.png" width="708" alt="Acquire access token for Bitbucket Cloud"/>

### Bitbucket Server

Commit Status Publisher supports the Bitbucket Server URL in the following format: `http[s]://<hostname>:<port>`. 
Besides the URL, you need to specify a username and password or an access token for authentication.

When you select the **Access Token** authentication type, TeamCity will display
a list of [configured OAuth connections](configuring-connections.md#Bitbucket+Server+and+Data+Center)
to Bitbucket Server / Data Center configured in the project and available to all project users.
If a token is already configured, TeamCity will display the information about the user that obtained the token and the connection that provided the token.

To obtain the token for the current user, click the **Acquire** button. 
If you are not signed in to your Bitbucket Server / Data Center account, TeamCity will ask for access to it.
After you sign in, you will be able to acquire a token for the required connection. 
TeamCity will update the token information for the connection.

<img src="dk-CSP-BBServerToken.png" width="708" alt="Acquire access token for Bitbucket Server"/>


To protect a branch and ensure that only verified pull requests are merged into it, you can specify [required builds](https://confluence.atlassian.com/bitbucketserver/checks-for-merging-pull-requests-776640039.html#Checksformergingpullrequests-Requiredbuildsmergecheck) in your Bitbucket repository settings. To set a TeamCity build as a _required build_, open the __Add required builds__ page in Bitbucket and specify a build configuration ID as a build key in the __Add builds__ field. In this case, Bitbucket will not allow a pull request to be merged until the build on requested changes finishes successfully.

>The _TeamCity Integration for Bitbucket_ app made by Stiltsoft provides a more detailed preview of TeamCity builds in the Bitbucket UI and lets you run them without switching to TeamCity. Read more details about the app in [this post](https://blog.jetbrains.com/teamcity/2021/05/run-and-view-teamcity-builds-from-bitbucket/).

### Azure DevOps

Personal access tokens can be used for authentication. If a [VSTS connection](configuring-connections.md#Azure+DevOps+PAT+Connection) is configured, the personal access token can be automatically filled from the project connection.

You can create a [personal access token](https://www.visualstudio.com/en-us/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) in your Azure DevOps account. Set the _Code_ access scope to _Code (status)_ in the repositories you are about to send statuses to from TeamCity.`

### JetBrains Space

To establish integration with [JetBrains Space](https://www.jetbrains.com/space/), you need to create a predefined connection to it, as described [here](configuring-connections.md#jetbrains-space-connection).

Then, in the build configuration's settings:
1. Open __Build Features__ and add the _Commit status publisher_ build feature.
2. Select the _JetBrains Space_ publisher and the created connection.
3. Specify the name that will be displayed for this service in Space.
4. Save the settings.

Now, whenever you run a build in this configuration, TeamCity will report the build status to JetBrains Space.

### Perforce Helix Swarm

If a build is run on changes in Perforce [shelved files](https://www.perforce.com/manuals/v17.1/p4guide/Content/CmdRef/p4_shelve.html), TeamCity can report its statuses as comments to the respective code review in Perforce Helix Swarm.

In the Commit Status Publisher settings, specify:
* A repository to use for publishing a build status. Choose the default **All attached VCS roots** option if you wish to publish statuses for commits in all attached VCS roots handled by a single Swarm server.
>When multiple Swarm servers are used in the scope of a single build configuration, the option "All attached VCS roots" cannot be used. The VCS Root must be specified explicitly for each Swarm configuration.
* Your Helix Swarm server's URL.
* Username and cross-host [ticket](https://www.perforce.com/manuals/swarm/Content/Swarm/setup.swarm.html) for connection (use the `p4 login -a -p` command to obtain one)
* Whether you want TeamCity to create a test run on the Swarm server and update its status according to the build status in TeamCity. To be able to use this mode, make sure the provided credentials grant administrative access.

Note that Helix Swarm usually creates reviews on shelved changelists whose description contains a special keyword, depending on your setup (for example, `#review`). If you want TeamCity to trigger builds on Perforce shelved files automatically, you need to specify the same keyword in the [Perforce Shelve Trigger](perforce-shelve-trigger.md) settings as well.

>To get notified about the events, make sure to [configure Swarm triggers](https://www.perforce.com/manuals/swarm-admin/Content/Swarm/setup.perforce.html).

After running a build with Commit Status Publisher on a changelist that has a review in Helix Swarm, 
TeamCity shows the Swarm Reviews section on the build overview page. From each change, 
you can navigate to the change page on the Helix Swarm using `Open in Helix Swarm`.
This works for personal builds on shelved changelists, as well as builds on post-commit reviews: the Swarm integration finds reviews associated with the final revision of the build.

### Gerrit

Commit Status Publisher supports Gerrit versions 2.6+. For configuring integration with earlier Gerrit versions, contact our [support](feedback.md).

## Using Commit Status Publisher

1. [Add the build feature](adding-build-features.md) to your build configuration.
2. Use the default _All attached VCS roots_ option if you want Commit Status Publisher to attempt publishing statuses for commits in all attached VCS roots or select a single repository for publishing build statuses.
3. Select your system as the publisher and specify its connection details and credentials.
4. Test the connection
5. Save your settings.

__Example: Configuring Pull Requests Status Publishing to GitHub__

The example below demonstrates how to configure sending the status of builds with changes included in your pull request from TeamCity to GitHub.

1. Use [pull requests build feature](pull-requests.md) to configure pull requests branches. Alternatively you can make the branches available by configuring the [branch specification](working-with-feature-branches.md) in your VCS Root while ensuring that it includes pull requests branches (see also a related [blog post](http://blog.jetbrains.com/teamcity/2013/02/automatically-building-pull-requests-from-github-with-teamcity/)).
2. [Add](adding-build-features.md) the Commit Status Publisher build feature:
   * Use the default __All attached VCS roots__ option to publish statuses for commits in all attached VCS roots
   * Select GitHub as the publisher and specify its connection details and credentials and test the connection: <img src="CommitStatusPublisher.png" width="556" alt="Testing connection to GitHub"/>
3. Save your settings.
4. Commit changes to your source code and create a pull request in GitHub, then run a build with your changes in TeamCity. The Commit Status Publisher will inform you on the status of the build with your pull request changes:
   * It will show you whether the check is:
     * in progress ![progress.png](progress.png)
     * failed ![Failed.png](Failed.png)
     * successful ![Successful.png](Successful.png)
   * hovering over the commit status will display the build summary
   * clicking the build status icon or the _Details_ link will open the [build results](working-with-build-results.md) page in TeamCity. This information is also available on the __Commits__ tab of your pull request details.   
   Similarly to the previous page, clicking the build status icon opens the [build results](working-with-build-results.md) page in the TeamCity UI: 
   
    <img src="BuildResults.PNG" width="1054" alt="Build results"/>
    
## Using Commit Status Publisher with VCS checkout rules

If a build's VCS root has [checkout rules](configuring-vcs-settings.md#Configure+Checkout+Rules) configured, Commit Status Publisher will consider only commits that conform to these rules. That is, if the last commit made before the build start does not satisfy the checkout rules, it will not be labeled with the build status; the status will be displayed next to the last satisfying commit instead.

If you need to display the build status next to the last commit of the build (for example, in a pull request), you can adjust the checkout rules so this commit is included into the VCS root's scope. Or, if this is a recurrent issue, consider rearranging your build chain as follows:
1. Configure the main build with checkout rules.
2. Configure an utility [composite build](composite-build-configuration.md) without build steps and checkout rules but with the Commit Status Publisher feature.
3. In the composite build, configure a [snapshot dependency](snapshot-dependencies.md) on the main build.

In the scope of such a chain, Commit Status Publisher will not be bound by the checkout rules and the build status will be displayed next to the very last commit.
