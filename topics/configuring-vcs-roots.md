[//]: # (title: Configuring VCS Roots)
[//]: # (auxiliary-id: Configuring VCS Roots)

<anchor name="VCSRoots"/>

<include from="vcs-root.md" element-id="VCSRoot"/>

## Common VCS Root Properties

<table><tr>

<td>

Property

</td>

<td>

Description

</td></tr><tr>

<td id="vcs-type">

Type of VCS

</td>

<td>

Type of version control system supported by TeamCity: for example, Perforce or Subversion.

</td></tr><tr>

<td>

VCS root name

</td>

<td>

Unique name of VCS root across all VCS roots of the project.

</td></tr><tr>

<td id="VCSRootID">

VCS root ID

</td>

<td>

Unique [ID](identifier.md) of VCS root across all VCS roots in the system. VCS root ID can be used in parameter references to VCS root parameters and [REST API](https://www.jetbrains.com/help/teamcity/rest/teamcity-rest-api-documentation.html). If not specified, will be generated automatically from VCS root parameters.

</td></tr><tr>

<td>

Repository URL

</td>

<td>

URL to VCS repository. Supports URLs in [different formats](guess-settings-from-repository-url.md#VCS+URL+Formats), like: `http(s)://`, `svn://`, `ssh://git@`, `git://` and others as well as URLs in Maven format.

>If you use an SSH repository, watch our **video tutorial** on [how to check out from SSH repositories](https://www.youtube.com/watch?v=nUTb1BjMMoE).

</td></tr><tr>

<td>

Minimum polling interval

<anchor name="checkingInterval"/>

</td>

<td>
  
Specifies how often TeamCity polls the VCS repository for VCS changes. By default, the global predefined server setting is used that can be modified on the __Administration | Global Settings__ page. The interval time starts as soon as the last poll is finished on the per-VCS root basis. Here you can specify a custom interval for the current VCS root.

<note>

Some public servers may block access if polled too frequently.

</note>

If TeamCity detects that a [VCS commit hook](configuring-vcs-post-commit-hooks-for-teamcity.md) is used to trigger checking for changes, this interval is automatically increased up to the predefined value (4 hours). If the periodical check finds changes undetected via the commit hook, the polling interval is reset to the specified minimum.

</td></tr><tr>

<td id="svnRootSharing">

Belongs to project

</td>

<td>

Each VCS root belongs to some project, and in this section the name of this project is displayed. A VCS root can be moved to the common parent project of all subprojects, build configurations and templates where the root is currently used.

</td></tr></table>

Refer to the pages inside this section for VCS-specific configuration details.

<note>

Make sure to synchronize the system time at the VCS server, TeamCity server and TeamCity agent (if agent-side checkout is used) if you use the following version control systems:
* CVS
* StarTeam (if the audit is disabled or the server version is older than 9.0)
* Subversion repositories connected through externals to the main repository defined in the VCS root
* VSS (all VSS [clients](http://support.microsoft.com/kb/248240) and TeamCity server should have synchronized clocks)

</note>

<seealso>
        <category ref="concepts">
            <a href="vcs-root.md">VCS root</a>
        </category>
</seealso>


[//]: # (Internal note. Do not delete. "Configuring VCS Rootsd91e181.txt")    
[//]: # (Internal note. Do not delete. "Configuring VCS Rootsd91e186.txt")