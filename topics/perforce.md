[//]: # (title: Perforce)
[//]: # (auxiliary-id: Perforce)

This article describes the settings specific to a [Perforce Helix Core](https://www.perforce.com/products/helix-core) VCS root. Common VCS root settings are described [here](configuring-vcs-roots.md#Common+VCS+Root+Properties).

## P4 Connection Settings

<table><tr>

<td width="200">Setting</td><td>Description</td></tr><tr>

<td id="perforcePortOptionDescription">

Port

</td>

<td>

Define the Perforce server address as `host:port`.

For specific environments, the `P4Host` environment variable can be set in the Workspace options [below](#Agent+Checkout+Settings), for any type of checkout.

</td></tr><tr>

<td>

**Stream / Client / Client mapping**

</td>

<td>

Choose the connection option. See the details [below](#Perforce+Stream+Connection).

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

Optionally, specify the password or ticket.

If you enter a value, TeamCity will:
* Set it as the `P4PASSWD` environment variable for executing Perforce commands.
* Use the ticket value as a password for the `p4 login` command, if password-based authentication is disabled on the Perforce server.

If you leave the field empty, TeamCity will rely on an existing P4 ticket of the current user (<path>p4ticket.txt</path>). If the ticket is not present in this file, the failure will occur.  
The ticket file should be present on the TeamCity server machine and on all build agents where TeamCity runs Perforce builds for this VCS root.

</td></tr><tr>

<td id="perforceTicketBasedAuthenticationOptionDescription">

Ticket-based authentication

</td>

<td>

Check this option to enable ticket-based authentication. This option is enabled by default and not displayed.

</td></tr></table>

<anchor name="Perforce-perforceStreamOptionDescription"/>

### Use Perforce Streams

Choose this option to use an existing [Perforce stream](https://www.perforce.com/video-tutorials/vcs/what-perforce-streams). TeamCity will use this stream to prepare the stream-based workspace and adjust the client mapping to it.
{id="perforceStreamOptionDescription"}

This field accepts a deep structure specification, that is paths like `//DEPOTNAME/1/2/n`.

[Build parameters](configuring-build-parameters.md) are supported. For the `StreamAtChange` option, use the _[Label to checkout](#Agent+Checkout+Settings)_ field.

<anchor name="branch-support"/>
<anchor name="branchStreams"/>

The "_Enable feature branches support_" options allow you to specify branch streams to be monitored for changes, in addition to the default one. Enter the branch specification as a newline-delimited set of rules. The syntax is `+|-:stream_name` (with the optional `*` placeholder).

<include src="branch-filter.md" include-id="OR-syntax-tip"/>

#### Limitations
{id="stream-limitations"}

__Checkout rules limitations__  
When Perforce streams are used with the [agent-side checkout mode](vcs-checkout-mode.md#agent-checkout), simple [checkout rules](vcs-checkout-rules.md) like `. => sub/directory` are supported. Exclude checkout rules, multiple include rules, or rules like `aaa=>bbb` are supported only when the "_Create non-stream workspace_" option is enabled (see [below](#Agent+Checkout+Settings)).

__Task stream limitations__  
When task streams are used for feature branches, TeamCity may miss some changes in task streams until a modifying commit is made, which means that merge commits from the parent stream are not detected until a _real_ commit to the task stream is made (see ticket [TW-44765](https://youtrack.jetbrains.com/issue/TW-44765)).

<anchor name="Perforce-perforceClientOptionDescription"/>

### Use Perforce Client

This option allows specifying the client workspace name directly. The workspace must be already created by a Perforce client application (like P4V or P4Win). Only mapping rules from the configured client workspace are used. The client name is ignored.
{id="perforceClientOptionDescription"}

#### Limitations
{id="cc-limitations"}

__Performance impact__  
When this option is used with the [server checkout](vcs-checkout-mode.md#server-checkout) mode, the internal TeamCity source caching on the server side is disabled. This may worsen the performance of [clean checkouts](clean-checkout.md).

__No reuse of snapshot dependencies__  
If this option is enabled, snapshot dependency builds can not be [reused](snapshot-dependencies.md) in a build chain.

<anchor name="Perforce-perforceClientMappingOptionDescription"/>

### Map Perforce Depot to Client

This option allows specifying the mapping of the depot to the client computer.
{id="perforceClientMappingOptionDescription"}

TeamCity will handle file separators according to the OS/platform of the build agent where a build is run. To be able to use a specific line separator for all build agents, choose the _Client_ or _Stream_ option instead (with `LineEnd` specified in Perforce). Alternatively, you can add an [agent requirement](configuring-agent-requirements.md) to run builds only on a specific platform.

Use `team-city-agent` instead of the client name in the mapping.

Example:

```Plain Text
//depot/MPS/... //team-city-agent/...
//depot/MPS/lib/tools/... //team-city-agent/tools/...

```

#### Use ChangeView

To focus on specific revisions, use the [`ChangeView`](https://www.perforce.com/manuals/p4guide/Content/P4Guide/configuration.workspace_view.changeview.html) specification:

```
//depot/... //team-city-agent/...
ChangeView:
    //depot/dir1/…@90
    //depot/dir2/…@automaticLabelWithRevision
```
where `90` is the number of the exact revision of `dir1` and `automaticLabelWithRevision` is the labeled revision of `dir2`. All the other revisions of these directories will not be monitored by this VCS root.

#### Clean Checkout Limitations

Changing client mapping __will not force__ [clean checkout](clean-checkout.md) for the agent-side checkout when:
* A Perforce client name is used: changing the Perforce client mapping for the client will not result in a clean checkout.
* A Perforce stream is used: changing the stream name while keeping the same stream root will not result in a clean checkout.

If the direct client mapping is changed, a clean checkout __will be forced__, unless the `teamcity.perforce.enable-no-clean-checkout` [internal property](server-startup-properties.md) is set on the server.
{product="tc"}

If the direct client mapping is changed, a clean checkout __will be forced__.
{product="tcc"}

<anchor name="Perforce-perforceLabelToCheckout"/>

## Agent Checkout Settings

When the [agent-side checkout](vcs-checkout-mode.md#agent-checkout) is used, TeamCity creates a Perforce workspace for each [checkout directory](build-checkout-directory.md)/[VCS root](vcs-root.md). These workspaces are automatically created when necessary and are automatically deleted after a timeout.

>To customize the name generated by TeamCity: add the `teamcity.perforce.workspace.prefix` [configuration parameter](configuring-build-parameters.md) page with the prefix in the value.

<table><tr>

<td>Setting</td><td>Description</td></tr><tr>

<td id="perforceWorkspaceOptions">

<anchor name="Perforce-perforceWorkspaceOptions"/>

Workspace options

</td>

<td>

Optionally, set the following options for the `[p4 client](http://www.perforce.com/perforce/doc.092/manuals/cmdref/client.html#1040665)` command: `Options`, `SubmitOptions`, and `LineEnd`.

For specific environments, `P4Host` can be specified here for any type of checkout.

</td></tr><tr>

<td>

Create non-stream workspace

</td>

<td>

This option is available only for [Streams](#Use+Perforce+Streams). Enable it to be able to check out using a non-stream workspace based on the stream specification. This allows using checkout rules but makes it impossible to commit to the stream within the build.

</td></tr><tr>

<td>

Run 'p4 clean' for clean-up

</td>

<td>

Enable this option to clean up your workspace from extra files before a build. When enabled, the `p4 clean` command will be run before `p4 sync command`, unless `p4 sync -f` or `p4 sync -p` is used. See the [command reference](http://www.perforce.com/perforce/r14.2/manuals/cmdref/p4_sync.html).

</td></tr><tr>

<td>

Skip the have list update

</td>

<td>

Enable this option to not track files on the Perforce server on synchronization (always transfer all files to the agent, `[p4 sync -p](http://www.perforce.com/perforce/doc.current/manuals/cmdref/p4_sync.html)`).

</td></tr><tr>

<td>

Extra sync options

</td>

<td>

Specify additional `p4 sync` options, like `--parallel`. See the `[command reference](http://www.perforce.com/perforce/r14.2/manuals/cmdref/p4_sync.html)`.

</td></tr></table>

### Perforce Workspace Parameters

TeamCity stores connection variables of each Perforce VCS root in the following parameters:
* `%\vcsRoot.extId.port%`
* `%\vcsRoot.extId.user%`
* `%\vcsRoot.extId.p4client%`

where `extId` is the VCS root’s external ID, specified in its settings.

This way, you can access them from a script separately for each root.

With checkout on an agent, TeamCity provides environment variables describing the Perforce workspace created during the checkout process.  
If several Perforce VCS roots are used for the checkout, the variables are created for the __first__ VCS root. The variables are:
* __P4USER__ — same as the `vcsroot.<VCS_root_ID>.user` [parameter](predefined-build-parameters.md#VCS+Parameters).
* __P4PORT__ — same as the `vcsroot.<VCS_root_ID>.port` [parameter](predefined-build-parameters.md#VCS+Parameters).
* __P4CLIENT__ — same as the `vcsroot.<VCS root ID>.p4client` [parameter](predefined-build-parameters.md#VCS+Parameters), the name of the generated P4 workspace on the build agent.

These variables can be used to perform custom `p4` commands after the checkout.

More information: [Perforce Workspace Handling in TeamCity](perforce-workspace-handling-in-teamcity.md)

### Perforce Proxy Settings

To allow using Perforce proxy with the [agent-side checkout](vcs-checkout-mode.md#agent-checkout), specify the `env.TEAMCITY_P4PORT` environment variable on the [build agent](configure-agent-installation.md), and the agent will take this value as the `P4PORT` value.

## Other Settings

<table>

<tr><td>Setting</td><td>Description</td></tr>

<tr>

<td id="perforcePathtop4ExecutableOptionDescription">

P4 path on the build agent

</td>

<td>

Specify the path to the Perforce command-line client, the `p4.exe` file.

This works only on the agent side, for agent-side checkout. On the agent side, the value of this parameter could be overridden via the `TEAMCITY_P4_PATH` environment variable, if such a variable is set in `[buildAgent.properties](configure-agent-installation.md)` or comes from [build parameters](configuring-build-parameters.md).

For the server, the p4 binary should be present in the `PATH` environment variable of the TeamCity server machine, or can be specified via the `teamcity.perforce.customP4Path` [internal property](server-startup-properties.md#TeamCity+Internal+Properties).
{product="tc"}

To restore the obsolete behavior and specify a semicolon-separated list of allowed p4 paths, set the `teamcity.perforce.p4PathOnServerWhitelist` [internal property](server-startup-properties.md#TeamCity+Internal+Properties).
{product="tc"}

</td></tr><tr>

<td id="perforceLabelToCheckout">

Label/changelist to sync

</td>

<td>

If you need to check out sources not with the latest revision, but with a specific Perforce label (with selective changes), you can specify this label here. For instance, this can be useful to produce a milestone / release build. If the field is left blank, the latest changelist will be used for synchronization.

<warning>

If you use symbolic labels, consider using the [agent-side checkout](vcs-checkout-mode.md#agent-checkout). With the server-side checkout on label, TeamCity will perform full checkout.
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

Enable this option if you have UTF-16 files stored as the `utf16` [Perforce file type](https://www.perforce.com/perforce/doc.current/manuals/p4guide/appendix.filetypes.html) in your project.

You may want to enable this option if you use _server-side checkout and have files of the `utf16`_ [Perforce file type](https://www.perforce.com/perforce/doc.current/manuals/p4guide/appendix.filetypes.html) in your depot. Enable it to keep the `UTF-16` encoding in the checked out files. Otherwise, such files may be converted to `UTF-8` upon checkout.

If you store `UTF-16` files as the `binary` [Perforce file type](https://www.perforce.com/perforce/doc.current/manuals/p4guide/appendix.filetypes.html), they will always be checked out "as is", no conversion will be performed.

</td></tr></table>

## Case-Insensitivity in Perforce Checkout Rules

<include src="vcs-checkout-rules.md" include-id="note-perforce-vcs"/>

<seealso>
        <category ref="admin-guide">
            <a href="vcs-checkout-mode.md">VCS Checkout Mode</a>
        </category>
</seealso>
