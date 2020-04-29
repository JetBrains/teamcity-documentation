[//]: # (title: Configuring VCS Roots)
[//]: # (auxiliary-id: Configuring VCS Roots)

<anchor name="VCSRoots"/>

## VCS Roots in TeamCity

<chunk include-id="VCSRoot">

A _VCS root_ in TeamCity defines a [connection to a version control system](configuring-vcs-settings.md) and consists of a set of settings (paths to sources, username, password, and other settings) that defines how TeamCity communicates with a version control (SCM) system to [monitor changes](configuring-vcs-roots.md#Common+VCS+Root+Properties) and get sources for a build. A VCS root can be attached to a build configuration or template. You can add several VCS Roots to a build configuration or a template and specify portions of the repository to checkout and target paths via [VCS Checkout Rules](vcs-checkout-rules.md).

<anchor name="SharedVCSRoots"/>

VCS roots are created in a project and are available to all the build configurations defined in that project or its [subprojects](project.md#Settings+Propagation).

You can view all VCS roots configured within the project and create/edit/delete/detach them using the __VCS Roots__ section of the __Project Settings__.   
If someone attempts to modify a VCS root that is used in more than one project or build configuration, TeamCity will issue a warning that the changes to the VCS root could potentially affect other projects or build configurations. The user is then prompted to either save the changes and apply them to all the affected projects and build configurations, or to make a copy of the VCS root to be used by either a specific build configuration or project.

On an attempt to create a new VCS root, TeamCity checks whether there are other VCS roots accessible in this project with similar settings. If such VCS roots exist, TeamCity suggests using them.

<anchor name="CommonVCSRootProps"/>

Once a VCS root is configured, TeamCity regularly queries the version control system for new changes and displays the changes in the [Build Configurations](build-configuration.md) that have the root attached. You can set up your build configuration to trigger a new build each time TeamCity detects changes in any of the build configuration's VCS roots, which suits most cases. When a build starts, TeamCity gets the changed files from the version control and applies the changes to the [Build Checkout Directory](build-checkout-directory.md).

</chunk>


### Common VCS Root Properties

<table><tr>

<td>

Property

</td>

<td>

Description

</td></tr><tr>

<td>

<anchor name="vcs-type"/>

Type of VCS


</td>

<td>

Type of a version control system supported by TeamCity, for example, Perforce, Subversion, and so on.


</td></tr><tr>

<td>

VCS root name

</td>

<td>
 
 
Unique name of VCS root across all VCS roots of the project.

</td></tr><tr>

<td>

<anchor name="VCSRootID"/>

VCS root ID


</td>

<td>

<anchor name="VCSRootID"/>

Unique [ID](identifier.md) of VCS root across all VCS roots in the system. VCS root ID can be used in parameter references to VCS root parameters and [REST API](rest-api.md). If not specified, will be generated automatically from VCS root parameters.

</td></tr><tr>

<td>

Repository URL

</td>

<td>

URL to VCS repository. Supports URLs in [different formats](guess-settings-from-repository-url.md#VCS+URL+Formats), like: `http(s)://`, `svn://`, `ssh://git@`, `git://` and others as well as URLs in Maven format.

</td></tr><tr>

<td>

Minimum checking interval

<anchor name="checkingInterval"/>

</td>

<td>
  
Specifies how often TeamCity polls the VCS repository for VCS changes. By default, the global predefined server setting is used that can be modified on the __Administration | Global Settings__ page. The interval time starts as soon as the last poll is finished on the per-VCS root basis. Here you can specify a custom interval for the current VCS root.

<note>

Some public servers may block access if polled too frequently.

</note>

If TeamCity detects that a [VCS commit hook](configuring-vcs-post-commit-hooks-for-teamcity.md) is used to trigger checking for changes, this interval is automatically increased up to the predefined value (4 hours). If the periodical check finds changes undetected via the commit hook, the checking interval is reset to the specified minimum.


</td></tr><tr>

<td>

<anchor name="svnRootSharing"/>

Belongs to project


</td>

<td>

Each VCS root belongs to some project, and in this section the name of this project is displayed. A VCS root can be moved to the common parent project of all subprojects, build configurations and templates where the root is currently used.

</td></tr></table>

Refer to the following pages for VCS-specific configuration details:

<toc>
</toc>

<note>

Make sure to synchronize the system time at the VCS server, TeamCity server and TeamCity agent (if agent-side checkout is used) if you use the following version control systems:
* CVS
* StarTeam (if the audit is disabled or the server version is older than 9.0)
* Subversion repositories connected through externals to the main repository defined in the VCS root
* VSS (all VSS [clients](http://support.microsoft.com/kb/248240) and TeamCity server should have synchronized clocks)

</note>


[//]: # (Internal note. Do not delete. "Configuring VCS Rootsd91e181.txt")    
[//]: # (Internal note. Do not delete. "Configuring VCS Rootsd91e186.txt")    

__ __