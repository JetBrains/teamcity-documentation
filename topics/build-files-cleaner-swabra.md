[//]: # (title: Build Files Cleaner \(Swabra\))
[//]: # (auxiliary-id: viewpage.actionpageId113084151;Build Files Cleaner \(Swabra\))

_Swabra_ is a bundled TeamCity plugin that allows you to add the Swabra [build feature](adding-build-features.md) to your build configurations. This build feature allows you to do the following:

* Remove files generated during a build. The feature creates a list of all files in the checkout directory after the sources checkout is complete. After a build finishes (or before the next build starts), files that are not on this list are automatically removed.

* Detect files modified or deleted during the build. Such files are reported to the build log (however, deleted files are not restored). This allows you to ensure your new builds do not start with certain source files deleted or modified by previous builds, and initiate a clean checkout if this is the case.

* Dump processes that lock directory by the end of the build (requires [handle.exe](#Installing+Handle)).


> Swabra is compatible with any build configuration regardless of its build steps. However, it should be used only when the checkout mode is set to [automatic checkout](vcs-checkout-mode.md): when configured, Swabra runs __before the first build step__ to record the file tree after the sources checkout and to restore it after the build finishes.
>
{style="note"}

Swabra saves the checkout directory state to the `<checkout-directory-name-hash>.snapshot` file in the caches directory using the DiskDir format. The path to the checkout directory to be cleaned is stored in the `snapshot.map` file. The snapshot stores information about existing files, their last modification and size. This information is used when a build finishes (or a new build is about to start) to identify changes.

## Configuring Swabra Options

<table><tr>

<td>

Option


</td>

<td>

Description


</td></tr><tr>

<td>

Files clean-up


</td>

<td>

Select whether you want to perform build files clean-up, and when it will be performed.


</td></tr><tr>

<td>

Clean checkout


</td>

<td>

Select the __Force clean checkout if cannot restore clean directory state__ option to ensure new builds utilize source files that fully match those stored in the remote repository. If Swabra detects files in the checkout directory that were modified or deleted, it enforces the [clean checkout](clean-checkout.md). If Swabra cannot remove any leftover files created by a previous build, the current build will fail.

If this option is disabled, Swabra displays warnings when it finds modified and deleted files, but does not trigger the clean checkout.


</td></tr><tr>

<td>

Paths to monitor


</td>

<td>

Specify a newline-separated set of `+:path` (to include) and `-:path` (to exclude) rules to specify files and directories Swabra should watch. By default, the entire checkout directory is monitored. The path can be relative to the [build's checkout directory](build-checkout-directory.md) or absolute, and can include Ant-like wildcards. If no `+:` or `-:` prefix is specified, a rule as treated as "include".

Rules should be arranged according to their explicitness: less specific rules that point to directories and use wildcards should be placed above more targeted rules that point to individual files. The first rule should always point to a directory.

Swabra is __case-sensitive.__

Examples:

* `-:*/dir/*` excludes all `dir` directories and their content
* `-:some/dir, +:some/dir/inner` excludes the `some/dir` directory and all its content except for the `inner` subdirectory and its content
* `+:./*file.txt` includes only the specified file in the build checkout directory into monitoring
* `-:file.txt `excludes the specified file in the build checkout directory from monitoring

<note>

We recommend that you run a clean checkout after removing exclude (`-:...`) rules.

</note>


</td></tr><tr>

<td>

Locking processes


</td>

<td>

Select whether Swabra should search for processes that lock checkout directory files, and how it should handle such processes. Note that Swabra requires [`handle.exe`](#Installing+Handle) installed on agents to detect such processes.


</td></tr><tr>

<td>

Verbose output


</td>

<td>

Check this option to enable detailed logging to the build log.


</td></tr></table>

## Default excluded paths

If a build employs the agent checkout, Swabra ignores all `.svn`, `.git`, `.hg`, `CVS` directories and their content. To disable this behaviour, specify an empty `swabra.default.rules` configuration parameter.


## Installing Handle
{product="tc"}

You can install `handle.exe` from the __Administration__ | __Tools__ page.  
Click the __Install Tool__ button and select __Sysinternals handle.exe__ from the list of tools.  
Specify whether you want to download the latest version of `handle.exe` or upload it manually choosing the path on local machine, and click __Add__. After the TeamCity server downloads or uploads `handle.exe`, it sends this tool to Windows agents during a scheduled upgrade.

Note that running `handle.exe` [requires administrator privileges](https://learn.microsoft.com/en-us/sysinternals/downloads/handle) for the build agent user.

## Installing Handle
{product="tcc"}

TeamCity Cloud does not currently support manual installation of agent tools. Contact TeamCity Support if you need Handle to be installed in your TeamCity Cloud instance.


## Debug options

Generally snapshot file is deleted after files collection. Set the `swabra.preserve.snapshot` system property to preserve snapshots for debugging purposes.

[//]: # (Internal note. Do not delete. "Build Files Cleaner Swabra d36e260.txt")    

## Clean Checkout

Note that Swabra may sometimes cause [clean checkout](clean-checkout.md) to restore clean checkout directory state.   
To avoid unnecessary frequent clean checkouts, always set up identical Swabra build features for build configurations working in the same checkout directory.   
Build configurations work in the same checkout directory if either the same custom checkout directory path or same VCS settings configured for them.

