[//]: # (title: Commit Status Publisher)
[//]: # (auxiliary-id: Commit Status Publisher)
Commit status publisher is a [build feature](adding-build-features.md) which allows TeamCity to automatically send build statuses of your commits to an external system. The feature is implemented as an [open-source plugin](https://github.com/JetBrains/commit-status-publisher) bundled with TeamCity.

The supported systems are:
* JetBrains Upsource
* GitHub (the build statuses for pull requests are supported as well)
* GitLab
* TFS/VSTS\-hosted Git (supported statuses: Pending, Succeeded, Failed, Error)
* Atlassian Bitbucket Server (formerly Stash) and Atlassian Bitbucket Cloud
* Gerrit Code Review tool 2.6\+
 

<note>

__GitLab__

If you use a recent version of GitLab (&gt;= 9.0), it is recommended to use the GitLab URL of the following format: `http[s]://<hostname>[:<port>]/api/v4` as GitLab will [stop supporting](https://about.gitlab.com/2018/01/22/gitlab-10-4-released/#api-v3) the v3 API in GitLab 11. If you have `/api/v3` in your current TeamCity configurations, they may stop working with GitLab 11\+, so consider changing the server URL to `api/v4`.

For older versions of GitLab, use the GitLab URL of the format `http[s]://<hostname>[:<port>]/api/v3`.
</note>

<note>

__Bitbucket__

Make sure that the [TeamCity server URL](configuring-server-url.md) is FQDN, for example, [`http://myteamcity.domain.com:8111`](http://myteamcity.domain.com:8111). Short names, for example, [`http://myteamcity:8111`](http://myteamcity:8111) are rejected by the Bitbucket API.   
For Bitbucket Cloud team accounts, it is possible to use the team name as the username, and the API key as the password.

</note>

<note>

__Gerrit__

Commit Status Publisher in TeamCity 2018.1 supports Gerrit versions 2.6\+. For configuring integration with earlier Gerrit versions please contact our [support](https://confluence.jetbrains.com/display/TW/Feedback).

</note>

<note>

__TFS/VSTS__

Personal Access Tokens can be used for authentication. If a [VSTS connection](integrating-teamcity-with-vcs-hosting-services.md#Connecting+to+Visual+Studio+Team+Services) is configured, the personal access token can be automatically filled from the project connection.

</note>

To use the tool:
1. [Add the build feature](adding-build-features.md) to your build configuration.
2. Use the default __All attached VCS roots__ option if you want Commit Status Publisher to attempt publishing statuses for commits in all attached VCS roots or select a single repository for publishing build statuses.
3. Select your system as the publisher, and specify its connection details and credentials.
4. Test the connection
5. Save your settings.

See the example below to configure sending the status of builds with changes included in your pull request from TeamCity to GitHub.

1. Configure the [branch specification](working-with-feature-branches.md) in your VCS Root ensuring that it includes pull requests. Detailed information is available in the Branch specification section of this TeamCity [blog post](http://blog.jetbrains.com/teamcity/2013/02/automatically-building-pull-requests-from-github-with-teamcity/).
2. [Add the build feature](adding-build-features.md):
   * Use the default __All attached VCS roots__ option to publish statuses for commits in all attached VCS roots
   * Select GitHub as the publisher and specify its connection details and credentials and test the connection:   
   
   <img src="CommitStatusPublisher.png" width="556" alt="Testing connection to GitHub"/>
   
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
      