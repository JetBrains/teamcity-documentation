[//]: # (title: Perforce)
[//]: # (auxiliary-id: Perforce)

TeamCity can integrate with Perforce to build source projects stored in Perforce Helix Core and ensure their continuous integration and delivery. Learn more about this integration [here](integrating-teamcity-with-perforce.md).

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

Choose the connection mode. See the details [below](#Use+Perforce+Streams).

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
* Set it as the `P4PASSWD` environment variable for executing Perforce commands, _or_
* Use it as the ticket in the `p4 login` command, if password-based authentication is disabled on the Perforce server.

If you leave the field empty, TeamCity will rely on an existing P4 ticket of the current user (<path>p4ticket.txt</path>). If the ticket is not present in this file but required for authentication with Perforce, the failure will occur.  
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

Choose the _Stream_ option to use an existing [Perforce stream](https://www.perforce.com/video-tutorials/vcs/what-perforce-streams). TeamCity will use this stream to prepare the stream-based workspace and adjust the client mapping to it.
{id="perforceStreamOptionDescription"}

Notes on using this mode:
* The _Stream_ field format:
  * Supports a deep structure specification, that is paths like `//DEPOTNAME/1/2/n`.
  * Supports [build parameters](configuring-build-parameters.md).
* To use the `StreamAtChange` option, you need to define _[Label/changelist to sync](#Other+Settings)_.
* When streams are used with the [agent-side checkout mode](vcs-checkout-mode.md#agent-checkout), simple [checkout rules](vcs-checkout-rules.md) like `. => sub/directory` are supported. Exclude checkout rules, multiple include rules, or rules like `aaa=>bbb` are supported only when the "_Create non-stream workspace_" option is enabled (see [below](#Agent+Checkout+Settings)).
* When task streams are used for feature branches, TeamCity may miss some changes in task streams until a modifying commit is made, which means that merge commits from the parent stream are not detected until a _real_ commit to the task stream is made (see ticket [TW-44765](https://youtrack.jetbrains.com/issue/TW-44765)).
* <include from="vcs-checkout-rules.md" element-id="note-perforce-vcs"/>

<anchor name="branch-support"/>
<anchor name="branchStreams"/>

The "_Enable feature branches support_" option allows you to specify branch streams to be monitored for changes, in addition to the default one. [Read more](integrating-teamcity-with-perforce.md#Running+Builds+on+Perforce+Streams) about this functionality.

<anchor name="Perforce-perforceClientOptionDescription"/>

### Use Perforce Client

The _Client_ option allows specifying the client workspace name directly. The workspace must be already created by a Perforce client application (like P4V or P4Win). Only mapping rules from the configured client workspace are used. The client name is ignored.
{id="perforceClientOptionDescription"}

Notes on using this mode:
* When this option is used with the [server-side checkout](vcs-checkout-mode.md#server-checkout), the internal TeamCity source caching on the server side is disabled. This may worsen the performance of [clean checkouts](clean-checkout.md).
* If a build configuration has a [configuration parameter](configuring-build-parameters.md) `teamcity.perforce.agent.reuse.client=true` and uses default checkout rules, TeamCity won't create another Perforce workspace on agent and will try to reuse the existing Perforce client, with the name specified in the Perforce VCS root.

<anchor name="Perforce-perforceClientMappingOptionDescription"/>

### Map Perforce Depot to Client

The _Client mapping_  option allows specifying the mapping of the depot to the client machine.
{id="perforceClientMappingOptionDescription"}

Notes on using this mode:
* The _Client mapping_ field format:
  * TeamCity will handle file separators according to the OS/platform of the build agent where a build is run. To be able to use a specific line separator for all build agents, choose the _Client_ or _Stream_ option instead (with `LineEnd` specified in Perforce). Alternatively, you can add an [agent requirement](configuring-agent-requirements.md) to run builds only on a specific platform.
  * Use `team-city-agent` instead of the client name in the mapping.  
    Example:
    ```Plain Text
    //depot/MPS/... //team-city-agent/...
    //depot/MPS/lib/tools/... //team-city-agent/tools/...
    
    ```
* If the direct client mapping is changed between two builds, a [clean checkout](clean-checkout.md) for the second build __will be forced__, unless the `teamcity.perforce.enable-no-clean-checkout` [internal property](server-startup-properties.md) is set on the server.
{product="tc"}
* If the direct client mapping is changed, a clean checkout __will be forced__.
{product="tcc"}
* Changing client mapping __will not force__ clean checkout for the agent-side checkout when:
  * A Perforce client name is used: changing the Perforce client mapping for the client will not result in a clean checkout.
  * A Perforce stream is used: changing the stream name while keeping the same stream root will not result in a clean checkout.

#### Using ChangeView

To focus on specific revisions, use the [`ChangeView`](https://www.perforce.com/manuals/p4guide/Content/P4Guide/configuration.workspace_view.changeview.html) specification:

```
//depot/... //team-city-agent/...
ChangeView:
    //depot/dir1/…@90
    //depot/dir2/…@automaticLabelWithRevision
```
where `90` is the number of the exact revision of `dir1` and `automaticLabelWithRevision` is the labeled revision of `dir2`. All the other revisions of these directories will not be monitored by this VCS root.

<anchor name="Perforce-perforceLabelToCheckout"/>

## Agent Checkout Settings

When the [agent-side checkout](vcs-checkout-mode.md#agent-checkout) is used, TeamCity creates a Perforce workspace for each [checkout directory](build-checkout-directory.md)/[VCS root](vcs-root.md). These workspaces are automatically created when necessary and are automatically deleted after a timeout. See more details on [Perforce workspace handling](perforce-workspace-handling-in-teamcity.md).

<table><tr>

<td>Setting</td><td>Description</td></tr><tr>

<td id="perforceWorkspaceOptions">

<anchor name="Perforce-perforceWorkspaceOptions"/>

Workspace options

</td>

<td>

Optionally, set the following options for the `[p4 client](https://www.perforce.com/perforce/doc.092/manuals/cmdref/client.html#1040665)` command: `Options`, `SubmitOptions`, and `LineEnd`.

For specific environments, define `P4Host` (supported for any type of checkout). See the details on workspace parameters [below](#Perforce+Workspace+Parameters).

</td></tr><tr>

<td>

Create non-stream workspace

</td>

<td>

_Available only for [Streams](#Use+Perforce+Streams)_

Enable to be able to check out sources using a non-stream workspace based on the stream specification. This allows using [checkout rules](vcs-checkout-rules.md) but makes it impossible to commit to the stream within the build.

</td></tr><tr>

<td>

Run 'p4 clean' for clean-up

</td>

<td>

Enable this option to clean up your workspace from extra files before a build. If enabled, the `p4 clean` command will be run before `p4 sync`, unless `p4 sync -f` or `p4 sync -p` is used. See the [command reference](https://www.perforce.com/manuals/cmdref/Content/CmdRef/p4_sync.html).

</td></tr><tr>

<td>

Skip the have list update

</td>

<td>

Enable this option to not track files on the Perforce server on synchronization (always transfer all files to the agent, `[p4 sync -p](https://www.perforce.com/manuals/cmdref/Content/CmdRef/p4_sync.html)`). If disabled, TeamCity will use `p4 have` to keep the list up to date.

</td></tr><tr>

<td>

Extra sync options

</td>

<td>

Specify additional `p4 sync` options.

If you need to specify the list of options that should be declared after the `sync` command, omit the name of this command. For example:

<ul>
<li>Enter <code>--parallel=threads=5</code> to run the <code>p4 sync --parallel=threads=5</code> command.</li>
</ul>

To specify options that should be declared both before and after the `sync` command, include the command name in this list of extra options. For example:

<ul>
<li>Enter <code>-r3 sync</code> to run the <code>p4 -r3 sync</code> command.</li>
<li>Enter <code>-r4 sync --parallel=threads=5</code> to run the <code>p4 -r4 sync --parallel=threads=5</code> command.</li>
</ul>

See the `[command reference](https://www.perforce.com/manuals/cmdref/Content/CmdRef/p4_sync.html)` for more information.

</td></tr></table>

### Perforce Workspace Parameters

TeamCity stores connection variables of each Perforce VCS root in the following parameters:
* `%\vcsRoot.extId.port%`
* `%\vcsRoot.extId.user%`
* `%\vcsRoot.extId.p4client%`

where `extId` is the VCS root's external ID, specified in its settings.

This way, you can access them from a script separately for each root.

With checkout on an agent, TeamCity provides environment variables describing the Perforce workspace created during the checkout process.  
If several Perforce VCS roots are used for the checkout, the variables are created for the __first__ VCS root. The variables are:
* __P4USER__ — same as the `vcsroot.<VCS_root_ID>.user` [build parameter](predefined-build-parameters.md#VCS+Parameters).
* __P4PORT__ — same as `vcsroot.<VCS_root_ID>.port`.
* __P4CLIENT__ — same as `vcsroot.<VCS root ID>.p4client`, the name of the generated P4 workspace on the build agent.

These variables can be used to perform custom `p4` commands after the checkout.

See [more details](perforce-workspace-handling-in-teamcity.md)

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

Specify the path to the Perforce command-line client (`p4.exe`).

This works only for the [agent-side checkout](vcs-checkout-mode.md#agent-checkout). On the agent side, the value of this parameter could be overridden via the `TEAMCITY_P4_PATH` environment variable, if such a variable is set in `[buildAgent.properties](configure-agent-installation.md)` or comes from [build parameters](configuring-build-parameters.md).

For the server, the p4 binary should be present in the `PATH` environment variable of the TeamCity server machine, or can be specified via the `teamcity.perforce.customP4Path` [internal property](server-startup-properties.md#TeamCity+Internal+Properties).
{product="tc"}

>In the past versions, TeamCity supported a semicolon-separated list of allowed p4 paths. To restore this obsolete behavior, you can set the `teamcity.perforce.p4PathOnServerWhitelist` [internal property](server-startup-properties.md#TeamCity+Internal+Properties).
{product="tc"}

</td></tr><tr>

<td id="perforceLabelToCheckout">

Label/changelist to sync

</td>

<td>

Specify the label if you want to check out sources not with the latest revision, but with a specific Perforce label (with selective changes). For instance, this can be useful to produce a milestone / release build. If this field is empty, the latest changelist will be used for synchronization.

<warning>

If you use symbolic labels, consider using the [agent-side checkout](vcs-checkout-mode.md#agent-checkout). With the server-side checkout on label, TeamCity will perform full checkout.
</warning>

</td></tr><tr>

<td id="perforceCharsetOptionDescription">

Charset

</td>

<td>

Select the character set used on the client machine.

</td></tr><tr>

<td id="utf16">

Support UTF-16 encoding

</td>

<td>

Enable this option if you have UTF-16 files stored as the `utf16` [Perforce file type](https://www.perforce.com/perforce/doc.current/manuals/p4guide/appendix.filetypes.html) in your project.

You may want to enable this option if you use _server-side checkout and have files of the `utf16`_ [Perforce file type](https://www.perforce.com/perforce/doc.current/manuals/p4guide/appendix.filetypes.html) in your depot. Enable it to keep the `UTF-16` encoding in the checked out files. Otherwise, such files may be converted to `UTF-8` upon checkout.

If you store `UTF-16` files as the `binary` [Perforce file type](https://www.perforce.com/perforce/doc.current/manuals/p4guide/appendix.filetypes.html), they will always be checked out "as is", no conversion will be performed.

</td></tr><tr>

<td id="P4trust">

P4 trust

</td>

<td>

If a VCS root of your project connects to Perforce via SSL, TeamCity will automatically establish a trusted connection to it. The `p4 trust` command is sent every time you test a Perforce connection, or a build agent checks out sources from Perforce.

If the SSL certificate on the Perforce server is renewed, the agents need to be configured to trust this new certificate 
with the help of a special parameter. For security reasons, this parameter needs to be removed 
after all agents have checked out the sources.
Set the `teamcity.internal.perforce.forceTrust=true` configuration parameter to the related project or build configuration.

</td></tr></table>

<seealso>
        <category ref="admin-guide">
            <a href="integrating-teamcity-with-perforce.md">Integrating TeamCity with Perforce</a>
            <a href="vcs-checkout-mode.md">VCS Checkout Mode</a>
        </category>
</seealso>
