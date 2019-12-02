[//]: # (title: Git)
[//]: # (auxiliary-id: Git)
[//]: # (Internal note. Do not delete. "Gitd153e3.txt" "Git \(JetBrains\)d152e3.txt")  
 
TeamCity supports Git out of the box. Git source control with Azure DevOps Services is supported (see authentication notes [below](#Authenticating+to+Azure+DevOps+Services)).

This page contains description of the Git\-specific fields of the VCS root settings.    
For common VCS Root properties, see [this section](configuring-vcs-roots.md#Common+VCS+Root+Properties).

<note>

Git command line client needs to be installed on the agents if the [agent-side checkout](vcs-checkout-mode.md#agent-checkout) is used.
</note>

__Important notes__:

* [Remote Run](remote-run.md) and [Pre-Tested Commit](pre-tested-delayed-commit.md) are supported in the [IntelliJ IDEA](intellij-platform-plugin.md) and [Eclipse](eclipse-plugin.md) plugins; with the [Visual Studio Addin](visual-studio-addin.md) use the [Branch Remote Run Trigger](branch-remote-run-trigger.md).
* Initial Git [checkout](build-checkout-directory.md#Checkout+Process) may take significant time (sometimes hours), depending on the size of your project history, because the whole project history is downloaded during the initial checkout.


On this page:

<tag-list of="chapter" mode="tree" depth="4"/>


## General Settings

<table><tr>

<td width="150">

Option


</td>

<td>

Description


</td></tr><tr>

<td>

Fetch URL


</td>

<td>

The URL of the remote Git repository used for fetching data from the repository.


</td></tr><tr>

<td>

Push URL


</td>

<td>

The URL of the target remote Git repository used for pushing annotated tags created via [VCS labeling](vcs-labeling.md) build feature to the remote repository. If blank, the fetch URL is used.


</td></tr><tr>

<td>

Default branch


</td>

<td>

<anchor name="defaultBranch"/>

Configures [default branch](working-with-feature-branches.md#Default+branch). Parameter references are supported here. Default value is `refs/heads/master`.

<note>

You can configure Git-plugin to fetch all heads by [adding a build configuration parameter](configuring-build-parameters.md#Defining+Build+Parameters+in+Build+Configuration) `teamcity.git.fetchAllHeads=true`.
</note>


</td></tr><tr>

<td>

Branch specification

</td>

<td>

Lists the patterns for branch names, required for [feature branches](working-with-feature-branches.md#Configuring+branches) support. The matched branches are monitored for changes in addition to the default branch. The syntax is similar to checkout rules: `+|-:branch_name`, where `branch_name` is specific to the VCS, i.e. `refs/heads/` in Git (with the optional `*` placeholder).

<include src="branch-filter.md" include-id="OR-syntax-tip"/>


</td></tr><tr>

<td>

Use tags as branches

</td>

<td>

Allows monitoring / checking out git [tags](vcs-labeling.md) as branches making branch specification match tag names as well as branches (for example,`+|-:refs/tags/<tag_name>`). By default, tags are ignored.


</td></tr><tr>

<td>

Username style

</td>

<td>

Defines a way TeamCity reports username for a VCS change. Changing the username style will affect only newly collected changes. Old changes will continue to be stored with the style that was active at the time of collecting changes.

</td></tr><tr>

<td>

Submodules

</td>

<td>

Select whether you want to ignore the submodules, or treat them as a part of the source tree. Submodule repositories should either not require authentication or use the same protocol and accept the same authentication as configured in the VCS root.


</td></tr><tr>

<td>

Username for tags/merge

</td>

<td>

A custom username used for [labeling](vcs-labeling.md).


</td></tr></table>

<anchor name="branchMatchingRules"/>

### Branch Matching Rules


* If the branch matches a line without patterns, the line is used.
* If the branch matches several lines with patterns, the best matching line is used.
* If there are several lines with equal matching, the one below takes precedence.    
Everything that is matched by the wildcard will be shown as a branch name in the TeamCity interface. For example, `+:refs/heads/*` will match `refs/heads/feature1` branch, but in the TeamCity interface you'll see only `feature1` as a branch name.    
The short name of the branch is determined as follows:
* if the line contains no brackets, then full line is used, if there are no patterns or part of line starting with the first pattern\-matched character to the last pattern\-matched character.
* if the line contains brackets, then part of the line within brackets is used. When branches are specified here, and if your build configuration has a VCS trigger and a change is found in some branch, TeamCity will trigger a build in this branch.

### Supported Git Protocols

The following protocols are supported for Git repository URL:
* ssh: (for example, `ssh://git.somwhere.org/repos/test.git`, `ssh://git@git.somwhereElse.org/repos/test.git`, scp\-like syntax: `git@git.somwhere.org:repos/test.git`)

<note>

__Be careful__

The SCP-like syntax requires a colon after the hostname, while a usual SSH URL does not. This is a common source of errors.
</note>

* git: (for example, [`git://git.kernel.org/pub/scm/git/git.git`](git://git.kernel.org/pub/scm/git/git.git))
* http: (for example, [`http://git.somewhere.org/projects/test.git`](http://git.somewhere.org/projects/test.git))
* file: (for example, [`file:///c:/projects/myproject/.git`](file:///c:/projects/myproject/.git))

<note>

__Be careful__

The SCP\-like syntax requires a colon after the hostname, while a usual SSH URL does not. This is a common source of errors.
When you run TeamCity as a Windows service, it cannot access mapped network drives and repositories located on them.
</note>

## Authentication Settings

<table><tr>

<td>

Authentication Method


</td>

<td>

Description


</td></tr><tr>

<td>

Anonymous


</td>

<td>

Select this option to clone a repository with anonymous read access.


</td></tr><tr>

<td>

 Password


</td>

<td>

<anchor name="passwordAuth"/>

Specify a valid __username__ (if there is no username in the clone URL; the username specified here overrides the username from the URL) and a __password__ to be used to clone the repository.    
For the [agent-side checkout](vcs-checkout-mode.md), it is supported __only if Git 1.7.3\+ client__ is installed on the agent. See [TW-18711](http://youtrack.jetbrains.com/issue/TW-18711).    
For Git hosted from Team Foundation Server 2013, specify NTLM credentials here.

You can use a personal access token instead of a password to authenticate to Azure DevOps Services. Note that TeamCity does not support token authentication to hosted [Azure DevOps Server](https://azure.microsoft.com/en-in/services/devops/server/) (formerly, Team Foundation Server) installations.


</td></tr><tr>

<td>

Private Key


</td>

<td>

Valid only for SSH protocol. A private key must be in the __OpenSSH format__. Select one of the options from the __Private Key__ list and specify a valid username (if there is no username in the clone URL; the username specified here overrides the username from the URL).    
Available __Private Key__ options:

* __Uploaded Key__ – uses the key(s) uploaded to the project. See [SSH Keys Management](ssh-keys-management.md) for details.
* __Default Private key__ – uses the keys available on the file system in the default locations used by common ssh tools: the mapping specified in `<USER_HOME>/.ssh/config` if the file exists or the private key file `<USER_HOME>/.ssh/id_rsa` (the files are required to be present on the server and also on the agent if the [agent-side checkout](vcs-checkout-mode.md) is used).
* __Custom Private Key__ – supported __only for [server-side checkout](vcs-checkout-mode.md)__. When this method is used, fill the __Private Key Path__ field with an absolute path to the private key file on the server machine. If required, specify the passphrase to access your SSH key in the corresponding field.


</td></tr></table>

For all available options to connect to GitHub, see the [comment](http://youtrack.jetbrains.com/issue/TW-16194#comment=27-475793).

## Authenticating to Azure DevOps Services

If you use Git source control with Azure DevOps Services, the following options are available to you:

<include src="team-foundation-server.md" include-id="azure-authentication"/>


## Server Settings

These are the settings used in case of the server\-side [checkout](vcs-checkout-mode.md).

<table><tr>

<td width="150">

Option


</td>

<td>

Description


</td></tr><tr>

<td>

<anchor name="serverAutoCRLF"/>

 Convert line\-endings to CRLF


</td>

<td>

Convert line\-endings of all text files to CRLF (works as setting `core.autocrlf=true` in a repository config). When not selected, no line\-endings conversion is performed (works as setting `core.autocrlf=false`). Affects the server\-side checkout only. A change to this property causes a clean checkout.


</td></tr></table>

## Agent Settings

<anchor name="agentSettings"/>

These are the settings used in case of the agent\-side [checkout](vcs-checkout-mode.md).   
Note that the agent\-side checkout has limited support for SSH. The only supported authentication methods are "Default Private Key" and "Uploaded Private Key".   
If you plan to use the [agent-side checkout](vcs-checkout-mode.md), you need to have Git 1.6.4\+ installed on the agents.

<table><tr>

<td width="150">

Option


</td>

<td>

Description


</td></tr><tr>

<td>

Path to git


</td>

<td>

Provide the path to a git executable to be used on the agent. When set to `%env.TEAMCITY_GIT_PATH%`, the automatically detected git will be used, see [Git executable on the agent](#Git+executable+on+the+agent) for details


</td></tr><tr>

<td>

Clean Policy/Clean Files Policy


</td>

<td>

Specify here when the `git clean` command is to run on the agent, and which files are to be removed.


</td></tr><tr>

<td>

<anchor name="use-alternates"/>

Use mirrors


</td>

<td>

When __enabled__ (default), TeamCity clones the repository under the agent's `system\git` directory and uses the mirror as an alternate repository when updating the checkout directory for the build. As a result, this speeds\-up clean checkout (because only the working directory is cleaned), and saves disk space (as there is only one clone of the given Git repository on an agent).

If you __disable__ this option, TeamCity will clone the repository directly under the build's working directory, unless the [`teamcity.git.use.local.mirrors`](#use-local-mirrors) property is set to `true`. 

</td></tr></table>

<tip>

To configure a connection from a TeamCity server running behind a proxy to a remote Git repository, see [this section](how-to.md#Configure+TeamCity+to+Use+Proxy+Server+for+Outgoing+Connections).
</tip>

### Git executable on the agent

<anchor name="agentGitPath"/>

TeamCity needs Git command line client version 1.6.4\+ on the agent in order to use the agent\-side checkout.

The recommended approach is to ensure that the git client is available in `PATH` of the TeamCity agent and leave the "Path to git" setting in the VCS root blank.   
If you only have the git command line on some machines, set "Path to git" setting in the VCS root to the `%env.TEAMCITY_GIT_PATH%` value.

Instead of adding Git to the agent's PATH, you can set the `TEAMCITY_GIT_PATH` environment variable (or `env.TEAMCITY_GIT_PATH` property in the agent's [`buildAgent.properties`](build-agent-configuration.md) file) to the full path to the git executable.

If `TEAMCITY_GIT_PATH` is not defined, the Git agent plugin tries to detect the installed git on the launch of the agent. It first tries to run git from the following locations:
* for Windows \- it tries to run `git.exe` at:
   * `C:\Program Files\Git\bin`
   * `C:\Program Files (x86)\Git\bin`
   * `C:\cygwin\bin`
* for \*nix \- it tries to run `git` at:
   * `/usr/local/bin`
   * `/usr/bin`
   * `/opt/local/bin`
   * `/opt/bin`

If git is not found in any of these locations, it tries to run the git accessible via the `PATH` environment variable.   
If a compatible git (1.6.4\+) is found, it is reported in the `TEAMCITY_GIT_PATH` environment variable. This variable can be used in the __Path to git__ field in the [VCS root](vcs-root.md) settings. As a result, the configuration with such a VCS root will run only on the agents where git was detected or specified in the agent properties.

<anchor name="Git_gc"/>

## Configuring Git Garbage Collection on Server

TeamCity server maintains a local clone for every Git repository used in the VCS roots configured on the server. Since the server performs fetch in those clones many times a day, the clone needs regular optimization to maintain predictable performance. If the Git garbage collection for the clone was not run for a long time, the process of collecting changes may slow down or start to report memory-related errors.
TeamCity can automatically run git gc periodically when native Git client can be found on the server. Inability to run Git GC results in a related health report.

To fix the warning / meet automatic git gc requirements, perform the following:
1. Install a native Git client manually on the TeamCity server.
2. Specify path to the Git executable:
   * Add the drectory with the executable to the `Path` environment variable and restart the server, _or_
   * Set the full path to the directory in the `teamcity.server.git.executable.path` [internal property](configuring-teamcity-server-startup-properties.md#TeamCity+internal+properties) without the server restart.
   
When TeamCity runs Git garbage collection, the details are logged into the [`teamcity-cleanup.log`](teamcity-server-logs.md). If git garbage collection fails, a corresponding warning is displayed.

TeamCity executes Git garbage collection until the total time doesn't exceed 5 hours quota; the quota can be changed using the `teamcity.server.git.gc.quota.minutes` [internal property](configuring-teamcity-server-startup-properties.md#TeamCity+internal+properties).   
Git garbage collection is executed every night at 2 a.m., this can be changed by specifying the internal property with a cron expression like this: `teamcity.git.cleanupCron=0 0 2 * * ?` (restart the server for the property to take effect). If the `git gc` process works slowly and cannot be completed in the allotted time, check the `git-repack` configuration in the default Git configuration files (for example, you can increase `--window-memory` to improve the `git gc` performance).

If the local Git clones need some kind of manual maintenance, you can find them under `<TeamCity Data Directory>/system/caches/git` directory. The `map` file in the directory contains mapping between the repository URL and the subdirectory storing the bare clone of the repository.

## Git LFS

TeamCity supports Git LFS for agent\-side checkout. To use it, install git 1.8.5\+ and Git LFS on the build agent machine. Git LFS should be enabled using the `git lfs install` command (on Windows an elevated command prompt may be needed). More information is available in [Git LFS documentation](https://git-lfs.github.com/). 


<anchor name="internalProperties"/>

## Internal Properties

For Git VCS it is possible to configure the following [internal properties](configuring-teamcity-server-startup-properties.md#TeamCity+internal+properties):

<table><tr>

<td width="300">

Property


</td>

<td width="100">

Default


</td>

<td>

Description


</td></tr><tr>

<td>

`teamcity.git.idle.timeout.seconds`


</td>

<td>

1800


</td>

<td>

The idle timeout for communication with the remote repository. If no data were sent or received during this timeout, the plugin throws a timeout error to prevent hanging of the process forever.


</td></tr><tr>

<td>

`teamcity.git.fetch.timeout`


</td>

<td>

1800


</td>

<td>

(deprecated) Override of `teamcity.git.idle.timeout.seconds` for the `git fetch` operation


</td></tr><tr>

<td>

`teamcity.git.fetch.separate.process`


</td>

<td>

true


</td>

<td>

Defines whether TeamCity runs `git fetch` in a separate process


</td></tr><tr>

<td>

<anchor name="max-memory"/>

`teamcity.git.fetch.process.max.memory`

</td>

<td>

512M

</td>

<td>

<note>

It is recommended to disable this property, so TeamCity can automatically manage the amount of memory used by the `git fecth` process. Instead this option, use [`teamcity.git.fetch.process.max.memory.limit`](#max-memory-limit) to set `-Xmx` for `git fetch`.

</note>

The value of the JVM `-Xmx` parameter for a separate fetch process. Ensure the server machine has enough memory as the memory configured will be used in addition to the main server process and there can be several child processes doing `git fetch`, each using the configured amount of the memory.   
For large repositories requiring heap memory greater than `-Xmx1024m` for Git fetch, [switching to 64-bit Java](installing-and-configuring-the-teamcity-server.md#Setting+Up+Memory+settings+for+TeamCity+Server) may be needed.


</td></tr>

<tr>

<td>

<anchor name="max-memory-limit"/>

`teamcity.git.fetch.process.max.memory.limit`

</td>

<td>


</td>
<td>

The value of the JVM `-Xmx` parameter for a separate fetch process.

TeamCity uses a nested Java process for `git fetch` and automatically selects the memory settings for this process. This property sets a maximum amount of available memory for `get fetch` in each VCS root. It comes into effect only if the `teamcity.git.fetch.process.max.memory` is disabled.

</td>


</tr>


<tr>

<td>

`teamcity.git.monitoring.expiration.timeout.hours`


</td>

<td>

24


</td>

<td>

</td></tr><tr>

<td>

`teamcity.server.git.gc.enabled`


</td>

<td>

false


</td>

<td>

Whether TeamCity should run `git gc` during the server cleanup (native git is used)


</td></tr><tr>

<td>

`teamcity.server.git.executable.path`


</td>

<td>

git


</td>

<td>

The path to the native git executable on the server


</td></tr><tr>

<td>

`teamcity.server.git.gc.quota.minutes`


</td>

<td>

60


</td>

<td>

Maximum amount of time to run `git gc`


</td></tr><tr>

<td>

`teamcity.git.cleanupCron`


</td>

<td>

0 0 2 \* \* ? \*


</td>

<td>

[Cron expression](http://quartz-scheduler.org/documentation/quartz-1.x/tutorials/crontrigger) for the time of a cleanup in git\-plugin, by default \- daily at 2a.m.


</td></tr><tr>

<td>

`teamcity.git.stream.file.threshold.mb`


</td>

<td>

128


</td>

<td>

Threshold in megabytes after which JGit uses streams to inflate objects. Increase it if you have large binary files in the repository and see symptoms described in [TW-14947](http://youtrack.jetbrains.com/issue/TW-14947)


</td></tr><tr>

<td>

`teamcity.git.buildPatchInSeparateProcess`


</td>

<td>

true


</td>

<td>

Git-plugin builds patches in a separate process, set it to false to build patch in the server process. To build patch, git-plugin has to read repository files into memory. To not run out of memory git-plugin reads only objects of size smaller than the threshold, for larger objects streams are used and they can be slow ([TW-14947](http://youtrack.jetbrains.com/issue/TW-14947)). With patch building in a separate process all objects are read into memory. Patch process uses the memory settings of the separate fetch process.


</td></tr><tr>

<td>

`teamcity.git.mirror.expiration.timeout.days`


</td>

<td>

7


</td>

<td>

The number of days after which an unused clone of the repository will be removed from the server machine. The repository is considered unused if there were no TeamCity operations on this repository, like checking for changes or getting the current version. These operations are quite frequent, so 7 days is a reasonably high value.


</td></tr><tr>

<td>

`teamcity.git.commit.debug.info`


</td>

<td>

false


</td>

<td>

Defines whether to log additional debug info on each found commit


</td></tr><tr>

<td>

`teamcity.git.sshProxyType`


</td>

<td>

</td>

<td>

Type of ssh proxy, supported values: `http`, `socks4`, `socks5`. Keep in mind that socks4 proxy cannot resolve remote host names, so if you get an UnknownHostException, either switch to socks5 or add an entry for your git server into the hosts file on the TeamCity server machine.


</td></tr><tr>

<td>

`teamcity.git.sshProxyHost`


</td>

<td>

</td>

<td>

SSH proxy host


</td></tr><tr>

<td>

`teamcity.git.sshProxyPort`


</td>

<td>

</td>

<td>

SSH proxy port


</td></tr><tr>

<td>

`teamcity.git.connectionRetryAttempts`


</td>

<td>

3


</td>

<td>

Number of attempts to establish connection to the remote host for testing connection and getting a current repository state before admitting a failure


</td></tr><tr>

<td>

`teamcity.git.connectionRetryIntervalSeconds`


</td>

<td>

4


</td>

<td>

Interval in seconds between connection attempts


</td></tr>


<tr>

<td>

<anchor name="use-local-mirrors"/>

`teamcity.git.use.local.mirrors`


</td>

<td>

false


</td>

<td>

_TeamCity checks the state of this property only if the "[_Use mirrors_](#use-alternates)" option is disabled in the VCS root settings._

By default, if you disable "_Use mirrors_", TeamCity will clone the repository under the build's working directory.   
Set `teamcity.git.use.local.mirrors` to `true` to clone the repository under the agent's `system\git` directory instead. When running a build, TeamCity will copy the repository from this directory to the build's working directory.


<note>

Copying a repository from a directory located on the same machine is faster than always cloning the repository from the source, so we recommend setting `teamcity.git.use.local.mirrors` to `true` if you disable the "_Use mirrors_" option.

</note>


</td></tr>

<tr>

<td>

`teamcity.git.use.shallow.clone`


</td>

<td>

false

</td>

<td>

_TeamCity checks the state of this property only if the "Use mirrors" option is disabled in the VCS root settings and `teamcity.git.use.local.mirrors` is set to `true`._

If `teamcity.git.use.shallow.clone` is set to `true`, TeamCity will only clone the last version of the repository. This is equal to creating a _shallow_ clone, which means setting the [depth](https://git-scm.com/docs/git-clone#Documentation/git-clone.txt---depthltdepthgt) of the `git clone` operation to `1`.

If you don't need to store any commits, except the last one (for example if you often use disposable cloud instances for running builds), you can optimize the performance of your setup by disabling the "[_Use mirrors_](#use-alternates)" option, and setting the internal properties as follows:

- `teamcity.git.use.local.mirrors=true`
- `teamcity.git.use.shallow.clone=true`

In all other cases we recommend leaving the "_Use mirrors_" option enabled.

</td></tr>

</table>

[Agent configuration](build-agent-configuration.md) for Git:

<table><tr>

<td width="300">

Property


</td>

<td>

Default


</td>

<td>

Description


</td></tr><tr>

<td>

`teamcity.git.use.native.ssh`


</td>

<td>

false


</td>

<td>

When checkout on agent: whether TeamCity should use native SSH implementation.


</td>

<td>

[//]: # (Internal note. Do not delete. "Gitd153e964.txt")    





</td></tr><tr>

<td>

`teamcity.git.idle.timeout.seconds`


</td>

<td>

1800


</td>

<td>

The idle timeout for the `git fetch` operation when the agent-side checkout is used. The fetch is terminated if there is no output from the fetch process during this time. Prior to 8.0.4 the default was 600.


</td></tr></table>


<anchor name="limitations"/>

## Limitations

When using checkout on an agent, a limited subset of [checkout rules](vcs-checkout-rules.md) is supported. Git\-plugin translates some of the checkout rules to the sparse checkout patterns. Only the __rules which do not remap files are supported__:


```Shell

+:some/dir
-:some/dir/subDir
```



An __unsupported__ rule example is `+:some/dir=>some/otherDir`.

## Known Issues

* `java.lang.OutOfMemoryError` while fetching from a repository. Usually occurs when there are large files in the repository and if the [`teamcity.git.fetch.process.max.memory`](#max-memory) internal property is defined. Since TeamCity 2019.2, the recommended approach is to disable this property thus delegating the automatic memory management to TeamCity; you can set a limit for the available memory via the [`teamcity.git.fetch.process.max.memory.limit`](#max-memory-limit) property.   
In earlier versions of TeamCity, you can increase the value of the `teamcity.git.fetch.process.max.memory` property.
* Teamcity running as a Windows service cannot access a network mapped drives, so you cannot work with git repositories located on such drives. To make this work, run TeamCity using `teamcity-server.bat`.
* Inflation using streams in JGit prevents `OutOfMemoryError`, but can be time-consuming (see the related thread at [jgit-dev](http://dev.eclipse.org/mhonarc/lists/jgit-dev/msg00687.html) for details and the [TW-14947](http://youtrack.jetbrains.net/issue/TW-14947) issue related to the problem). If you meet conditions similar to those described in the issue, try to increase `teamcity.git.stream.file.threshold.mb`. Additionally, it is recommended to increase the overall amount of memory dedicated for TeamCity to prevent `OutOfMemoryError`.

## Development Links

Git support is implemented as an open-source plugin. For development links, refer to the [plugin's page](https://plugins.jetbrains.com/plugin/8887-git).

__  __

__See also:__

__Administrator's Guide__: [Branch Remote Run Trigger](branch-remote-run-trigger.md)

__ __
