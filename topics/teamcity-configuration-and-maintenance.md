[//]: # (title: TeamCity Configuration and Maintenance)
[//]: # (auxiliary-id: TeamCity Configuration and Maintenance)

<tip>

Server configuration is only available to the [System Administrators](role-and-permission.md#Per-Project+Authorization+Mode).
</tip>


__To edit the server configuration:__

On the __Administration__ page, select __Global Settings__. 

The __TeamCity Configuration__ section of the page displays the following information:

<table><tr>

<td>

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

<td>

<anchor name="artifact-directories"/>

Artifact directories


</td>

<td>


The list of the root directories used by the TeamCity server to store [Build Artifact](build-artifact.md), build logs and other build data. The default location is `/system/artifacts`. Note that artifacts can also be stored on [external storage](configuring-artifacts-storage.md).

The list can be changed by specifying a new\-line delimited list of paths. Absolute and relative (to TeamCity Data Directory) paths are supported.All the specified directories use the same [structure](teamcity-data-directory.md#artifacts).

When looking for build artifacts, the specified locations are searched for the directory corresponding to the build. The search is done in the order the root directories are specified. The first found build artifacts directory is used as the source of artifacts of this build.

Artifacts for the newly starting builds are placed under the first directory in the list.


</td></tr><tr>

<td>

Caches directory:

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

 


The __Build Settings section__ allows configuring the following settings:

<table><tr>

<td>

Setting

</td>

<td>

Description

</td></tr><tr>

<td>

Maximum build artifact file size:

</td>

<td>

Maximum size in bytes. KB, MB, GB or TB suffixes are allowed. \-1 indicates no limit

</td></tr><tr>

<td>

Default build execution timeout:

</td>

<td>

Maximum time for a build. Can be overridden when defining [build failure conditions](build-failure-conditions.md).

</td></tr></table>

 


The __Version Control Settings__ controls the following:

 

<table><tr>

<td>

Setting

</td>

<td>

Description

</td></tr><tr>

<td>

Default VCS changes check interval:

</td>

<td>

Set to 60 seconds by default. Specifies how often TeamCity polls the VCS repository for VCS changes. Can be overridden when [configuring VCS roots](configuring-vcs-roots.md).

</td></tr><tr>

<td>

Default VCS trigger quiet period:

</td>

<td>

Set to 60 seconds by default. Specifies a period (in seconds) that TeamCity maintains between the moment the last VCS change is detected and a build is added into the queue.  Can be overridden when [configuring VCS triggers](configuring-vcs-triggers.md).

</td></tr></table>


