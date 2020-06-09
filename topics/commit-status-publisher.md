[//]: # (title: Commit Status Publisher)
[//]: # (auxiliary-id: Commit Status Publisher)

Commit Status Publisher is a [build feature](adding-build-features.md) which allows TeamCity to automatically send build statuses of your commits to an external system. The feature is implemented as an [open-source plugin](https://github.com/JetBrains/commit-status-publisher) bundled with TeamCity.

The supported systems are:
* JetBrains Upsource
* GitHub (the build statuses for pull requests are supported as well)
* GitLab
* TFS/VSTS-hosted Git (supported statuses: Pending, Succeeded, Failed, Error)
* Atlassian Bitbucket Server (formerly Stash) and Atlassian Bitbucket Cloud
* Gerrit Code Review tool 2.6+
 
## Provider-specific Configuration

### GitHub

Commit Status Publisher supports the GitHub URL in the following format:
* For GitHub.com: `https://api.github.com`
* For GitHub Enterprise: `http[s]://<host>[:<port>]/api/v3`

For connection, select one of the available authentication types:
* _Access Token_   
Use a personal access token or obtain a token through an OAuth connection. The token must have the following scopes:
   * for public repositories: `public_repo` and `repo:status`
   * for private repositories: `repo`
* _Password_   
Provide the GitHub username and password.

Note that the password authentication will not work if connecting to a GitHub Enterprise repository or if the user's GitHub account is protected with a two-factor authentication. In these cases, use an access token instead.

### GitLab

If you use a recent version of GitLab (&gt;= 9.0), it is recommended to use the GitLab URL of the following format: `http[s]://<hostname>[:<port>]/api/v4` as GitLab [stops supporting](https://about.gitlab.com/2018/01/22/gitlab-10-4-released/#api-v3) the v3 API in GitLab 11. If you have `/api/v3` in your current TeamCity configurations, they may stop working with GitLab 11+, so consider changing the server URL to `api/v4`.

For older versions of GitLab, use the GitLab URL of the format `http[s]://<hostname>[:<port>]/api/v3`.

### Bitbucket Cloud

To be able to connect to Bitbucket Cloud, make sure the [TeamCity server URL](configuring-server-url.md) is a fully qualified domain name (FQDN): for example, [`http://myteamcity.domain.com:8111`](http://myteamcity.domain.com:8111). Short names, such as [`http://myteamcity:8111`](http://myteamcity:8111), are rejected by the Bitbucket API.

In the Commit Status Publisher settings, specify a username and password for authentication. For Bitbucket Cloud team accounts, it is possible to use the team name as the username and the API key as the password.

### Bitbucket Server

Commit Status Publisher supports the Bitbucket Server URL in the following format: `http[s]://<hostname>:<port>`. Apart from the URL, you need to specify a username and password for authentication.

### Gerrit

Commit Status Publisher in TeamCity 2018.1 and later supports Gerrit versions 2.6+. For configuring integration with earlier Gerrit versions, contact our [support](https://confluence.jetbrains.com/display/TW/Feedback).


### TFS/VSTS

<note>

In 2019, Visual Studio Team Services and Team Foundation Server have been renamed to Azure DevOps Services and Azure DevOps Server.

</note>

Personal Access Tokens can be used for authentication. If a [VSTS connection](integrating-teamcity-with-vcs-hosting-services.md#Connecting+to+Azure+DevOps+Services) is configured, the personal access token can be automatically filled from the project connection.

## Using Commit Status Publisher

1. [Add the build feature](adding-build-features.md) to your build configuration.
2. Use the default _All attached VCS roots_ option if you want Commit Status Publisher to attempt publishing statuses for commits in all attached VCS roots or select a single repository for publishing build statuses.
3. Select your system as the publisher and specify its connection details and credentials.
4. Test the connection
5. Save your settings.

## Example: Configuring Pull Requests Status Publishing to GitHub

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
   * clicking the build status sign or the Details link will open the [build results](working-with-build-results.md) page in TeamCity:   
   
   <img src="InfoonHover.png" width="550" alt="Pull Requests | Conversation"/>
    
   This information is also available on the Commits tab of your pull request details:   
   
   <img src="CommitsTab.png" width="600" alt="Pull Requests | Commits"/>
      
   Similarly to the previous page, clicking the build status icon opens the [build results](working-with-build-results.md) page in the TeamCity web UI: 
   
    <img src="BuildResults.PNG" width="1054" alt="Build results"/>


__ __
