[//]: # (title: Azure DevOps)
[//]: # (auxiliary-id: Azure DevOps;Team Foundation Server)

TeamCity can connect to repositories in a version control system of Microsoft Azure DevOps Server (formerly, Team Foundation Server), or Azure DevOps Services.

This article contains descriptions of the fields and options available when setting up a _Team Foundation Server_ [VCS root](vcs-root.md) type that is responsible for connecting Azure DevOps projects. Common VCS root properties are described [here](configuring-vcs-roots.md#Common+VCS+Root+Properties).

__This type of root allows connecting to TFVC repositories only. When connecting to an Azure DevOps Git repository, select the [Git](git.md) type__.

## Cross-Platform Azure DevOps Integration

The TeamCity integration with Azure DevOps can work on Windows, Linux, and macOS. TeamCity servers and build agents can interact with Team Foundation Servers 2012 or later and with Azure DevOps Services out of the box.

There are two operating modes available for this type of VCS root: default and cross-platform. If Team Explorer is present on the Azure server, TeamCity uses the default mode. Otherwise, it switches to the cross-platform mode.  
When detecting the Team Explorer version, TeamCity checks [.NET GAC](https://msdn.microsoft.com/en-us/library/yf1d93sz.aspx) and the following paths:
* `Windows x86: %\CommonProgramFiles%\Microsoft Shared\Azure DevOps Server\%\version_number%`
* `Windows x64: %\CommonProgramFiles(x86)%\Microsoft Shared\Azure DevOps Server\%\version_number%`

To enforce the cross-platform mode, set the `teamcity.tfs.mode=java` [internal property](server-startup-properties.md#TeamCity+Internal+Properties) or [build configuration parameter](configuring-build-parameters.md).
{product="tc"}

To enforce the cross-platform mode, set the `teamcity.tfs.mode=java` [build configuration parameter](configuring-build-parameters.md).
{product="tcc"}

## Settings

<table><tr>

<td>

Setting

</td>

<td>

Description

</td></tr><tr>

<td>

URL

</td>

<td>

The Server URL in the following format:
* Azure DevOps Services: `https://dev.azure.com/<organization>`
* Azure DevOps Server: `http[s]://<host>:<port>/tfs/<collection>`
* VSTS: `https://<accountname>.visualstudio.com`
* TFS 2005/2008: `http[s]://<host>:<port>`

</td></tr><tr>

<td>

Root

</td>

<td>

Specify the root directory using the following format: `$<project_name><project_catalog>`.

</td></tr><tr>

<td>

Username

</td>

<td>

Specify a user to access Azure DevOps Server. This can be a username or `DOMAIN\UserName` string.   
Leave empty to let Azure DevOps select a user account that is used to run the TeamCity Server (or Agent, for the agent-side checkout).

</td></tr><tr>

<td>

Password

</td>

<td>

Enter the user's password.

</td></tr></table>

## Agent-Side Checkout

The [agent-side checkout](vcs-checkout-mode.md#agent-checkout) is supported on Windows, Linux, and macOS agent machines.

TeamCity automatically creates an Azure DevOps workspace for each [checkout directory](build-checkout-directory.md). The workspace is created on behalf of the user whose account is specified in the VCS root settings.

By default, the created Azure DevOps workspace uses the location defined in the TFS server settings. You can force TeamCity to use a specific workspace location via the [build configuration parameter](configuring-build-parameters.md) `teamcity.tfs.workspace.location` set to `local` or `server`.

The created Azure DevOps workspaces are automatically removed based on the timeout configured via the `teamcity.tfs.workspace.idleTime` [build agent property](configure-agent-installation.md) set to the default value of 1209600 sec (2 weeks).

<table><tr>

<td>

Setting

</td>

<td>

Description

</td></tr><tr>

<td>

Enforce overwriting all files.

</td>

<td>

When this option is enabled, TeamCity will call Azure DevOps to update the workspace rewriting all files.

</td></tr></table>

Normally, there is no need to do a forced update for every build. If you suspect that TeamCity is not getting the latest version from the repository, you can use this option.

Azure DevOps does not allow several workspaces on a machine mapped to the same directory. If it happens, the TeamCity Azure DevOps agent-side checkout will attempt to remove intersecting workspaces to create a new workspace that matches the specified VCS root and checkout rules.  
Note that it can fail to remove workspaces created by another user. In this case, you need to remove such workspaces manually.

To differentiate local mappings, it is recommended to use checkout rules in the following format:
* `$/root1 => /root1`
* `$/root2 => /root2`

>If you use TFS version 11 or later, or Azure DevOps, note that the checkout rules are treated as case-sensitive.

<anchor name="TeamFoundationServer-azure-devops"/>
<anchor name="VisualStudioOnline"/>
<anchor name="vsts"/>
<anchor name="teamFoundationServerLive"/>

## Authentication in Azure DevOps

The following authentication options are available in Azure DevOps.

### Personal Access Tokens

When connecting to a TFVC repository, leave the _Username_ field in the VCS root settings empty and specify your access token as _Password_.

You can create a [personal access token](https://www.visualstudio.com/en-us/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) in your Azure DevOps account.

Set the _Code_ access scope to _All scopes_ in the repositories you are about to access from TeamCity.

### Alternate Authentication Credentials

Azure DevOps [stops supporting](https://devblogs.microsoft.com/devops/azure-devops-will-no-longer-support-alternate-credentials-authentication/) alternate credentials since March 2, 2020. To be able to authenticate in Azure DevOps, please use other methods instead (such as personal access tokens).

### NTLM/Kerberos on Linux and macOS

To use this authentication method, check that your machine includes Kerberos libraries and that the authentication is properly configured. If you encounter any issues, please check the steps described in the [Microsoft documentation](https://msdn.microsoft.com/en-us/library/gg475929(v=vs.120).aspx#Anchor_3).

## Azure DevOps Proxy Configuration

To enable usage of [Azure DevOps Proxy](https://www.visualstudio.com/en-us/docs/setup-admin/tfs/install/install-proxy-setup-remote), define the `TFSPROXY` [environment variable](configuring-build-parameters.md#Environment+Variables) for the user account which runs the TeamCity server or agent and restart them to apply changes.

Example:

```Plain Text
TFSPROXY=https://tfs-proxy:8081
```

## HTTP Proxy Server Configuration

### Default .NET Working Mode

To interact with the Azure DevOps Server, the proxy server settings specified for the user account which runs the TeamCity server or agent will be used. 

### Cross-Platform Working Mode

The default Java proxy server settings specified for the TeamCity server or agent will be used in the Azure DevOps integration. On the TeamCity server, [internal properties or Java options](server-startup-properties.md) can be used. On the TeamCity agent, [build agent configuration](configure-agent-installation.md) or [Java options](configuring-build-agent-startup-properties.md) can be used. The TeamCity-TFS integration supports the following options:
{product="tc"}

The default Java proxy server settings specified for the TeamCity agent will be used in the Azure DevOps integration. On the TeamCity agent, [build agent configuration](configure-agent-installation.md) or [Java options](configuring-build-agent-startup-properties.md) can be used. The TeamCity — Azure DevOps integration supports the following options:
{product="tcc"}

```Plain Text
http.proxyHost
http.proxyPort
http.nonProxyHosts
https.proxyHost
https.proxyPort
http.proxyUser
http.proxyPassword
```

<seealso>
        <category ref="admin-guide">
            <a href="azure-board-work-items.md">Azure Board Work Items</a>
        </category>
</seealso>
