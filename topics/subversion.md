[//]: # (title: Subversion)
[//]: # (auxiliary-id: Subversion)

This article contains descriptions of Subversion-specific fields and options available when setting up a VCS root.

Common VCS root properties are described [here](configuring-vcs-roots.md#Common+VCS+Root+Properties).

>You do not need a Subversion client to be installed on the TeamCity server or agents. TeamCity bundles the Java implementation of an SVN client ([SVNKit](https://svnkit.com/)).
>
{style="tip"}

## SVN Connection Settings

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

Specify the SVN URL that points to the project sources.

</td></tr><tr>

<td>

Username

</td>

<td>

Specify the SVN username.

</td></tr><tr>

<td>

Password

</td>

<td>

Specify the SVN password.

</td></tr><tr>

<td>

Configuration directory

</td>

<td>

You can specify an alternative Subversion configuration directory, or use the default one (recommended). This setting also applies to agent-side checkout. TeamCity does not store authentication in SVN configuration directory, but can read settings stored there.

</td></tr><tr>

<td>

Use default configuration directory

</td>

<td>

Enable this option to make this the default configuration directory for the SVN connection.

</td></tr><tr>

<td>

Externals support

</td>

<td>

Select one of the following options to control the SVN externals processing.

* _Full support (load changes and checkout)_: TeamCity will check out all the configuration's sources (including the sources from the externals) and will gather and display information about externals' changes on the __[Changes](viewing-user-changes-in-builds.md)__ tab. Note that [versioned settings](storing-project-settings-in-version-control.md) may be affected as enabling this option will cause [timestamps to be added to revisions](https://youtrack.jetbrains.com/issue/TW-75645/).
* _Checkout, but ignore changes_: TeamCity will check out the sources from externals but any changes in externals' source files will not be gathered and displayed on the __[Changes](viewing-user-changes-in-builds.md)__ tab. You can use this option if you have several SVN externals and do not want to get information about any changes made in the externals' source files.   
    
   <note>

   __Build revision number impact__

   If you use the "_Checkout, but ignore changes_" option, TeamCity will always use the latest repository revision as the revision for a checkout (the same revision will be used for the `build.vcs.number` parameter). For other two options, TeamCity takes the revision of the latest detected change as the revision for a checkout.
   </note>

* _Ignore externals_: TeamCity will ignore any configured `svn:externals` property, and thus TeamCity will not check for changes or check out any source file from the SVN externals.   

   <note>

   __Subversion Repository UUID__

   TeamCity relies on Subversion repository UUID as an unique identifier of a repository. If you have 2 different repositories with the same UUID (due to repository copy) TeamCity may function incorrectly, for instance, wrong HEAD revision of an external repository can be checked out.
   </note>

</td></tr><tr>

<td>

HTTPS Connections: Accept non-trusted SSL certificates

</td>

<td>

If enabled, TeamCity is able to connect to SVN servers without properly signed SSL certificate.

</td></tr></table>

<note>

Note that if you have anonymous access for some path within SVN, the entered username will never be used to authenticate when accessing any of its subdirectories. Anonymous access will be used instead. This rule only applies for `svn://` and `http(s)://` protocols; i.e. if you have a build configuration which uses a combination of this VCS Root \+ [VCS Checkout Rules](vcs-checkout-rules.md) referencing a non-restricted path above the restricted one for another build configuration, changes under the restricted path will be ignored _even_ if you specify correct username/password for the VCS Root itself.
</note>

## SSH settings

<table><tr>

<td>

Option

</td>

<td>

Description

</td></tr><tr>

<td>

Private Key File Path

</td>

<td>

Specify the full path to the file that contains the OpenSSH\-formatted private key.

You can also specify an [SSH key uploaded to TeamCity](ssh-keys-management.md)

</td></tr><tr>

<td>

Private Key File Password

</td>

<td>

Enter the password to the SSH private key.

</td></tr><tr>

<td>

SSH Port

</td>

<td>

Specify the port used by SSH.

</td></tr></table>

Only the OpenSSH format is supported for the key. The key in a format unsupported by TeamCity has to be converted to the OpenSSH format (for example, a Putty private key  (`*.ppk`) can be converted using `PuTTYgen.exe`: see __Conversions | Export OpenSSH key__). 

## Checkout on agent settings

<table><tr>

<td>

Option

</td>

<td>

Description

</td></tr><tr>

<td>

Working copy format

</td>

<td>

Select the format of the working copy. Available values for this option are 1.4 through 1.8 (current default).   
This option defines the format version of Subversion files located in `.svn` directories, when the [checkout on agent mode](vcs-checkout-mode.md#agent-checkout) is used. The specified format is important in two cases:

* If you run command-line `svn` commands on the files checked out by TeamCity. For example, if your working copy has version 1.5, you will not be able to use Subversion 1.4 binaries to work with it.
* If you use new Subversion features; for example, file-based externals which were added in Subversion 1.6. Thus, unless you set the working copy format to 1.6, the file-based externals will not be available in the __checkout on agent__ mode.

</td></tr><tr>

<td>

Revert before update

</td>

<td>

If the option is selected, then TeamCity always runs the `svn revert` command before updating sources; that is, it will revert all changes in versioned files located in the checkout directory. When the option is disabled and local modifications are detected during the update process, TeamCity runs the `svn revert` after the update.   
TeamCity does not delete non-versioned files in the working directory during the revert. For deleting non-versioned files, consider using [Swabra](build-files-cleaner-swabra.md).

</td></tr></table>

## Labeling settings

<table><tr>

<td>

Option

</td>

<td>

Description

</td></tr><tr>

<td>

Labeling rules

</td>

<td>

Specify a newline-delimited set of rules that defines the structure of the repository. See the [detailed format description](vcs-labeling.md#Subversion+Labeling+Rules) for more details.

</td></tr></table>

## Authentication for SVN externals

TeamCity does not allow specifying SVN externals authentication parameters explicitly, in user interface. To authenticate on the SVN externals server, the following approaches are used:
* authenticate using the same credentials (username/password) as for the main repository
* authenticate without explicit username/password. In this case, the credentials should be already available to the svn process (usually, they stored in subversion configuration directory). So, this require setting correct "Configuration Directory" or "Default Config Directory" option under [SVN Connection Settings](#SVN+Connection+Settings)

When TeamCity has to connect to a SVN external, it uses the following sequence:
* if the SVN external URL has the same prefix as the main repository (there is a match &gt; 20 characters), TeamCity tries the main repository credentials first, and in case of a failure tries to connect without the username/password (so they picked up from SVN configuration directory)
* if the SVN external URL noticeably differs from the main repository, TeamCity tries to connect without the username/password, and in case of a failure, tries using the credentials from the main repository

## Timeouts

Sometimes, the SVN checkout operation for remote SVN servers may fail with an error like `svn: E175002: timed out waiting for server`. Usually this can happen due to network slowness or the SVN server overload.The timeout values for the connection and for read operations can be configured.

### Connection timeout

Connection timeout is applied when TeamCity creates a connection to the SVN server. The default timeout for this operation is __60 seconds__, and can be specified via the `teamcity.svn.connect.timeout` property, in seconds.

The value of the property is set differently for server-side checkout and agent-side checkout:
{product="tc"}
* Server-side operations — [configure an internal property](server-startup-properties.md#TeamCity+Internal+Properties).
{product="tc"}
* Agent-side checkout — [add a start-up property](configuring-build-agent-startup-properties.md#Agent+Properties).
{product="tc"}
  
For agent-side checkout, [add a start-up property](configuring-build-agent-startup-properties.md#Agent+Properties).
{product="tcc"}

### Read timeout

The read timeout is used when a connection with the SVN server is established, and TeamCity is waiting for the data from the server. The value of the timeout depends on the SVN server access protocol.

#### Subversion server access via HTTP/HTTPS (both server/agent)

For HTTP read timeout TeamCity uses the `http-timeout` setting specified in the `servers` file in the Subversion configuration directory. On Win32 systems, this directory is typically located the Application Data area of the user's profile directory. On Unix/Mac, this directory is usually named `$HOME/.subversion` for the user account who runs the TeamCity server/agent.

If not specified, the default value for the timeout is 1 hour.

#### Subversion server access via svn:// or svn+ssh://

In this case, the read timeout can be specified in seconds via the TeamCity `teamcity.svn.read.timeout` internal property. The default value is 30 minutes.

The value of the property is set differently for a server-side checkout and agent-side checkout:
{product="tc"}
* Server-side operations — [configure an internal property](server-startup-properties.md#TeamCity+Internal+Properties).
{product="tc"}
* Agent-side checkout — [add a start-up property](configuring-build-agent-startup-properties.md#Agent+Properties).
{product="tc"}

For agent-side checkout, [add a start-up property](configuring-build-agent-startup-properties.md#Agent+Properties).
{product="tcc"}

## Ignored changes

TeamCity ignores changes in the `svn:mergeinfo` properties and does not consider a directory _changed_ if only these properties are modified in a given commit.

You can alter the list of ignored SVN properties via the TeamCity [internal property](server-startup-properties.md#TeamCity+Internal+Properties) `teamcity.svn.ignorable.properties`. The value of this property is a comma-separated list of SVN properties; the default value is `svn:mergeinfo`.
{product="tc"}

 <seealso>
        <category ref="admin-guide">
            <a href="configuring-vcs-settings.md">Configuring VCS Settings</a>
            <a href="vcs-checkout-mode.md">VCS Checkout Mode</a>
        </category>
</seealso>
