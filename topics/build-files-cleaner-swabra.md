[//]: # (title: Build Files Cleaner (Swabra))
[//]: # (auxiliary-id: viewpage.actionpageId113084151;Build Files Cleaner (Swabra))

_Swabra_ (originally from the Russian noun '_shvabra_' – a mop, also from the English verb 'swab' \- clean with a mop) is a bundled plugin allowing you to clean files created during the build.

The plugin remembers the state of the file tree after the sources checkout and deletes all the newly added files at the end of the build or at the next build start depending on the settings. Swabra also detects files modified or deleted during the build and reports them to the build log (however, such files are not restored by the plugin). The plugin can also ensure that by the start of the build there are no files modified or deleted by previous builds and initiate clean checkout if such files are detected.

Moreover, Swabra gives the ability to dump processes which lock directory by the end of the build (requires [handle.exe](#Installing+Handle))

Swabra can be added as a build feature to your build configuration regardless of what set of build steps you have. By configuring its options you can enable scanning the checkout directory for newly created, modified and deleted files and enable file locking processes detection.

<tip>

Swabra should be used with the [automatic checkout](vcs-checkout-mode.md) only: after this build feature is configured, it will run __before the first build step__ to remember the state of the file tree after the sources checkout and to restore it after the build.
</tip>

The checkout directory state is saved into a file in the caches directory named `<checkout-directory-name-hash>.snapshot` using the DiskDir format. The path to the checkout directory to be cleaned is saved into the `snapshot.map` file. The snapshot is used later (at the end of the build or at the next build start) to determine which files and folders are newly created, modified or deleted. It is done based on the actual files' presence, last modification data and size comparison with the corresponding records in the snapshot.

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

Select the __Force clean checkout if cannot restore clean directory state__ option to ensure that the checkout directory corresponds to the sources in the repository at the build start. If Swabra detects any modified or deleted files in the checkout directory before the build start, it will enforce [clean checkout](clean-checkout.md). The build will fail if Swabra cannot delete some files created during the previous build.

If this option is disabled, you will only get warnings about modified and deleted files.


</td></tr><tr>

<td>

Paths to monitor


</td>

<td>

Specify a newline\-separated set of `+-:path` rules to define which files and folders are to be involved in the files collection process (by default and until explicitly excluded, the entire checkout directory is monitored). The path can be relative (based on the [build's checkout directory](build-checkout-directory.md)) or absolute and can include Ant\-like wildcards. If no `+:` or `-:` prefix is specified, a rule as treated as "include".

Rules on any path must come in the order __from more general to more concrete__.    
The top level path must always point to a directory. Specifying a directory affects its entire content and sub\-directories. Note also that Swabra is __case\-sensitive.__

Examples:

* `-:*/dir/*` excludes all `dir` folders and their content
* `-:some/dir, +:some/dir/inner` excludes `some/dir` folder and all its content except for the `inner` subfolder and its content
* `+:./*file.txt` includes only the specified file in the build checkout directory into monitoring
* `-:file.txt `excludes the specified file in the build checkout directory from monitoring

<note>

Note that after removing some exclude rules, it is advisable to run a clean checkout.
</note>


</td></tr><tr>

<td>

Locking processes


</td>

<td>

Select whether you want Swabra to inspect the checkout directory for processes locking files in this directory, and what to do with such processes. Note that [`handle.exe`](#Installing+Handle) is required on agents for locking processes detection.


</td></tr><tr>

<td>

Verbose output


</td>

<td>

Check this option to enable detailed logging to build log.


</td></tr></table>

## Default excluded paths

If the build is set up to checkout on the agent, by default Swabra ignores all `.svn`, `.git`, `.hg`, `CVS` folders and their content. To turn off this behaviour, specify an empty `swabra.default.rules` configuration parameter.

## Installing Handle

You can install `handle.exe` from the __Administration__ | __Tools__ page.   
Click the __Install Tool__ button and select __Sysinternals handle.exe__ from the list of tools.   
Specify whether you want to download the latest version of `handle.exe` or upload it manually choosing the path on local machine, and click __Add__. TeamCity will download or upload `handle.exe` and send it to Windows agents.

`handle.exe` is present on agents only after the upgrade.

Note that running handle.exe requires some additional permissions for the build agent user. For more details read [this thread](https://social.technet.microsoft.com/Forums/en-US/e8d97be5-8265-418a-9f44-00a399858bcf/handleexe-amp-user-rights-needed?forum=miscutils).

## Debug options

Generally snapshot file is deleted after files collection. Set the `swabra.preserve.snapshot` system property to preserve snapshots for debugging purposes.


[//]: # (Internal note. Do not delete. "Build Files Cleaner Swabra d36e260.txt")    


## Clean Checkout

Note that Swabra may sometimes cause [clean checkout](clean-checkout.md) to restore clean checkout directory state.   
To avoid unnecessary frequent clean checkouts, always set up identical Swabra build features for build configurations working in the same checkout directory.   
Build configurations work in the same checkout directory if either the same custom checkout directory path or same VCS settings configured for them.

## Development links

See plugin page at [Swabra](https://confluence.jetbrains.com/display/TW/Swabra).

__ __