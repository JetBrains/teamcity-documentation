[//]: # (title: Configuring Connections)
[//]: # (auxiliary-id: Configuring Connections)

TeamCity allows storing presets of connections to external services. You can reuse these presets in various places on the server: when creating projects, configuring notifications, integrating with issue trackers, and more. This article gives instructions on how to add each type of connection.

To add a connection, go the target project's settings, open the __Connections__ page, and click __Add Connection__. Select the connection type, set its _Display name_ to distinguish it from the others, and configure it as described below.

When created, a connection can be used in all the nested subprojects of the current project. If you add a connection in the Root project, it will become available on the whole server.

## Azure DevOps

<chunk include-id="azure-devops">

There are two types of Azure DevOps connections in TeamCity:
* __Azure DevOps OAuth 2.0__ allows signing in to TeamCity via an Azure DevOps Services account.
* __Azure DevOps PAT__ allows creating TeamCity projects from Azure Git and TFVC repositories.

<anchor name="azure-devops-connection"/>

#### Azure DevOps OAuth 2.0 Connection
{id="Connecting+to+Azure+DevOps" auxiliary-id="Connecting to Azure DevOps"}

This type of connection supports only Azure DevOps Services. It uses the [OAuth 2.0 protocol](https://docs.microsoft.com/en-us/azure/devops/integrate/get-started/authentication/oauth?view=azure-devops) based on JWT tokens and requires creating a dedicated app in your Azure profile.

This connection can be used for enabling [user authentication via Azure DevOps](configuring-authentication-settings.md#Azure+DevOps+Services).

To configure an Azure DevOps OAuth 2.0 connection:
1. In __Project Administration | Connections__, click __Add Connection__.
2. Select _Azure DevOps OAuth 2.0_ as the connection type.
3. TeamCity will display the _Callback URL_ and _scopes_ required for registering an OAuth application in Azure DevOps.  
   Go to the [Register Application](https://app.vsaex.visualstudio.com/app/register) page in Azure and create a new app using the provided parameters. When created, copy the app's ID and client secret.
4. Go back to the connection form in TeamCity and enter the Azure DevOps Services URL, the new application ID, and client secret.
5. Save the connection.

To activate the Azure DevOps Services authentication on your server, proceed to enabling the respective [authentication module](configuring-authentication-settings.md#Azure+DevOps+Services).

#### Azure DevOps PAT Connection

This type of connection uses personal access tokens. It allows creating a [project from a Git or TFVC repository URL](creating-and-editing-projects.md#Creating+project+pointing+to+repository+URL), creating a [TFS VCS root](team-foundation-server.md), or integrating with the [Team Foundation Work Items](team-foundation-work-items.md) tracker.

To configure an Azure DevOps PAT connection:
1. In __Project Administration | Connections__, click __Add Connection__.
2. Select _Azure DevOps PAT_ as the connection type.   
   The page that opens provides the parameters to be used when connecting TeamCity to Azure DevOps Services.
3. Log in to your Azure DevOps Services account to create a personal access token with _All scopes_ as described in the [Microsoft documentation](https://www.visualstudio.com/en-us/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate).
4. Continue configuring the connection in TeamCity: on the __Add Connection__ page that is open, specify
   * the server URL in the `https://{account}.visualstudio.com` format or your Team Foundation Server web portal as `https://{server}:8080/tfs/`
   * your personal access token
5. Save the connection settings.
6. The connection is configured, and now a small Azure DevOps Services icon becomes active in several places where a repository URL can be specified: [create project from URL](creating-and-editing-projects.md#Creating+project+pointing+to+repository+URL), [create VCS root from URL](guess-settings-from-repository-url.md), create [TFS](team-foundation-server.md) VCS root, create [Team Foundation Work Items](team-foundation-work-items.md) tracker. Click the icon, log in to Azure DevOps Services and authorize TeamCity. TeamCity will be granted full access to all the resources that are available to you.   
   When configuring Commit Status Publisher for Git repositories hosted in TFS/VSTS, the personal access token can be filled out automatically if a VSTS project connection is configured.

>It is possible to configure several VSTS connections. In this case, the server URL will be displayed next to the VSTS icon to distinguish the server in use.

</chunk>

## Bitbucket Cloud

<chunk include-id="bb-cloud">

>In TeamCity Cloud, a connection to Bitbucket Cloud is already predefined in the Root project's settings, which makes it available in all the other projects.
>
{product="tcc"}

A connection to Bitbucket Cloud can be used to:
* create a [project from Bitbucket URL](creating-and-editing-projects.md#Creating+project+pointing+to+repository+URL);
* create a [VCS root from URL](guess-settings-from-repository-url.md);
* create a [Mercurial VCS root](mercurial.md);
* integrate with a [Bitbucket Cloud issue tracker](bitbucket-cloud.md);
* enable [BitBucket Cloud authentication](configuring-authentication-settings.md#Bitbucket+Cloud).

The Bitbucket Cloud connection form provides multiple parameters. You need to use them for [creating a new OAuth consumer in Bitbucket](https://confluence.atlassian.com/bitbucket/oauth-on-bitbucket-cloud-238027431.html#OAuthonBitbucketCloud-Createaconsumer).

After the consumer is created:
1. Copy its key and secret.
2. Go back to the connection form in TeamCity.
3. Paste the key and secret.
4. Save the connection.

A Bitbucket icon will become active in several places where a repository URL can be specified. Click it to authorize TeamCity in your Bitbucket profile. TeamCity will be granted access to your public repositories. For private repositories, you will need to provide Bitbucket credentials to be used for authentication by TeamCity, as Bitbucket Cloud does not provide non-expiring access tokens. See the [related discussion](https://bitbucket.org/site/master/issues/11774/application-specific-passwords-or-tokens). If you configure multiple Bitbucket connections, the server URL will be displayed next to each icon, so it is easier to distinguish the server in use.

</chunk>

## GitHub

<chunk include-id="github">

>In TeamCity Cloud, a connection to GitHub.com is already predefined in the Root project's settings, which makes it available in all the other projects.
>
{product="tcc"}

There are two types of GitHub connections: __GitHub Enterprise__ and __GitHub.com__. Choose it depending on your GitHub account type.

A connection to GitHub can be used to:
* create a [project from GitHub URL](creating-and-editing-projects.md#Creating+project+pointing+to+repository+URL)
* create a [VCS root from URL](guess-settings-from-repository-url.md)
* create a [Git VCS root](git.md)
* integrate with a [GitHub issue tracker](github.md)
* enable [GitHub.com authentication](configuring-authentication-settings.md#GitHub.com)

The GitHub connection form provides multiple parameters. You need to use them to [create a new OAuth application in GitHub](https://docs.github.com/en/developers/apps/authorizing-oauth-apps).

After the app is created:
1. Copy its client ID and secret.
2. Go back to the connection form in TeamCity.
3. Paste the GitHub server URL (only for Enterprise) and the app ID and secret.
4. Save the connection.

If you use a GitHub Enterprise server with HTTPS, you need to also upload its HTTPS certificate as described [here](uploading-ssl-certificates.md).

>If you enable the [GitHub.com authentication](configuring-authentication-settings.md#GitHub.com) module and want to restrict access to TeamCity to users of specific GitHub organizations, you need to ensure that your OAuth app is allowed by all these organizations. By default, GitHub does not allow OAuth apps to access the organizations. You can either disable this restriction for all apps or approve only the TeamCity app in each of the required organizations. Please refer to the [GitHub documentation](https://docs.github.com/en/github/setting-up-and-managing-organizations-and-teams/about-oauth-app-access-restrictions) for more details.

A GitHub icon will become active in several places where a repository URL can be specified. Click it to authorize TeamCity in your GitHub profile. TeamCity will be granted full control of your private repositories and get the _Write repository hooks_ permission. If you configure multiple GitHub integrations, the server URL will be displayed next to each icon, so it is easier to distinguish the server in use.

</chunk>

## GitLab

<chunk include-id="gitlab">

>In TeamCity Cloud, a connection to GitLab.com is already predefined in the Root project's settings, which makes it available in all the other projects.
>
{product="tcc"}

There are two types of GitLab connections: __GitLab CE/EE__ and __GitLab.com__. Choose it depending on your GitHub account type.

A connection to GitLab can be used to:
* create a [project from GitLab URL](creating-and-editing-projects.md#Creating+project+pointing+to+repository+URL)
* create a [VCS root from URL](guess-settings-from-repository-url.md)
* enable [GitLab.com authentication](configuring-authentication-settings.md#GitLab.com)

The GitLab connection form provides multiple parameters. You need to use them to [create a new OAuth application in GitLab](https://docs.gitlab.com/ee/integration/oauth_provider.html).

After the app is created:
1. Copy its client ID and secret.
2. Go back to the connection form in TeamCity.
3. Paste the GitLab server URL (only for CE/EE) and the app ID and secret.
4. Save the connection.

If you use a GitLab CE/EE server with HTTPS, you need to also upload its HTTPS certificate as described [here](uploading-ssl-certificates.md).

A GitLab icon will become active in several places where a repository URL can be specified. Click it to authorize TeamCity in your GitLab profile. TeamCity will be granted access to your repositories. If you configure multiple GitLab connections, the server URL will be displayed next to each icon, so it is easier to distinguish the server in use.

</chunk>

## Docker Registry

A connection to Docker Registry can be used to:
* sign in to an authenticated Docker registry before running a build / sign out after the build;
* clean up published images after the build.

See more information in the [dedicated article](configuring-connections-to-docker.md).

## Amazon ECR

An Amazon ECR (Elastic Container Registry) connection allows accessing private AWS registries. With its help, the [Docker Support](docker-support.md) build feature can store Docker images produced by a build to a private registry.

Connection settings:

<table>
<tr><td>Setting</td><td>Description</td></tr>

<tr>
<td>

AWS region

</td>
<td>

Select an AWS region where the target resources are located.

</td>
</tr>

<tr>
<td>

Credentials type

</td>
<td>

* __Access key__: select to use preconfigured AWS account access keys. You can find them in the [Identity and Access Management](https://console.aws.amazon.com/iam) section of your AWS console.
* __Temporary credentials__: get [temporary access keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp.html) via AWS STS. Such credentials are short-term and can be revoked anytime. They do not belong to a specific user and can be provided on demand — to grant temporary access to specific resources.

</td>
</tr>

<tr>
<td>

IAM role ARN

(_only for Temporary credentials_)

</td>
<td>

Specify a role to be used for generating temporary credentials. You need to [create this role in advance](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-user.html) in your AWS console and assign it to all the permissions you need.

</td>
</tr>

<tr>
<td>

External ID

(_only for Temporary credentials_)

</td>
<td>

Specify an [external ID](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-user_externalid.html). We strongly recommend that you always define it when using temporary credentials. This ensures that only TeamCity will be able to use the specified IAM role.

</td>
</tr>

<tr>
<td>

Default credential provider chain

</td>
<td>

Enable this option to automatically find access credentials according to the [default chain](https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/credentials.html#credentials-default).

This approach is recommended if you do not want to store the credentials anywhere in the TeamCity environment. By default, it will use the values of `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` environment variables.

</td>
</tr>

<tr>
<td>

Access key ID

</td>
<td>

Specify the access key ID.

See how to get it [here](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html).

</td>
</tr>

<tr>
<td>

Secret access key

</td>
<td>

Specify the secret access key.

See how to get it [here](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html).

</td>
</tr>

<tr>
<td>

Registry ID

</td>
<td>

Enter your [account ID](https://docs.aws.amazon.com/IAM/latest/UserGuide/console_account-alias.html#FindingYourAWSId) number.

</td>
</tr>

</table>

## Slack

This type of connection is used to enable notifications via [Slack](https://slack.com/).

Before configuring a Slack connection, you need to create a [Slack app](https://api.slack.com/apps) with the following [bot token scopes](https://api.slack.com/scopes): `channels:read`, `chat:write`, `im:read`, `im:write`, `users:read`, `team:read`, `groups:read`. You can add these in __Features | OAuth & Permissions | Scopes__ of your Slack app.

To ensure your TeamCity server can connect to Slack, specify all the possible endpoint addresses of the server as __Redirect URLs__ in __Features | OAuth \& Permissions__. In most cases, it would be enough to specify the _Server URL_ set in __[Global Settings](configuring-server-url.md)__ in TeamCity. However, if you use a proxy for your TeamCity server but access this server directly, the authentication in Slack might not work unless the server's IP address is also specified in __Redirect URLs__.
{product="tc"}

To ensure your TeamCity server can connect to Slack, specify its address as __Redirect URL__ in __Features | OAuth \& Permissions__.
{product="tcc"}

>See this [Basic app setup](https://api.slack.com/authentication/basics) guide for more details.

Now you can return to TeamCity, add a new Slack connection, and enter the following connection parameters:
* client ID and secret from the app's __Basic Information__ page;
* a [bot user token](https://api.slack.com/docs/token-types#bot) of your app.

Save the connection and proceed with adding a [Notifier](notifications.md#Slack+Notifier) build feature.

<anchor name="jetbrains-space-connection"/>

## JetBrains Space
{id="connect-to-jetbrains-space" auxiliary-id="Connect to JetBrains Space"}

This type of connection can be used for:
* publishing build statuses in [JetBrains Space](https://www.jetbrains.com/space/) with the help of [Commit Status Publisher](commit-status-publisher.md)
* [authenticating in TeamCity](configuring-authentication-settings.md#JetBrains+Space) with a JetBrains Space account
* creating [projects](creating-and-editing-projects.md), [build configurations](creating-and-editing-build-configurations.md), and [VCS roots](configuring-vcs-roots.md) from a JetBrains Space repository

Before configuring this connection, you need to create a dedicated application in JetBrains Space:
1. Go to __Administration | Applications__ and click __New application__.
2. Enter a convenient name and save the application.
3. Go to the app's __Authorization__ tab and click __Configure requirements__ under the __In-context Authorization__ section. Enter the name of the Space project you are about to access from TeamCity.
4. Now, you need to set permissions that will be granted to the app in this project. Click __Configure__ and enable the following permissions:
   * General access / authentication:
      * _Members | View member profile_ (you might need a server administrator's approval for that)
   * Required for Commit Status Publisher:
      * _Git Repositories | Report external check status_ (if you are the project's administrator, you can approve this permission right in this __Authorization__ tab)
5. Go back to the app's __Overview__ and open the __Authentication__ tab.
6. Enable _Client Credentials Flow_.
7. To be able to use authentication via Space in TeamCity or/and to create projects/configurations from Space repositories, enable _Authorization Code Flow_ as well. Enter your TeamCity server's URL as the redirect URI.  
   To ensure that your TeamCity server can always connect to JetBrains Space, specify all the other possible endpoint addresses of the server. In most cases, it would be enough to specify the _Server URL_ set in __Global Settings__ in TeamCity. However, if you use a proxy for your TeamCity server but access this server directly, the authentication might not work unless the server's IP address is also specified here.
8. Copy the app's _Client ID_ and _Client secret_.

__Note__: When you create a project in JetBrains Space, it does not automatically add you to this project as a member — this needs to be done manually. TeamCity will be able to see only those projects where you (or the user who created the application in Step 1) are listed as a member.

Now you can return to TeamCity, add a new JetBrains Space connection, and enter the following connection parameters:
* URL of the Space server
* client ID and secret of your Space application

Save the connection and proceed with adding a [Commit Status Publisher](commit-status-publisher.md), [enabling Space authentication](configuring-authentication-settings.md#JetBrains+Space), or creating a [project](creating-and-editing-projects.md#Creating+project+pointing+to+JetBrains+Space) / [build configuration](creating-and-editing-build-configurations.md#Creating+Build+Configuration+Pointing+to+Specific+VCS) / [VCS root](configuring-vcs-roots.md).

## NPM Registry
{id="npm-registry-settings" auxiliary-id="npm-registry-settings"}

This type of connection allows accessing a private [npm registry](https://docs.npmjs.com/cli/v7/using-npm/registry) during a build.

Connection settings:

<table>

<tr><td>Setting</td><td>Description</td></tr>

<tr>

<td>Scope</td>
<td>

Specify an npm user/organization's [scope](https://docs.npmjs.com/cli/v7/using-npm/scope) to [associate](https://docs.npmjs.com/cli/v7/using-npm/scope#associating-a-scope-with-a-registry) with the connected registry. If you want to use multiple registries per project, you need to specify a scope for each of them.

Leave empty if you want to use only one registry in this project. It will be used by `npm`/`yarn` commands by default.

>If you don't specify a scope in the connection settings, you can still access a specific registry within a Node.js step by appending `--registry registry_url` to the `npm ...` command.

</td>

</tr>

<tr>

<td>Registry URL</td>
<td>

Specify the npm registry URL in the following format: `http(s)://hostname[:port]`. For example, `https://npm.pkg.jetbrains.space/mycompany/p/projectkey/mynpm`. The HTTPS schema is used by default.

</td>

</tr>

<tr>

<td>Access token</td>
<td>

Specify a [token](https://docs.npmjs.com/about-access-tokens), if it's needed for accessing the registry. Leave empty for anonymous access. Note that token-based authentication could differ depending on a registry type. See instructions for [npm Enterprise](https://docs.npmjs.com/creating-and-viewing-access-tokens), [Space Packages](https://www.jetbrains.com/help/space/authenticate-in-packages.html#space_account), or [GitHub Packages](https://docs.github.com/en/packages/learn-github-packages/about-permissions-for-github-packages#about-scopes-and-permissions-for-package-registries).

</td>

</tr>

</table>

Save the connection and proceed with adding an [NPM Registry Connection](nodejs.md#Accessing+Private+NPM+Registries) build feature.

## Perforce Administrator Access

This type of connection allows [processing task streams on your Perforce server](perforce-workspace-handling-in-teamcity.md#Cleaning+Workspaces+on+Perforce+Server). In the connection settings, enter the host and user credentials for accessing the Perforce server (the user must have the [admin](https://www.perforce.com/manuals/p4sag/Content/P4SAG/protections.set.html#protections.set.access_levels) permission).