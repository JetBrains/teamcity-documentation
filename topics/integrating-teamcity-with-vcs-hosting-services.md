[//]: # (title: Integrating TeamCity with VCS Hosting Services)
[//]: # (auxiliary-id: Integrating TeamCity with VCS Hosting Services)

If you have an organization account in [GitHub](https://github.com/), [GitHub Enterprise](https://enterprise.github.com/), [Bitbucket Cloud](https://bitbucket.org/), [GitLab.com](https://about.gitlab.com/), or [GitLab CE/EE](https://about.gitlab.com/install/ce-or-ee/), you can connect TeamCity to these source code hosting services making it easier for the organization users to create new projects, [Git](https://confluence.jetbrains.com/display/TCD10/Git) or [Mercurial](https://confluence.jetbrains.com/display/TCD10/Mercurial) VCS roots, [GitHub](https://confluence.jetbrains.com/display/TCD10/GitHub) or [Bitbucket](https://confluence.jetbrains.com/display/TCD10/Bitbucket) issue tracker, which are now now supported out of the box.

It is also possible to connect TeamCity to [Azure DevOps Services](https://visualstudio.microsoft.com/team-services/) making it really simple to set up projects which use VSTS repositories or issue tracker.

## Configuring Connections

Connections are created on the project level, a configured connection is accessible in the current project and in all of its subprojects. If you use global VCS hosting services like GitHub or Bitbucket Cloud, it makes sense to configure a single connection for the Root project. Or your organization administrator can create a parent project and a configure connection to GitHub there once, and the users will see a list of GitHub repositories URLs in the TeamCity web UI, which will make setting up subprojects a lot simpler for them.  

Connections are configured on the __Project Administration | Connections__ page. 

<img src="projectConnections.png" width="700" alt="Adding a GitHub connection"/>

You need to register your TeamCity application in your VCS hosting service using the information provided by TeamCity, enter the access details provided by the service in the TeamCity form, and log in to your VCS hosting service from TeamCity to authorize the TeamCity application in the VCS. See details below.

### Connecting to GitHub

You need to configure a connection to your GitHub repository to create a [project from URL](creating-and-editing-projects.md#Creating+project+pointing+to+repository+URL), create a [VCS root from URL](guess-settings-from-repository-url.md), create a [Git](git.md) VCS root or create [GitHub](github.md) issue tracker.

To configure a GitHub connection:
1. In __Project Administration | Connections__, click __Add Connection__.
2. Select _GitHub_ as the connection type. The page that opens provides the parameters to be used when registering your TeamCity application in GitHub service.
3. Click the _register application_ link. The GitHub page opens.   
You need to register TeamCity as an [OAuth application](https://developer.github.com/v3/oauth/) in GitHub.   
The following steps are performed in your GitHub account:
   * Log into your GitHub account. On the Register a new OAuth application page specify the name and an optional description, the homepage URL and the callback URL as provided by TeamCity.
   * Click __Register application__.  The page is updated with Client ID and the client secret information for your TeamCity application. 
4. Continue configuring the connection in TeamCity: on the Add Connection page that is open, specify the Client ID and the client secret.
5. Save the connection settings. 
6. The connection is configured, and now a small GitHub icon becomes active in several places where a repository URL can be specified: [create project from URL](creating-and-editing-projects.md#Creating+project+pointing+to+repository+URL), [create VCS root from URL](guess-settings-from-repository-url.md), create [Git](git.md) VCS root, create [GitHub](github.md) issue tracker. Click the icon, log in to GitHub and authorize TeamCity. The authorized application will be granted full control of private repositories and the _Write repository hooks_ permission.

### Connecting to Bitbucket

You need to configure a connection to your public Bitbucket repository to create a [project from URL](creating-and-editing-projects.md#Creating+project+pointing+to+repository+URL), create a [VCS root from URL](guess-settings-from-repository-url.md), create a [Mercurial](mercurial.md) VCS root, or create a [Bitbucket](bitbucket-cloud.md) issue tracker.

To configure a Bitbucket connection:
1. In __Project Administration | Connections__, click __Add Connection__.
2. Select _Bitbucket_ as the connection type.   
The page that opens provides the parameters to be used when registering an OAuth consumer on Bitbucket Cloud. Click the _register application_ link.   
You need to create an [OAuth consumer](https://confluence.atlassian.com/bitbucket/oauth-on-bitbucket-cloud-238027431.html#OAuthonBitbucketCloud-Createaconsumer) on Bitbucket Cloud. The following steps are performed in your Bitbucket account:
   1. Log into your Bitbucket account, click your avatar and select __Bitbucket settings__ from the menu. The Account page appears.
   2. Click __OAuth__ from the menu bar. On the __Add OAuth consumer__ page, specify the name and an optional description, the callback URL and the URL as provided by TeamCity.
   3. Specify the set of permissions: TeamCity requires "_read_" access to your account and your repositories.
   4. Save your settings.
   5. On the page that opens, in the __OAth consumers__ section, click the name of your TeamCity application to display the key and the secret.
3. Continue configuring the connection in TeamCity: on the __Add Connection__ page that is open, specify the key and secret.
4. Save the connection settings. 
5. The connection is configured, and now a small Bitbucket icon becomes active in several places where a repository URL can be specified: [create project from URL](creating-and-editing-projects.md#Creating+project+pointing+to+repository+URL), [create VCS root from URL](guess-settings-from-repository-url.md), create [Mercurial](mercurial.md) VCS root, create [Bitbucket](bitbucket-cloud.md) issue tracker. Click the icon, log in to Bitbucket  and authorize TeamCity. TeamCity will be granted access to your public repositories. For __private__ repositories you'll still have to sign in to Bitbucket as it doesn't provide non\-expiring access tokens. See the [related discussion](https://bitbucket.org/site/master/issues/11774/application-specific-passwords-or-tokens).

### Connecting to GitLab

You need to configure a connection to your public GitLab repository to create a [project from URL](creating-and-editing-projects.md#Creating+project+pointing+to+repository+URL) or create a [VCS root from URL](guess-settings-from-repository-url.md).

To configure a GitLab connection in TeamCity:
1. In __Project Administration | Connections__, click __Add Connection__.
2. Select _GitLab.com_ or _GitLab CE/EE_ as the connection type.
3. TeamCity will display the _Redirect URL_ required for registering an OAuth application in GitLab.   
To register the application, follow [this instruction](https://docs.gitlab.com/ee/integration/oauth_provider.html). Insert the TeamCity _Redirect URL_ as _Redirect URI_ and select the `api` [scope](https://docs.gitlab.com/ee/integration/oauth_provider.html#authorized-applications). Save your application, and copy the __secret__ and __application ID__ generated by GitLab.
4. Once ready, go back to configuring the connection in TeamCity and fill in the following fields:
   * _Application ID_
   * _Secret_
   * _Server URL_ (only for GitLab CE/EE)
5. Save the connection settings.
6. The connection is configured, and now a small GitLab icon becomes active in several places where a repository URL can be specified: [create project from URL](creating-and-editing-projects.md#Creating+project+pointing+to+repository+URL) and [create VCS root from URL](guess-settings-from-repository-url.md). Click the icon, sign in to GitLab, and authorize TeamCity. TeamCity will be granted access to your repositories.

<note>

When creating a VCS root URL for GitLab, note that TeamCity will not extract credentials from the root URL, so you need to specify a username and password manually.

</note>

### Connecting to Azure DevOps Services

You can configure a connection to your Azure DevOps Services (or Azure DevOps Server, formerly Team Foundation Server) to create a [project from URL](creating-and-editing-projects.md#Creating+project+pointing+to+repository+URL), create a [VCS root from URL](guess-settings-from-repository-url.md), create [TFS](team-foundation-server.md) VCS root, or create [Team Foundation Work Items](team-foundation-work-items.md) tracker.

To configure a connection to Azure DevOps Services, follow these steps:
1. In __Project Administration | Connections__, click __Add Connection__.
2. Select _Azure DevOps Services_ as the connection type.   
The page that opens provides the parameters to be used when connecting TeamCity to Azure DevOps Services.
3. Log in to your Azure DevOps Services account to create a personal access token with _All scopes_ as described in the [Microsoft documentation](https://www.visualstudio.com/en-us/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate).
4. Continue configuring the connection in TeamCity: on the __Add Connection__ page that is open, specify
   * the server URL in the `https://{account}.visualstudio.com` format or your Team Foundation Server web portal as `https://{server}:8080/tfs/`
   * your personal access token
5. Save the connection settings. 
6. The connection is configured, and now a small Azure DevOps Services icon becomes active in several places where a repository URL can be specified: [create project from URL](creating-and-editing-projects.md#Creating+project+pointing+to+repository+URL), [create VCS root from URL](guess-settings-from-repository-url.md), create [TFS](team-foundation-server.md) VCS root, create [Team Foundation Work Items](team-foundation-work-items.md) tracker. Click the icon, log in to Azure DevOps Services and authorize TeamCity. TeamCity will be granted full access to all of the resources that are available to you.   
__Since TeamCity 2017.2 EAP1__, when configuring Commit Status Publisher for Git repositories hosted in TFS/VSTS, the personal access token can be filled out automatically if a VSTS project connection is configured.

<tip>

 It is possible to configure several VSTS connections. In this case the server URL will be displayed next to the VSTS icon to distinguish the server in use. 
</tip>



## Creating Entities from URL in TeamCity

Now creating entities from a URL in TeamCity is extremely easy: on clicking the GitHub, Bitbucket, GitLab, or VSTS icon, the list of repositories available to the current user is displayed (note that only public Bitbucket repositories will be available via the configured connection):

<img src="ProjectConnectionBitbucket.png" width="587" alt="Available Bitbucket repositories"/>

You can select the URL and proceed with the configuration.

__ __