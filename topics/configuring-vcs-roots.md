[//]: # (title: Configuring VCS Roots)
[//]: # (help-id: Configuring VCS Roots;VCS Root)

<anchor name="VCSRoots"/>

<include from="project-administrator-guide.md" element-id="VCSRoots"/>


## Common VCS Root Properties
{help-id="ConfiguringVCSRoots-CommonVCSRootProps"}

<deflist type="full">

<def title="Type of VCS" id="vcs-type" help-id="vcs-type">

The type of version control system supported by TeamCity. For example, Git, Perforce, Subversion, and more.

</def>

<def title="VCS root name">

The unique name of VCS root across all VCS roots of the project. This is the public name shown in TeamCity UI (for example, in the build configuration's "Attach VCS root" menu).

</def>

<def title="VCS root ID" id="VCSRootID" help-id="VCSRootID">

Unique [ID](identifier.md) of VCS root across all VCS roots in the system. By default, the root ID combines truncated names of its parent project and the root itself, divided with an underscore. For example, `MyProject_HttpsGitHubComJohndoeMyrepoRefsHeadsMain`. When changing the root name, you can click the **Regenerate ID** link to update this value.


Root IDs are used in build parameters that allow you to read root properties, for example `vcsroot.ProjectName_RootName.branch` and `vcsroot.ProjectName_RootName.url`. In addition, IDs can be used in [REST API](https://www.jetbrains.com/help/teamcity/rest/teamcity-rest-api-documentation.html).

</def>

<def title="Repository URL">

The URL of a VCS repository. Supports URLs in [different formats](guess-settings-from-repository-url.md#VCS+URL+Formats), like: `http(s)://`, `svn://`, `ssh://git@`, `git://` and others as well as URLs in Maven format.

> If using an SSH URL, you need to provide a private key so that TeamCity could access a repo. See the [](ssh-keys-management.md) article or watch our [video tutorial](https://www.youtube.com/watch?v=nUTb1BjMMoE) to learn more.

If a project (or its parent projects) has [connections to VCS providers](configuring-connections.md), TeamCity displays corresponding VCS icons next to this field. If you have multiple connections to the same provider (for example, GitHub OAuth and GitHub App), hover over an icon to see the connection name.

<img src="dk-connections-in-root-settings.png" width="706" alt="Connection icons in VCS Root settings"/>

Click an icon to view the list of repositories TeamCity detected using this connection. Since a connection stores all necessary repo access data, not only does selecting a repository fill in the **Repository URL**, but it also fills authentication settings (**Username** and **Password / Access token**).

</def>


<def title="Minimum polling interval" id="checkingInterval" help-id="checkingInterval">

Specifies how often TeamCity polls the VCS repository for VCS changes. By default, the global predefined server setting is used that can be modified on the __Administration | Global Settings__ page. The interval time starts as soon as the last poll is finished on the per-VCS root basis. Here you can specify a custom interval for the current VCS root.

<note>

Some public servers may block access if polled too frequently.

</note>

If TeamCity detects that a [VCS commit hook](configuring-vcs-post-commit-hooks-for-teamcity.md) is used to trigger checking for changes, this interval is automatically increased up to the predefined value (4 hours). If the periodical check finds changes undetected via the commit hook, the polling interval is reset to the specified minimum.

</def>

<def title="Belongs to project" id="svnRootSharing" help-id="svnRootSharing">

Displays the project that owns this VCS root. You can move a VCS root to a parent project so that it becomes available for all build configurations inside this new owner and its subprojects.


</def>

</deflist>



Refer to the pages inside this section for VCS-specific configuration details.

<note>

Make sure to synchronize the system time at the VCS server, TeamCity server and TeamCity agent (if agent-side checkout is used) if you use the following version control systems:
* CVS
* StarTeam (if the audit is disabled or the server version is older than 9.0)
* Subversion repositories connected through externals to the main repository defined in the VCS root
* VSS (all VSS [clients](http://support.microsoft.com/kb/248240) and TeamCity server should have synchronized clocks)

</note>


<!--[//]: # (Internal note. Do not delete. "Configuring VCS Rootsd91e181.txt")    
[//]: # (Internal note. Do not delete. "Configuring VCS Rootsd91e186.txt")-->