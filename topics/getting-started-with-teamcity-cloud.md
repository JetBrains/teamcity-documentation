[//]: # (title: Getting Started with TeamCity Cloud)
[//]: # (auxiliary-id: Getting Started with TeamCity Cloud)

TeamCity is a CI/CD server which key features are a powerful toolset and universality. With our Cloud version, we address the user demand in the full-featured CI/CD solution and make it available to you in a couple of minutes, with no need to maintain a server on-premises.

__If you are new to CI/CD or TeamCity__, the Cloud is a great starting point as it automatically resolves the task of installing and configuring the server.

When you register a TeamCity Cloud account, your own TeamCity server is automatically created in Amazon Web Services. The server operates on the newest version of TeamCity and currently provides Windows and Linux cloud build agents out of the box (self-hosted agents for Windows, Linux, and macOS are also supported).

## 1. Learn about CI with TeamCity Cloud

Understand the idea behind continuous integration, learn [basic TeamCity concepts and build lifecycle](continuous-integration-with-teamcity.md).

## 2. Start TeamCity

To start TeamCity in cloud, __[register an account](https://www.jetbrains.com/teamcity/signup/)__. In a matter of seconds, your server will be available under the `teamcity.com` domain.

After the server is ready, an invitation link will be sent to your email. Proceed via this link to get to your administrative account. Everything is ready to build!

## 3. Run your First Build

Create your [first project](configure-and-run-your-first-build.md) in TeamCity Cloud and configure and run your first build.

## Differences Between TeamCity Cloud and On-Premises

Users of our Cloud and On-Premises versions can expect a similar level of scalability and universality of these solutions. However, the Cloud version is automatically configured and maintained by TeamCity and thus provides limited server administration settings comparing to our [On-Premises](https://www.jetbrains.com/teamcity/) solution.

TeamCity Cloud has the following limitations compared to On-Premises:
* Limited server configuration and diagnostics. See the related pages in the On-Premises documentation: [Installing and Configuring Server](https://www.jetbrains.com/help/teamcity/installing-and-configuring-the-teamcity-server.html) and [Monitoring and Diagnostics](https://www.jetbrains.com/help/teamcity/teamcity-monitoring-and-diagnostics.html).
* TeamCity Cloud data is backed up and cleaned up automatically. The set of available configuration options may differ from the On-Premises installations. See the related pages in the On-Premises documentation: [Data Backup](https://www.jetbrains.com/help/teamcity/teamcity-data-backup.html) and [Clean-Up](https://www.jetbrains.com/help/teamcity/clean-up.html).
* Some settings are unavailable to TeamCity Cloud administrators: for example, changing the location for [storing external artifacts](https://www.jetbrains.com/help/teamcity/configuring-artifacts-storage.html#external-artifacts-storage).
* No plugin management. The following bundled plugins are currently disabled:
    * LDAP support
    * Microsoft Windows Domain authentication
    * VCS Support: CVS
    * VCS Support: StarTeam
    * Build Agent JVM updater
    * NuGet Support
    * Search

If you are interested in our On-Premises solution, you can visit its [website](https://www.jetbrains.com/teamcity/) or [documentation](https://www.jetbrains.com/help/teamcity/teamcity-documentation.html).

Comparing to On-Premises, TeamCity Cloud offers the following new features:
* To ensure secure agent-server connection, you can easily generate and preconfigure authentication tokens for self-hosted build agents.
* If you authenticate via GitHub.com, GitLab.com, or Bitbucket Cloud, the respective [connection](configuring-connections.md) will be preconfigured automatically.
* The __Administration | Invitations__ page allows automatically inviting users to the server. An invited user will be able to register a new user account or authenticate via GitHub, GitLab, or Bitbucket.
* The following plugins are bundled and enabled in TeamCity Cloud:
  * [Unity Support](https://plugins.jetbrains.com/plugin/11453-unity-support) for building Unity projects
  * [GitHub Commit Hooks](https://plugins.jetbrains.com/plugin/9179-github-commit-hooks) to easily install GitHub webhooks via the TeamCity UI
  * [Caches Cleanup](https://github.com/JetBrains/teamcity-caches-cleanup-plugin) helps easily free disk space
  * [Octopus Deploy integration](https://plugins.jetbrains.com/plugin/9038-octopus-deploy-integration) for triggering Octopus Deploy deployments after a build

All the listed features will be introduced in our On-Premises version in the nearest future.
