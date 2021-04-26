[//]: # (title: Perforce)
[//]: # (auxiliary-id: Perforce)

This page contains descriptions of the fields and options available when setting up VCS roots using Perforce.   
Common VCS root properties are described [here](configuring-vcs-roots.md#Common+VCS+Root+Properties).

<note product="tc">

A Perforce client must be installed on the TeamCity server and it should be present in `PATH`. Alternatively, a full path to `p4` could be set via the [internal property](configuring-teamcity-server-startup-properties.md#TeamCity+internal+properties) `teamcity.perforce.customP4Path`.   
If you plan to use the agent-side [checkout mode](vcs-checkout-mode.md#agent-checkout), note that a Perforce client must be installed on the agents, and the path to the p4 executable must be added to the PATH environment variable.  
Also check [TeamCity and Perforce compatibility](perforce-vcs-compatibility.md).
</note>

## P4 Connection Settings

<table><tr>

<td width="200">

Option

</td>

<td>

Description

</td></tr><tr>

<td id="perforcePortOptionDescription">

Port

</td>

<td>

Specify the Perforce server address. The format is `host:port`. For specific environments, P4Host can be specified in the Workspace options [below](#Agent+Checkout+Settings) for any type of checkout.

</td></tr><tr>

<td id="perforceStreamOptionDescription">

Stream

</td>

<td>

Click this radio button to specify an existing Perforce stream. TeamCity will use this stream to prepare the stream-based workspace, and will use the client mapping from such a workspace.

TeamCity supports deeper directory structure within the root depot: depots with a depth of `//DEPOTNAME/1/2/n` can be specified in this field.

[Parameters](configuring-build-parameters.md) are supported. For the `StreamAtChange` option, use the [_Label to checkout_](#Agent+Checkout+Settings) field.

<note>

__Checkout rules limitations__

When Perforce Streams are used with the [agent-side checkout mode](vcs-checkout-mode.md#agent-checkout), simple [checkout rules](vcs-checkout-rules.md) like `. => sub/directory` are supported. Exclude checkout rules, multiple include rules, or rules like `aaa=>bbb` are supported only when the "_Create non-stream workspace_" option is enabled (see below).
</note>

<anchor name="branch-support"/>
<anchor name="branchStreams"/>

__Enable feature branches support (experimental)__ — select this checkbox to specify branch streams you want to be monitored for changes in addition to the default one. Enter / Edit the branch specification as a newline-delimited set of rules. The syntax is `+|-:stream_name` (with the optional `*` placeholder).

<include src="branch-filter.md" include-id="OR-syntax-tip"/>

<note>

__Task stream limitations__

When task streams are used for feature branches, TeamCity may miss some changes in task streams until a modifying commit is made, which means that merge commits from the parent stream are not detected until a 'real' commit to the task stream is made ([TW-44765](https://youtrack.jetbrains.com/issue/TW-44765)).
</note>

</td></tr><tr>

<td id="perforceClientOptionDescription">

Client

</td>

<td>

Click this radiobutton to directly specify the client workspace name. The workspace must be already created by a Perforce client application like P4V or P4Win. Only the mapping rules from the configured client workspace are used. The client name is ignored.

<warning>

__Performance impact__

When this option is used with the [checkout on the server](vcs-checkout-mode.md#server-checkout) mode, the internal TeamCity source caching on the server side is disabled, which may worsen the performance of [clean checkouts](clean-checkout.md). Also, with this option, snapshot dependencies builds are not [reused](snapshot-dependencies.md).
</warning>

</td></tr><tr>

<td id="perforceClientMappingOptionDescription">

<anchor name="Perforce-perforceClientMappingOptionDescription"/>

Client Mapping

</td>

<td>

Click this radiobutton to specify the mapping of the depot to the client computer. If you have __Client mapping__ selected, TeamCity handles file separators according to the OS/platform of the build agent where a build is run. To enforce specific line separator for all build agents, use __Client__ or __Stream__ with the `LineEnd` option specified in Perforce instead of __Client mapping__. Alternatively, you can add an [agent requirement](configuring-agent-requirements.md) to run builds only on a specific platform.

>Use `team-city-agent` instead of the client name in the mapping.

Example:

```Plain Text
//depot/MPS/... //team-city-agent/...
//depot/MPS/lib/tools/... //team-city-agent/tools/...

```

To focus on specific revisions, use the [`ChangeView`](https://www.perforce.com/manuals/p4guide/Content/P4Guide/configuration.workspace_view.changeview.html) specification:

```
//depot/... //team-city-agent/...
ChangeView:
    //depot/dir1/…@90
    //depot/dir2/…@automaticLabelWithRevision
```
where `90` is the number of the exact revision of `dir1` and `automaticLabelWithRevision` is the labeled revision of `dir2`. All the other revisions of these directories will not be monitored by this VCS root.

[Clean Checkout](clean-checkout.md) on a client mapping change __is not__ enforced for the agent-side checkout in the following cases:

* when a Perforce client name is used, changing the Perforce client mapping for the client will not result in a clean checkout
* when a Perforce stream is used, changing the stream name while keeping the same stream root will not result in a clean checkout

If the direct client mapping is changed, a clean checkout __will be forced__ unless the `teamcity.perforce.enable-no-clean-checkout` [internal property](configuring-teamcity-server-startup-properties.md) is set on the server.
{product="tc"}

If the direct client mapping is changed, a clean checkout __will be forced__.
{product="tcc"}

</td></tr><tr>

<td id="perforceUserOptionDescription">

Username

</td>

<td>

Specify the user login name.

</td></tr><tr>

<td id="perforcePasswordOptionDescription">

Password or Ticket

</td>

<td>

Specify the password or ticket.

If this field is specified, TeamCity

* sets this value as P4PASSWD environment variable for executed perforce commands
* uses this field as a password for `p4 login` command if password-based authentication is disabled on the perforce server

If the password is not specified at all, TeamCity relies on an existing p4 ticket for the current user (`p4ticket.txt`), and if the ticket is not present, it will fail.

The ticket file, should be present on all build agents where TeamCity runs perforce builds for this VCS root and on the TeamCity server (as the server also executes perforce commands).

</td></tr><tr>

<td id="perforceTicketBasedAuthenticationOptionDescription">

Ticket-based authentication

</td>

<td>

Check this option to enable ticket-based authentication. This option is enabled by default and not displayed.

</td></tr></table>

## Case-Insensitivity in Checkout Rules

<include src="vcs-checkout-rules.md" include-id="note-perforce-vcs"/>

<anchor name="Perforce-perforceLabelToCheckout"/>

## Agent Checkout Settings

When the [agent-side checkout](vcs-checkout-mode.md#agent-checkout) is used, TeamCity creates a Perforce workspace for each [checkout directory](build-checkout-directory.md)/[VCS root](vcs-root.md). These workspaces are automatically created when necessary and are automatically deleted after some idle time. It is possible to customize the name generated by TeamCity: add the `teamcity.perforce.workspace.prefix` configuration parameter at the [Parameters](configuring-build-parameters.md) page with the prefix in the value.

<table><tr>

<td>

Option

</td>

<td>

Description

</td></tr><tr>

<td id="perforceWorkspaceOptions">

<anchor name="Perforce-perforceWorkspaceOptions"/>

Workspace options

</td>

<td>

If needed, you can set here the following options for the [p4 client](http://www.perforce.com/perforce/doc.092/manuals/cmdref/client.html#1040665) command: `Options`, `SubmitOptions`, and `LineEnd`.    
For specific environments, P4Host can be specified here for any type of checkout.

</td></tr><tr>

<td>

Create non-stream workspace

</td>

<td>

This option is available only when '_Stream_' is selected in [Connection Settings](#P4+Connection+Settings). Enable the option to be able to check out using a non-stream workspace based on the stream specification. This allows using checkout rules but makes it impossible to commit to the stream within the build.

</td></tr><tr>

<td>

Run 'p4 clean' for clean-up

</td>

<td>

Enable this option to clean up your workspace from extra files before a build (since p4 2014.1). When enabled, the `p4 clean` command will be run before `p4 sync command`, unless `p4 sync -f` or `p4 sync -p` is used. See the [command reference](http://www.perforce.com/perforce/r14.2/manuals/cmdref/p4_sync.html).

</td></tr><tr>

<td>

Skip the have list update

</td>

<td>

Enable this option not to track files on the Perforce server on sync (always transfer all files to the agent, [p4 sync -p](http://www.perforce.com/perforce/doc.current/manuals/cmdref/p4_sync.html)).

</td></tr><tr>

<td>

Extra sync options

</td>

<td>

Specify additional `p4 sync` options, like `--parallel`. See [command reference](http://www.perforce.com/perforce/r14.2/manuals/cmdref/p4_sync.html).

</td></tr></table>

### Perforce Workspace Parameters

With checkout on an agent, TeamCity provides environment variables describing the Perforce workspace created during the checkout process.   
If several Perforce VCS Roots are used for the checkout, the variables are created for the first VCS root. The variables are:
* __P4USER__ — same as `vcsroot.<VCS root ID>.user` [parameter](predefined-build-parameters.md#VCS+Properties)
* __P4PORT__ — same as `vcsroot.<VCS root ID>.port` [parameter](predefined-build-parameters.md#VCS+Properties)
* __P4CLIENT__ — name of the generated P4 workspace on the agent
  These variables can be used to perform custom p4 commands after the checkout.

More information: [Perforce Workspace Handling in TeamCity](perforce-workspace-handling-in-teamcity.md)

### Perforce Proxy Settings

To allow using Perforce proxy with the [agent-side checkout](vcs-checkout-mode.md#agent-checkout), specify the `env.TEAMCITY_P4PORT` environment variable on the [build agent](build-agent-configuration.md) and the agent will take this value as the `P4PORT` value.

## Other Settings

<table>

<tr><td></td><td></td></tr>

<tr>

<td id="perforcePathtop4ExecutableOptionDescription">

P4 path on the build agent

</td>

<td>

Specify the path to the Perforce command-line client: `p4.exe` file.

This field works only on the agent side for agent-side checkout. On the agent side, the value of this parameter could be overridden via `TEAMCITY_P4_PATH` environment variable, if such a variable is set in buildAgent.properties or comes from build parameters.

For the server, the p4 binary should be present in the PATH of the TeamCity server or can be specified via the `teamcity.perforce.customP4Path` [internal property](configuring-teamcity-server-startup-properties.md#TeamCity+internal+properties).
{product="tc"}

To restore old behavior, the `teamcity.perforce.p4PathOnServerWhitelist` [internal property](configuring-teamcity-server-startup-properties.md#TeamCity+internal+properties) can be used to specify a semicolon-separated list of allowed p4 paths.
{product="tc"}

</td></tr><tr>

<td id="perforceLabelToCheckout">

Label/changelist to sync

</td>

<td>

If you need to check out sources not with the latest revision, but with a specific Perforce label (with selective changes), you can specify this label here. For instance, this can be useful to produce a milestone/release build. If the field is left blank, the latest changelist will be used for sync.

<warning>

It is recommended to use the [agent-side checkout](vcs-checkout-mode.md#agent-checkout) if you use symbolic labels. With the server-side checkout on label, TeamCity will perform full checkout.
</warning>


</td></tr><tr>

<td id="perforceCharsetOptionDescription">

Charset

</td>

<td>

Select the character set used on the client computer.

</td></tr><tr>

<td id="utf16">

Support UTF-16 encoding

</td>

<td>

Enable this option if you have UTF-16 files stored as `utf16` [Perforce file type](https://www.perforce.com/perforce/doc.current/manuals/p4guide/appendix.filetypes.html) in your project.

You may want to enable this option if you use __server\-side checkout and have files of the `utf16`__ [Perforce file type](https://www.perforce.com/perforce/doc.current/manuals/p4guide/appendix.filetypes.html) in your depot. Enable this flag for the checked out files to be in the `UTF-16` encoding. Otherwise, such files may be converted to `UTF-8` upon checkout.

If you store `UTF-16` files as the `binary` [Perforce file type](https://www.perforce.com/perforce/doc.current/manuals/p4guide/appendix.filetypes.html), they will always be checked out "as is", no conversion will be performed.

</td></tr></table>

## Perforce Jobs Support

For a changelist which was checked in with one or several associated jobs, TeamCity shows a wrench icon ![wrench.png](wrench.png) which allows you to view details of the jobs when clicked or hovered over.

## Logging

All Perforce plugin operations are logged into `teamcity-vcs.log` files with category `jetbrains.buildServer.VCS.P4` (on an agent or on a server, depending on the operation context). The detailed logging can be enabled with [TeamCity Server Logs](teamcity-server-logs.md).
{product="tc"}

All Perforce plugin operations are logged into `teamcity-vcs.log` files with category `jetbrains.buildServer.VCS.P4` (on an agent or on a server, depending on the operation context).
{product="tcc"}

## Perforce Workspace Handling in TeamCity

Refer to a [separate page](perforce-workspace-handling-in-teamcity.md).

## Perforce VCS Compatibility

Refer to a [separate page](perforce-vcs-compatibility.md).

## Perforce Streams as feature branches

Refer to a [separate page](perforce-streams-as-feature-branches.md).

<seealso>
        <category ref="admin-guide">
            <a href="vcs-checkout-mode.md">VCS Checkout Mode</a>
        </category>
</seealso>