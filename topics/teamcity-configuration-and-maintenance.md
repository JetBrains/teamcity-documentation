[//]: # (title: TeamCity Configuration and Maintenance)
[//]: # (help-id: TeamCity Configuration and Maintenance)

> Server configuration is only available to the [System Administrators](managing-roles-and-permissions.md#Per-Project+Authorization+Mode).
{instance="tc"}

> Server configuration is only available to the [System Administrators](managing-roles-and-permissions.md#Default+User+Roles).
{instance="tcc"}

To change the server configuration, go to __Administration | Global Settings__. The following blocks of settings are available:
{instance="tc"}

To change the server configuration, go to __Administration | Cloud Server Settings__.
{instance="tcc"}

## TeamCity Configuration
{instance="tc"}

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

The [database](set-up-external-database.md) used by the running TeamCity server.

</td></tr><tr>

<td>

Data Directory

</td>

<td>


The \<[TeamCity Data Directory](teamcity-data-directory.md)\> path with the ability to browse the directory.

</td></tr><tr>

<td>

Artifact directories
{id="artifact-directories"}

</td>

<td>

The list of the root directories used by the TeamCity server to store [Build Artifact](build-artifact.md), build logs and other build data. The default location is `/system/artifacts`. Note that artifacts can also be stored on [external storage](configuring-artifacts-storage.md).

The list can be changed by specifying a new-line delimited list of paths. Absolute and relative (to TeamCity Data Directory) paths are supported. All the specified directories use the same [structure](teamcity-data-directory.md#artifacts).

When looking for build artifacts, the specified locations are searched for the directory corresponding to the build. The search is done in the order the root directories are specified. The first found build artifacts directory is used as the source of artifacts of this build.

Artifacts for the newly starting builds are placed under the first directory in the list.

</td></tr><tr>

<td>

Caches directory

</td>

<td>

The directory containing TeamCity internal caches (of the VCS repository contents, search index, other). You can manually delete files from this directory to clear [caches](teamcity-monitoring-and-diagnostics.md#Caches).

</td></tr><tr>

<td>

Server URL
{id="server-url"}

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

Maximum size in bytes. KB, MB, GB or TB suffixes are allowed.  
\-1 indicates no limit.

</td></tr>

<tr instance="tc">

<td>

Maximum number of artifacts per build

</td>

<td>

Limits the number of artifacts published per build.   
This helps prevent memory consumption problems in case multiple builds publish many artifacts in parallel.


> This setting does not consider [hidden artifacts](build-artifact.md#Hidden+Artifacts), which have their own limit. If the number of **hidden** artifacts your build produces exceeds this separate threshold, TeamCity reports the "Failed to publish artifacts" error. To fix this issue, add the `teamcity.artifact.limit.internalArtifactsNumber=<value>` [internal property](server-startup-properties.md#TeamCity+Internal+Properties) (the default value is 1000).
>
> {type="tip" instance="tc"}

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

</td></tr><tr instance="tc">

<td>

Default VCS changes check interval

</td>

<td>

Set to 60 seconds by default. Specifies how often TeamCity polls the VCS repository for VCS changes. Can be overridden when [configuring VCS roots](configuring-vcs-roots.md).
{id="default-vcs-check-interval"}

Additionally, you can enforce the interval of VCS changes check as a minimum polling interval for all VCS roots on the server. This way, Project Administrators will only be able to set intervals that are larger than the default one. This helps restrict the frequency of polling requests thus offloading the server.

</td></tr><tr>

<td>

Default VCS trigger quiet period

</td>

<td>

Set to 60 seconds by default. Specifies a period (in seconds) that TeamCity maintains between the moment the last VCS change is detected and a build is added into the queue. Can be overridden when [configuring VCS triggers](configuring-vcs-triggers.md).

</td></tr></table>

<anchor name="TeamCityConfigurationandMaintenance-EncryptionSettings"/>

<!--
## Encryption Settings
{id="encryption-settings" help-id="Encryption Settings" instance="tc"}

TeamCity uses the following methods to secure sensitive data:

* All [SSH keys](ssh-keys-management.md) are encrypted using an internal encryption key.
* All [remotely stored secrets](storing-project-settings-in-version-control.md#Storing+Secure+Settings) are scrambled. Secrets' original values are stored in the [TeamCity Data Directory](teamcity-data-directory.md), so their safety relies on the overall security of your environment.


The **Encryption Settings** section lets you define a custom encryption key for both tasks. Use a TeamCity-generated or a custom key to specify the **Custom encryption key** option. TeamCity supports 128-bit keys encoded with Base64.

The custom encryption key is stored in the [`TeamCity Data Directory`](teamcity-data-directory.md)`/config/encryption-config.xml` file, so make sure you do not store this folder in a remote VCS repository that can be accessed by 3rd-party users.

When you generate or enter a new custom encryption key, it becomes the default for encrypting newly added secrets and SSH keys. Existing data remains encrypted with the previously used keys, which are stored in the `encryption-config.xml` file.

```XML
<?xml version="1.0" encoding="UTF-8"?>
<encryption-settings>
  <key value="oldKey1" />
  <key value="oldKey2" />
  <key value="currentKey" default="true" />
</encryption-settings>

```

<warning>

* During backup, your custom keys will be exported along with their projects and automatically available after restoring from backup. Since keys will be stored in the exported files in an open form, make sure the backup files are well-protected.
* TeamCity cannot import projects or restore data from a backup if those projects or backups originate from a server that uses encryption keys absent on this server. To successfully move data from an encrypted server, make sure all of its `encryption-config.xml` keys are added to the corresponding file of the target server.
</warning>
-->




## Encryption Settings
{id="encryption-settings" help-id="Encryption Settings" instance="tc"}

TeamCity uses the following methods to secure sensitive data:

* All [SSH keys](ssh-keys-management.md) are encrypted using an internal encryption key.
* All [remotely stored secrets](storing-project-settings-in-version-control.md#Storing+Secure+Settings) are scrambled. Secrets' original values are stored in the [TeamCity Data Directory](teamcity-data-directory.md), so their safety relies on the overall security of your environment.


The **Encryption Settings** section lets you define a custom encryption key for both tasks. The custom encryption key can be set via a TeamCity UI or (recommended) imported from an environment variable.

<deflist type="full">

<def title="In TeamCity UI" help-id="custom-encryption-key-from-ui">

Enter a 128-bit key encoded with Base64 in the **Custom encryption key** field. You can click a corresponding action to let TeamCity generate a valid key.

Keys specified in TeamCity UI are stored in the [`TeamCity Data Directory`](teamcity-data-directory.md)`/config/encryption-config.xml` file. 

Generating or entering a new encryption key forces TeamCity to use this key for newly encrypted objects. The previous keys are still in use for existing objects and are stored in the `encryption-config.xml` file.

```XML
<?xml version="1.0" encoding="UTF-8"?>
<encryption-settings>
  <key value="oldKey1" />
  <key value="oldKey2" />
  <!--more keys-->  
  <key value="currentKey" default="true" />
</encryption-settings>
```

<warning>

During backup, your custom keys will be exported along with their projects and automatically available after restoring from backup. Since keys will be stored in the exported files in an open form, make sure the backup files are well-protected.

</warning>
</def>

<def title="Import from the environment variable" id="custom-encryption-key-from-env-var" help-id="custom-encryption-key-from-env-var">

If a TeamCity server detects the non-empty `TEAMCITY_ENCRYPTION_KEYS` environment variable when starting, it imports encryption key(s) from this variable and locks the **Custom encryption key** field in the UI.

This is a more secure option since encryption keys are not stored in the `encryption-config.xml` file, making the `Data directory/config` folder more suitable for being stored in a remote VCS repository.

The `TEAMCITY_ENCRYPTION_KEYS` variable stores the currently used and previous encryption keys using a colon as a separator, with the current key being the first:

```Shell
currentKey:oldKey1:oldKey2:oldKey3...
```

The **Generate key** option does not automatically write the generated key to the `TEAMCITY_ENCRYPTION_KEYS` variable, you need to do that manually.
</def>

</deflist>

You can switch from one mode to another at any time. If your server stores keys in the `encryption-config.xml` file, export them to a variable as shown below.

<tabs>

<tab title="Linux/macOS">

```shell
export TEAMCITY_ENCRYPTION_KEYS="currentKey:oldKey1:oldKey2:oldKey3"
```

</tab>

<tab title="Windows">

```shell
setx TEAMCITY_ENCRYPTION_KEYS "newKey:oldKey1:oldKey2:oldKey3" /M
```

</tab>

</tabs>

Similarly, if your server uses the `TEAMCITY_ENCRYPTION_KEYS` variable, move its key values as separate `<key value="key_value"/>` entries to the `encryption-config.xml` file, adding `default="true"` for the currently used key.

<warning>
TeamCity cannot import projects or restore data from a backup if those projects or backups originate from a server that uses encryption keys absent on this server. To successfully move data from an encrypted server, make sure all of its keys from either the <code>encryption-config.xml</code> file or the <code>TEAMCITY_ENCRYPTION_KEYS</code> variable are added to the target server.
</warning>





## Artifacts' Domain Isolation
{id="artifacts-domain-isolation" help-id="Artifacts Domain Isolation" instance="tc"}

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

If enabled, build artifacts will be loaded from a separate domain and any potential malicious content will not be able to communicate with the TeamCity server on behalf of the user viewing this content. This mitigates the risk of XSS attacks through artifacts and of other related vulnerabilities.

Note that this mode requires configuring a dedicated domain for TeamCity. To continue using artifacts for displaying some build results (for example, custom reports), you need to specify this domain's URL below.

</td></tr><tr>

<td>

Artifacts' URL

</td>

<td>
  
Specify a URL to serve build artifacts from. Note the URLs for artifacts isolation and the [TeamCity server](#server-url) must have different hostnames. Using different ports of the same hostname for both resources may lead to various problems, including failed builds and issues with signing in to TeamCity.

On receiving a request for a content of some artifact, TeamCity will redirect your browser to a temporary URL that uses this artifacts' URL as a base. The temporary URL expires after some time to prevent unauthorized access to the artifact. Upon accessing the expired URL, a regular authentication will be performed and a new URL will be generated.  
The same logic applies to the [custom report tabs](including-third-party-reports-in-the-build-results.md) because their content also comes from the build artifacts.

For a personal TeamCity installation, which is accessible via localhost only, a URL like `http://127.0.0.1[:port]/` would be sufficient.

For a TeamCity server used by an organization, a new DNS name, or a [`CNAME`](https://en.wikipedia.org/wiki/CNAME_record), should be registered either for the machine where the server is installed or for a reverse proxy server if TeamCity is accessible through the proxy. The URL with this new hostname should be specified in the artifacts' URL. No extra configuration on the proxy side is required.

Note: as this is a special URL which exists for serving artifacts only, users will not be able to sign in to the TeamCity interface via it.

</td></tr></table>
