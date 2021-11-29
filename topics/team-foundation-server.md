[//]: # (title: Team Foundation Server)
[//]: # (auxiliary-id: Team Foundation Server)

<note>

In 2019, Team Foundation Server has been renamed to Azure DevOps Server. The content of this page is applicable to Azure DevOps Server.

</note>

This page contains descriptions of the fields and options available when setting up a [VCS root](vcs-root.md) to connect to Microsoft Team Foundation Server Version Control.

Common VCS Root properties are described [here](configuring-vcs-roots.md#Common+VCS+Root+Properties).

When connecting to an Azure DevOps Git repository, select [Git](git.md) as Type of VCS. 

<tip>

TeamCity can also [automatically configure the project / VCS root](guess-settings-from-repository-url.md) from a repository URL.
If you have a TFVC root configured, TeamCity will suggest configuring the [Team Foundation Work Items](team-foundation-work-items.md) as well.
</tip>

## Cross-Platform TFS Integration

TeamCity features the [cross-platform TFS integration](https://blog.jetbrains.com/teamcity/2015/12/teamcity-cross-platform-tfs-support/), which works on Linux, macOS, and Windows platforms. Without installing additional software, TeamCity servers and build agents can interact with Team Foundation Servers 2012 or later, and Azure DevOps Services.

The built-in TFS plugin can work in two modes: the default and cross-platform. The working mode is based on the availability of Team Explorer (default mode): if it is not present, the plugin falls back from the default to cross-platform mode. 

When detecting the Team Explorer version, TeamCity checks [.NET GAC](https://msdn.microsoft.com/en-us/library/yf1d93sz.aspx) and the following paths:
* `Windows x86: %\CommonProgramFiles%\Microsoft Shared\Team Foundation Server\%\version_number%`
* `Windows x64: %\CommonProgramFiles(x86)%\Microsoft Shared\Team Foundation Server\%\version_number%`

To enforce the cross-platform mode on TeamCity, set the `teamcity.tfs.mode=java` [internal property](server-startup-properties.md#TeamCity+Internal+Properties) or [build configuration parameter](configuring-build-parameters.md).
{product="tc"}

To enforce the cross-platform mode on TeamCity, set the `teamcity.tfs.mode=java` [build configuration parameter](configuring-build-parameters.md).
{product="tcc"}

## TFS Settings

<table><tr>

<td>

Option

</td>

<td>

Description

</td></tr><tr>

<td>

URL

</td>

<td>

Team Foundation Server URL in the following format:

* TFS 2010+: `http[s]://<host>:<port>/tfs/<collection>`
* TFS 2005/2008: `http[s]://<host>:<port>`
* Azure DevOps: `https://dev.azure.com/<organization>`
* VSTS: `https://<accountname>.visualstudio.com`

</td></tr><tr>

<td>

Root

</td>

<td>

Specify the root using the following format: ` $<project name><project_catalog>`

</td></tr><tr>

<td>

Username

</td>

<td>

Specify a user to access Team Foundation Server. This can be a username or `DOMAIN\UserName` string.   
Use blank to let TFS select a user account that is used to run the TeamCity Server (or Agent for the agent-side checkout).

</td></tr><tr>

<td>

Password

</td>

<td>

Enter the password of the user entered above

</td></tr></table>

Learn more about authentication in [Azure DevOps](#teamFoundationServerLive).

## Agent-Side Checkout

The [agent-side](vcs-checkout-mode.md#agent-checkout) checkout is supported on Windows, as well as Linux and Mac agent machines.

TeamCity automatically creates a TFS workspace for each [checkout directory](build-checkout-directory.md) used. The workspace is created on behalf of the user account specified in the VCS root settings.

By default, the created TFS workspace uses the location defined in the TFS server settings. You can force TeamCity to use a specific workspace location via the [build configuration parameter](configuring-build-parameters.md) `teamcity.tfs.workspace.location` set to `local` or `server`.

The created TFS workspaces are automatically removed based on the timeout configured via the `teamcity.tfs.workspace.idleTime` [build agent property](configure-agent-installation.md) set to the default value of 1209600 sec (2 weeks).

<table><tr>

<td>

Option

</td>

<td>

Description

</td></tr><tr>

<td>

Enforce overwrite all files

</td>

<td>

When the option is enabled, TeamCity will call TFS to update workspace rewriting all files.

</td></tr></table>

Normally, there is no need to do a forced update for every build. But, if you suspect that TeamCity is not getting the latest version from the repository, you can use this option.

TFS does not allow several workspaces on a machine mapped to the same directory. If it happens, the TeamCity TFS agent-side checkout will attempt to remove intersecting workspaces to create a new workspace that matches the specified VCS root and checkout rules.

Note that it can fail to remove workspaces created by another user, and in this case you need to remove such workspaces manually.

To differentiate local mappings, it is recommended to use checkout rules in the following format:

* `$/root1 => /root1`
* `$/root2 => /root2`

>If you use TFS version 11 or later, note that the checkout rules are treated as case-sensitive.

<anchor name="TeamFoundationServer-azure-devops"/>

<anchor name="VisualStudioOnline"/>

<anchor name="vsts"/>

<anchor name="teamFoundationServerLive"/>

## Authentication Notes

The following authentication options are available in Azure DevOps.

<chunk include-id="azure-authentication">

### Personal Access Tokens

To use access tokens, you need to create a [personal access token](https://www.visualstudio.com/en-us/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) in your Azure DevOps account, where you have to set some __Code__ [access scope](#Required+Access+Scope) in your repositories and use it when configuring a VCS root.

<note>

You can authenticate with a personal access token to Azure DevOps Services only. TeamCity does not support token authentication to hosted [Azure DevOps Server](https://azure.microsoft.com/en-in/services/devops/server/) (formerly, Team Foundation Server) installations.

</note>

<table><tr>

<td>

Option

</td>

<td>

Description

</td></tr><tr>

<td>

Username

</td>

<td>

Leave blank for TFVC, any value for Git, for example, username

</td></tr><tr>

<td>

Password

</td>

<td>

Enter your personal access token created earlier 

</td></tr></table>

#### Required Access Scope

<table><tr>

<td>

TFS subsystem

</td>

<td>

Scopes

</td></tr><tr>

<td>

TFVC

</td>

<td>

All scopes

</td></tr><tr>

<td>

Git

</td>

<td>

Code (read) / Code (read and write) for versioned settings

</td></tr><tr>

<td>

Work Items

</td>

<td>

Work Items (read, write, and manage)

</td></tr><tr>

<td>

Commit Status

</td>

<td>

Code (status)

</td></tr></table>

### Alternate Authentication Credentials

<note>

Azure DevOps [stops supporting](https://devblogs.microsoft.com/devops/azure-devops-will-no-longer-support-alternate-credentials-authentication/) alternate credentials since March 2, 2020. To be able to authenticate in Azure DevOps, please use alternative methods instead (such as personal access tokens).

</note>

To use the login/password pair authentication, you have to enable [alternate credentials](https://www.visualstudio.com/en-us/docs/report/analytics/client-authentication-options#create-an-alternate-access-credential) in your Azure DevOps account, where you can set a secondary username and password to use when configuring a VCS root.

</chunk>

### NTLM/Kerberos on Linux and macOS

To use this authentication method, check that your machine includes Kerberos libraries and that the authentication is properly configured. If you encounter any issues, please check the steps described in the [Microsoft documentation](https://msdn.microsoft.com/en-us/library/gg475929(v=vs.120).aspx#Anchor_3).

## Team Foundation Proxy Configuration

To enable usage of [Team Foundation Proxy](https://www.visualstudio.com/en-us/docs/setup-admin/tfs/install/install-proxy-setup-remote), define the `TFSPROXY` environment variable for the user account which runs the TeamCity server or agent and restart them to apply changes.

Example:

```Plain Text
TFSPROXY=https://tfs-proxy:8081
```

## HTTP Proxy Server Configuration

### Default .NET Working Mode

To interact with the TFS server, the proxy server settings specified for the user account which runs the TeamCity server or agent will be used. 

### Cross-Platform Working Mode
{product="tc"}

The default Java proxy server settings specified for the TeamCity server or agent will be used in the TFS integration. On the TeamCity server, [internal properties or Java options](server-startup-properties.md) can be used. On the TeamCity agent, [build agent configuration](configure-agent-installation.md) or [Java options](configuring-build-agent-startup-properties.md) can be used. The TeamCity-TFS integration supports the following options:


```Plain Text
http.proxyHost
http.proxyPort
http.nonProxyHosts
https.proxyHost
https.proxyPort
http.proxyUser
http.proxyPassword
```

### Cross-Platform Working Mode
{product="tcc"}

The default Java proxy server settings specified for the TeamCity agent will be used in the TFS integration. On the TeamCity agent, [build agent configuration](configure-agent-installation.md) or [Java options](configuring-build-agent-startup-properties.md) can be used. The TeamCity-TFS integration supports the following options:


```Plain Text
http.proxyHost
http.proxyPort
http.nonProxyHosts
https.proxyHost
https.proxyPort
http.proxyUser
http.proxyPassword
```
