[//]: # (title: TeamCity Configuration and Maintenance)
[//]: # (auxiliary-id: TeamCity Configuration and Maintenance)

>Server configuration is only available to the [System Administrators](role-and-permission.md#Per-Project+Authorization+Mode).

To change the server configuration, go to __Administration | Global Settings__. The following blocks of settings are available:
{product="tc"}

To change the server configuration, go to __Administration | Cloud Server Settings__.
{product="tcc"}

## TeamCity Configuration
{product="tc"}

<table><tr>

<td width="100">

Setting

</td>

<td>

Description

</td></tr><tr>

<td>

Database

</td>

<td>

The [database](setting-up-an-external-database.md) used by the running TeamCity server.

</td></tr><tr>

<td>

Data Directory

</td>

<td>


The \<[TeamCity Data Directory](teamcity-data-directory.md)\> path with the ability to browse the directory.

</td></tr><tr>

<td id="artifact-directories">

Artifact directories

</td>

<td>

The list of the root directories used by the TeamCity server to store [Build Artifact](build-artifact.md), build logs and other build data. The default location is `/system/artifacts`. Note that artifacts can also be stored on [external storage](configuring-artifacts-storage.md).

The list can be changed by specifying a new-line delimited list of paths. Absolute and relative (to TeamCity Data Directory) paths are supported.All the specified directories use the same [structure](teamcity-data-directory.md#artifacts).

When looking for build artifacts, the specified locations are searched for the directory corresponding to the build. The search is done in the order the root directories are specified. The first found build artifacts directory is used as the source of artifacts of this build.

Artifacts for the newly starting builds are placed under the first directory in the list.

</td></tr><tr>

<td>

Caches directory

</td>

<td>

The directory containing TeamCity internal caches (of the VCS repository contents, search index, other), which be manually deleted to clear [caches](teamcity-monitoring-and-diagnostics.md#Caches).

</td></tr><tr>

<td>

Server URL

</td>

<td>

The [configurable](configuring-server-url.md) URL of the running TeamCity server.

</td></tr></table>

## Build Settings

<table><tr>

<td width="100">

Setting

</td>

<td>

Description

</td></tr><tr>

<td>

Maximum build artifact file size

</td>

<td>

Maximum size in bytes. KB, MB, GB or TB suffixes are allowed. \-1 indicates no limit

</td></tr>

<tr product="tc">

<td>

Maximum number of artifacts per build

</td>

<td>

Limits the number of artifacts published per build.   
This helps prevent memory consumption problems in case multiple builds publish many artifacts in parallel.

The limit does not consider [hidden artifacts](build-artifact.md#Hidden+Artifacts).

</td></tr>



<tr>

<td>

Default build execution timeout

</td>

<td>

Maximum time for a build. Can be overridden when defining [build failure conditions](build-failure-conditions.md).

</td></tr></table>

## Version Control Settings

<table><tr>

<td width="100">

Setting

</td>

<td>

Description

</td></tr><tr product="tc">

<td>

Default VCS changes check interval

</td>

<td id="default-vcs-check-interval">

Set to 60 seconds by default. Specifies how often TeamCity polls the VCS repository for VCS changes. Can be overridden when [configuring VCS roots](configuring-vcs-roots.md).

Additionally, you can enforce the interval of VCS changes check as a minimum polling interval for all VCS roots on the server. This way, Project Administrators will only be able to set intervals that are larger than the default one. This helps restrict the frequency of polling requests thus offloading the server.

</td></tr><tr>

<td>

Default VCS trigger quiet period

</td>

<td>

Set to 60 seconds by default. Specifies a period (in seconds) that TeamCity maintains between the moment the last VCS change is detected and a build is added into the queue. Can be overridden when [configuring VCS triggers](configuring-vcs-triggers.md).

</td></tr></table>

<anchor name="TeamCityConfigurationandMaintenance-EncryptionSettings"/>

## Encryption Settings
{id="encryption-settings" auxiliary-id="Encryption Settings" product="tc"}

In this block, you can choose how TeamCity will process secure values: either using the default _scrambling strategy_ or by _encrypting them with a custom key_.

By default, TeamCity [stores all secure values](storing-project-settings-in-version-control.md#Storing+Secure+Settings), used in project configuration files, in a scrambled form. The initial values are stored in the [TeamCity Data Directory](teamcity-data-directory.md), and their safety primarily depends on the security of your environment. To minimize the risk of potential malicious actions, TeamCity can encrypt secure values with your custom key.

To use the custom encryption, select the respective option and enter an encryption key. Click __Generate__ to randomly generate it, or enter your own key (128-bit keys encoded with Base64 are supported). After you save the settings, TeamCity will change the strategy from _scrambling secure values_ to _encrypting them with your custom key using the AES algorithm_.

Any existing secure values will remain scrambled. Note that when you change any project parameters, all the project’s secure values are reencrypted automatically using the current key.

You can change the custom key or go back to using the default strategy anytime.

<note>

During backup, your custom keys will be exported along with their projects and automatically available after restoring from backup. Since keys will be stored in the exported files in an open form, make sure the backup files are well-protected.

</note>

## Artifacts' Domain Isolation
{id="artifacts-domain-isolation" auxiliary-id="Artifacts' Domain Isolation" product="tc"}

<table><tr>

<td width="100">

Setting

</td>

<td>

Description

</td></tr><tr>

<td>

Enable isolation protection

</td>

<td>


</td></tr><tr>

<td>

Artifacts' URL

</td>

<td>

</td></tr></table>