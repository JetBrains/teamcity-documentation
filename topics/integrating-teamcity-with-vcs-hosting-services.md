[//]: # (title: Integrating TeamCity with VCS Hosting Services)
[//]: # (auxiliary-id: Integrating TeamCity with VCS Hosting Services)

If you have an organization account in [GitHub.com](https://github.com/), [GitHub Enterprise](https://enterprise.github.com/), [Bitbucket Cloud](https://bitbucket.org/), [GitLab.com](https://about.gitlab.com/), or [GitLab CE/EE](https://about.gitlab.com/install/ce-or-ee/), you can connect TeamCity to these source code hosting services. This makes it easier for your organization's users to create new projects with [Git](git.md) or [Mercurial](mercurial.md) VCS roots and [GitHub](github.md) or [Bitbucket](integrating-teamcity-with-issue-tracker.md) issue trackers, which are supported out of the box.

If your team uses VSTS repositories or issue tracking, you can connect TeamCity to [Azure DevOps Services](https://visualstudio.microsoft.com/team-services/) for fast project setup.

>In TeamCity Cloud, connections to GitHub.com, GitLab.com, and Bitbucket Cloud are already predefined in the Root project settings, which makes them available in all the other projects.
>
{product="tcc"}

## Configuring Connections

Connections are created on the project level, which means that a configured connection is accessible in the current project and in all of its subprojects. If you use global VCS hosting services like GitHub or Bitbucket Cloud, it makes sense to configure a single connection for the Root project. Alternatively, your organization administrator can create a parent project where they configure the connection to GitHub. When setting up subprojects in the TeamCity web UI, users can just pick from a list of connected GitHub repository URLs.

To configure connections, go to the __Project Administration | Connections__ page. 

<img src="projectConnections.png" width="700" alt="Adding a GitHub connection"/>

You will need to complete three basic steps:
* Register your TeamCity application in your VCS hosting service using the information provided by TeamCity.
* Get the access details from the service and enter them in the TeamCity form.
* Log in to your VCS hosting service from TeamCity to authorize the TeamCity application in the VCS. 

See the details for each hosting service below.

### Connecting to GitHub

>In TeamCity Cloud, a connection to GitHub.com is already predefined in the Root project settings, which makes it available in all the other projects.
>
{product="tcc"}

You will need a GitHub repository connection to create a [project from a GitHub URL](creating-and-editing-projects.md#Creating+project+pointing+to+repository+URL), create a [VCS root from a URL](guess-settings-from-repository-url.md), create a [Git](git.md) VCS root, create a [GitHub](github.md) issue tracker, or [enable GitHub authentication](configuring-authentication-settings.md#GitHub.com).

To configure a GitHub connection:
1. In __Project Administration | Connections__, click __Add Connection__.
2. Select _GitHub.com_ or _GitHub Enterprise_ as the connection type. On the page that opens, copy the parameters to use for registering your TeamCity application in GitHub.
3. Click the _register application_ link. The GitHub page opens.   
You need to register TeamCity as an [OAuth application](https://docs.github.com/en/developers/apps/authorizing-oauth-apps) in GitHub.   
Perform the following steps in your GitHub account:
   1. Sign in to your GitHub account. 
   2. On the __Register a new OAuth application__ page, specify the name and an optional description, the homepage URL, and the callback URL provided by TeamCity.
   3. Click __Register application__. The page displays the client ID and the client secret for your TeamCity application. Copy them to use in the next step. 
   >If you use the [GitHub authentication](configuring-authentication-settings.md#GitHub.com) module and want to restrict access to TeamCity to users of specific GitHub organizations, you need to ensure that your OAuth app is allowed by all these organizations. By default, GitHub does not allow OAuth apps to access the organizations. You can either disable this restriction for all apps or approve only the TeamCity app in each of the organizations. For more information, see the [GitHub documentation](https://docs.github.com/en/github/setting-up-and-managing-organizations-and-teams/about-oauth-app-access-restrictions).
4. Continue configuring the connection in TeamCity. On the __Add Connection__ page, specify:   
      * _Client ID_
      * _Client secret_
      * _Server URL_ (only for GitHub Enterprise)
5. Save the connection settings. Now the connection is configured and you will see a small GitHub icon in several places where a repository URL can be specified: [create project from URL](creating-and-editing-projects.md#Creating+project+pointing+to+repository+URL), [create VCS root from URL](guess-settings-from-repository-url.md), [create Git VCS root](git.md), and [create GitHub issue tracker](github.md).
6. Click the icon, log in to GitHub, and authorize TeamCity. TeamCity will be granted full control of private repositories and assigned the _Write repository hooks_ permission.

>If your GitHub Enterprise server uses HTTPS, you need to also [upload its HTTPS certificate](uploading-ssl-certificates.md).

### Connecting to Bitbucket Cloud

>In TeamCity Cloud, a connection to Bitbucket Cloud is already predefined in the Root project settings, which makes it available in all the other projects.
>
{product="tcc"}

You will need a Bitbucket Cloud connection to create a [project from a Bitbucket URL](creating-and-editing-projects.md#Creating+project+pointing+to+repository+URL), create a [VCS root from a URL](guess-settings-from-repository-url.md), create a [Mercurial](mercurial.md) VCS root, create a [Bitbucket](bitbucket-cloud.md) issue tracker, or [enable BitBucket Cloud authentication](configuring-authentication-settings.md#Bitbucket+Cloud).

To configure a Bitbucket Cloud connection:
1. In __Project Administration | Connections__, click __Add Connection__.
2. Select _Bitbucket_ as the connection type. On the page that opens, copy the parameters to use for registering an OAuth consumer on Bitbucket Cloud. 
3. Click the _register application_ link.   
You need to create an [OAuth consumer](https://confluence.atlassian.com/bitbucket/oauth-on-bitbucket-cloud-238027431.html#OAuthonBitbucketCloud-Createaconsumer) on Bitbucket Cloud. Perform the following steps in your Bitbucket account:
   1. Log in to your Bitbucket account. 
   2. Click your avatar and select __Bitbucket settings__ from the menu. The __Account__ page opens.
   3. Choose __OAuth__ from the menu bar. On the __Add OAuth consumer__ page, specify the name and an optional description, the callback URL, and the URL provided by TeamCity.
   4. Specify the set of permissions. TeamCity requires "_read_" access to your account and your repositories.
   5. Save your settings.
   6. On the page that opens, go to the __OAuth consumers__ section and click the name of your TeamCity application to display the key and the secret.
4. Continue configuring the connection in TeamCity. On the __Add Connection__ page, specify the key and secret from Bitbucket.
5. Save the connection settings. Now the connection is configured and you will see a small Bitbucket icon in several places where a repository URL can be specified: [create project from URL](creating-and-editing-projects.md#Creating+project+pointing+to+repository+URL), [create VCS root from URL](guess-settings-from-repository-url.md), [create Mercurial VCS root](mercurial.md), and [create Bitbucket issue tracker](bitbucket-cloud.md).
6. Click the icon, log in to Bitbucket, and authorize TeamCity. TeamCity will be granted access to your public repositories. For __private__ repositories you'll still have to provide Bitbucket credentials to be used for authentication by TeamCity. This is because Bitbucket Cloud doesn't provide non-expiring access tokens — see the [related discussion](https://bitbucket.org/site/master/issues/11774/application-specific-passwords-or-tokens).

### Connecting to GitLab

>In TeamCity Cloud, a connection to GitLab.com is already predefined in the Root project's settings, which makes it available in all the other projects.
>
{product="tcc"}

You will need a GitLab connection to create a [project from a GitLab URL](creating-and-editing-projects.md#Creating+project+pointing+to+repository+URL), create a [VCS root from a URL](guess-settings-from-repository-url.md), or [enable GitLab authentication](configuring-authentication-settings.md#GitLab.com).

To configure a GitLab connection:
1. In __Project Administration | Connections__, click __Add Connection__.
2. Select _GitLab.com_ or _GitLab CE/EE_ as the connection type. On the page that opens, copy the _Redirect URL_ to use for registering an OAuth application in GitLab.
3. To register TeamCity in GitLab, follow [these instructions](https://docs.gitlab.com/ee/integration/oauth_provider.html). You will need to:   
   1. Insert the TeamCity _Redirect URL_ as _Redirect URI_ and select the `api` [scope](https://docs.gitlab.com/ee/integration/oauth_provider.html#authorized-applications). 
   2. Save your application.
   3. Copy the application ID and secret generated by GitLab to use in the next step.
4. Continue configuring the connection in TeamCity. On the __Add Connection__ page, specify:
   * _Application ID_
   * _Secret_
   * _Server URL_ (only for GitLab CE/EE)
5. Save the connection settings. Now the connection is configured and you will see a small GitLab icon where a repository URL can be specified: [create project from URL](creating-and-editing-projects.md#Creating+project+pointing+to+repository+URL) and [create VCS root from URL](guess-settings-from-repository-url.md).
6. Click the icon, sign in to GitLab, and authorize TeamCity. TeamCity will be granted access to your repositories.

>If your GitLab CE/EE server uses HTTPS, you need to [upload its HTTPS certificate](uploading-ssl-certificates.md).

<note>

When creating a VCS root URL for GitLab, note that TeamCity will not extract credentials from the root URL. You need to specify a username and password manually.

</note>

### Connecting to Azure DevOps Services

You can configure a connection to your Azure DevOps Services (or Azure DevOps Server, formerly Team Foundation Server) to create a [project from a URL](creating-and-editing-projects.md#Creating+project+pointing+to+repository+URL), create a [VCS root from a URL](guess-settings-from-repository-url.md), create a [TFS](team-foundation-server.md) VCS root, or create a [Team Foundation Work Items](team-foundation-work-items.md) tracker.

To configure a connection to Azure DevOps Services:
1. In __Project Administration | Connections__, click __Add Connection__.
2. Select _Azure DevOps Services_ as the connection type.   
On the page that opens, copy the parameters to use for connecting TeamCity to Azure DevOps Services.
3. Log in to your Azure DevOps Services account to create a personal access token with _All scopes_, as described in the [Microsoft documentation](https://www.visualstudio.com/en-us/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate).
4. Continue configuring the connection in TeamCity. On the __Add Connection__ page, specify:
   * The server URL in the `https://{account}.visualstudio.com` format or your Team Foundation Server web portal as `https://{server}:8080/tfs/`.
   * Your personal access token.
5. Save the connection settings. Now the connection is configured and you will see a small Azure DevOps Services icon where a repository URL can be specified: [create project from URL](creating-and-editing-projects.md#Creating+project+pointing+to+repository+URL), [create VCS root from URL](guess-settings-from-repository-url.md), [create TFS VCS root](team-foundation-server.md), and [create Team Foundation Work Items tracker](team-foundation-work-items.md).
6. Click the icon, log in to Azure DevOps Services, and authorize TeamCity. TeamCity will be granted full access to all of the resources that are available to you.   
After the VSTS project is connected, your personal access token will be inserted automatically when configuring Commit Status Publisher for Git repositories hosted in TFS/VSTS.

>It is possible to configure multiple VSTS connections. In this case the server URL will be displayed next to the VSTS icon to distinguish the server in use.

## Creating Entities from URLs in TeamCity

Now that you've connected your hosting service, creating entities from a URL in TeamCity is easier than ever. Click the GitHub, Bitbucket, GitLab, or VSTS icon to view the list of available repositories (note that for Bitbucket, only public repositories will be available via the configured connection):

<img src="ProjectConnectionBitbucket.png" width="587" alt="Available Bitbucket repositories"/>

You can select the URL and continue with the configuration.
