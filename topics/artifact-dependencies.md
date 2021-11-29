[//]: # (title: Artifact Dependencies)
[//]: # (auxiliary-id: Artifact Dependencies)

This page details configuration of the TeamCity _[artifact dependencies](dependent-build.md#Artifact+Dependency)_.

The __Build Configuration Settings | Dependencies | Artifact Dependencies__ section allows configuring these dependencies. It is possible to disable a configured dependency temporarily or permanently using the corresponding option in the last column of the _Artifact Dependencies_ list.

## Configuring Artifact Dependencies Using Web UI

To add an artifact dependency to a build configuration:
1. When [editing a build configuration](creating-and-editing-build-configurations.md), open the __Dependencies__ page.
2. Click __Add new artifact dependency__ and specify the following settings:

<table><tr>

<td>

Option

</td>

<td>

Description

</td></tr><tr>

<td>

Depend on

</td>

<td>

Specify the build configuration for the current build configuration to depend on. A dependency can be configured on a previous build of the same build configuration.

</td></tr><tr>

<td>

Get artifacts from

</td>

<td id="artifact-dep-get-from" auxiliary-id="artifact-dep-get-from">

Specify the type of build whose artifacts are to be taken:

* Latest successful build: artifacts will be taken from the successful dependency build with the most recent revision (the latest change ID)
* Latest [pinned build](pinned-build.md): artifacts will be taken from the pinned dependency build with the most recent revision (the latest change ID)
* Latest finished build: if a snapshot dependency is also configured in a build configuration, artifacts will be taken from the build with the same sources as the current build
* Build from the same chain: this option is useful when you have a [snapshot dependency](snapshot-dependencies.md) and want to obtain artifacts from a build with the same sources
* Build with specified build number
* Latest finished build with specified tag

<note>

When selecting the build configuration, take your [clean-up policy settings](teamcity-data-clean-up.md) into account.  
Builds are cleaned and deleted on a regular basis, thus the build configuration could become dependent on a non-existent build. When artifacts are taken from a build with a specific number, then the specific build will not be deleted during clean-up.

</note>

</td></tr><tr>

<td>

Build number

</td>

<td>

_This field appears if you have selected build with specific build number in the_ __Get artifacts from__ list_.   
Specify here the exact [build number](configuring-general-settings.md#Build+Number+Format) of the artifact.

</td></tr><tr>

<td>

Build tag

</td>

<td>

_This field appears if you have selected last finished build with specified tag in the __Get artifacts from__ list_.   
Specify here the tag of the build whose artifacts are to be used. When resolving the dependency, TeamCity will look for the last successful build with the given tag and use its artifacts.

</td></tr><tr>

<td>

Build branch filter

</td>

<td>

_This field appears if the dependency has a [branch specified](working-with-feature-branches.md#Configuring+branches) in the VCS root settings_. Allows setting a [branch filter](branch-filter.md) to limit source builds only to those in the matching branches. If not specified, the default branch is used.

</td></tr><tr>

<td>

Artifacts Rules


</td>

<td>

Refer to [this section](#Artifacts+Rules).

</td></tr><tr>

<td>

Clean destination paths before downloading artifacts

</td>

<td>

Check this option to delete the content of the destination directories before copying artifacts. It will be applied to all inclusive rules.

</td></tr></table>

At any point you can launch a build with [custom artifact dependencies](running-custom-build.md#Promoting+Build).

### Artifacts Rules

Here you specify the artifacts of the source build to be downloaded and the location on the agent they will be downloaded to before the dependent build starts.

A newline-delimited set of rules. Each rule must have the following syntax:

```Plain Text
[+:|-:]SourcePath[!ArchivePath][=>DestinationPath]
```

Each rule specifies the files to be downloaded from the "source" build. The _SourcePath_ should be relative to the artifacts' directory of the "source" build. The path can either identify a specific file, directory, or use wildcards to match multiple files. [Ant-like wildcards](wildcards.md) are supported.   
Downloaded artifacts will keep the "source" directory structure starting with the first `*` or `?`.   
_DestinationPath_ specifies the destination directory on the agent where downloaded artifacts are to be placed. If the path is relative (which is recommended), it will be resolved against the build checkout directory. If needed, the destination directories can be cleaned before downloading artifacts. If the destination path is empty, artifacts will be downloaded directly to the checkout root.

Basic examples:

* Use `a/b/**=>lib` to download all files from `a/b` directory of the source build to the `lib` directory. If there is a `a/b/c/file.txt` file in the source build artifacts, it will be downloaded into the file `lib/c/file.txt`.
* At the same time, artifact dependency `**/*.txt=>lib` will preserve the directories structure: the `a/b/c/file.txt` file from source build artifacts will be downloaded to `lib/a/b/c/file.txt`.

_ArchivePath_ is used to extract downloaded [compressed](configuring-general-settings.md#Artifact+Paths) artifacts. `zip`, `7zip`, `jar`, `tar`, and `tar.gz` are supported.   
_ArchivePath_ follows general rules for _SourcePath_: ant-like wildcards are allowed, the files matched inside the archive will be placed in the directory corresponding to the first wildcard match (relative to destination path)   
For example: `release.zip!*.dll` command will extract all .dll files residing in the root of the `release.zip` artifact.

Archive processing examples:

* `release-*.zip!*.dll=>dlls` will extract `*.dll` from all archives matching the `release-*.zip` pattern to the `dlls` directory.
* `a.zip!**=>destination` will unpack the entire archive saving the path information.
* `a.zip!a/b/c/**/*.dll=>dlls` will extract all `.dll` files from `a/b/c` and its subdirectories into the `dlls` directory, without the `a/b/c` prefix.

`+:` and `-:` can be used to include or exclude specific files from the download or unpacking. As `+:` prefix can be omitted: rules are inclusive by default, and at least one inclusive rule is required. The order of rules is unimportant. For each artifact the most specific rule (the one with the longest prefix before the first wildcard symbol) is applied. When excluding a file, _DestinationPath_ is ignored: the file won't be downloaded at all. Files can also be excluded from archive unpacking. The set of rules applied to the archive content is determined by the set of rules matched by the archive itself.

Exclusive patterns examples:

* `**/*.txt=>texts`  
  `-:bad/exclude.txt`  Will download all `*.txt` files from all directories, excluding `exclude.txt` from the `bad` directory
* `+:release-*.zip!**/*.dll=>dlls`
  `-:release-0.0.1.zip!Bad.dll`  Will download and unpack all `*.dll` files from `release-*.zip` files to the `*.dll`s' directory. The `Bad.dll` file from `release-0.0.1.zip` will be skipped
* `**/*.*=>target`    
  `-:excl/**/*.*`  
  `+:excl/must_have.txt=>target`  Will download all artifacts to the `target` directory. Will not download anything from the `excl` directory, but the file called `must_have.txt`

>Click the ![ArtifactsBrowserIcon.png](ArtifactsBrowserIcon.png) icon to invoke the Artifact Browser. TeamCity will try to locate artifacts according to the specified settings and show them in a tree. Select the required artifacts from the tree and TeamCity will place the paths to them into the input field.

The artifacts placed under the `.teamcity` directory are considered [hidden](build-artifact.md#Hidden+Artifacts). These artifacts are ignored by wildcards by default.   
If you want to include files from the `.teamcity` directory for any purpose, be sure to add the artifact path starting with `.teamcity` explicitly.

Example of accessing hidden artifacts:

* `.teamcity/properties/*.properties`
* `.teamcity/*.*`

By default, downloading artifact dependencies to the [agent work directory](agent-work-directory.md) is allowed, the [agent home directory](agent-home-directory.md) is prohibited. To override the defaults, set custom rules to download artifacts by specifying the comma-separated paths in the [`buildAgent.properties`](configure-agent-installation.md): `teamcity.artifactDependenciesResolution.blackList` and `teamcity.artifactDependenciesResolution.whiteList`. Blacklisting a path forbids artifacts download to the directory unless it is whitelisted.

## Configuring Artifact Dependencies Using Ant Build Script

This section describes how to download TeamCity build artifacts inside the build script. These instructions can also be used to download artifacts from outside of TeamCity.

To handle artifact dependencies between builds, this solution is more complicated than configuring dependencies in the TeamCity UI but allows for greater flexibility. For example, managing dependencies this way will allow you to start a personal build and verify that your build is still compatible with dependencies.

To configure dependencies via Ant build script:

1\. Download Ivy.

<tip>

TeamCity itself acts as an Ivy repository. You can read more about the Ivy dependency manager [here](http://ant.apache.org/ivy/).
</tip>

2\. Add Ivy to the classpath of your build.

3\. Create the `ivyconf.xml` file that contains some meta information about TeamCity repository. This file is to have the following content:


```XML

<ivysettings>
<property name='ivy.checksums' value=''/>
<caches defaultCache="${teamcity.build.tempDir}/.ivy/cache"/>
<statuses>
    <status name='integration' integration='true'/>
</statuses>
<resolvers>
    <url name='teamcity-rep' alwaysCheckExactRevision='yes' checkmodified='true'>
        <ivy pattern='http://YOUR_TEAMCITY_HOST_NAME/httpAuth/repository/download/[module]/[revision]/teamcity-ivy.xml' />
        <artifact pattern='http://YOUR_TEAMCITY_HOST_NAME/httpAuth/repository/download/[module]/[revision]/[artifact](.[ext])' />
    </url>
</resolvers>
<modules>
    <module organisation='.*' name='.*' matcher='regexp' resolver='teamcity-rep' />
</modules>
</ivysettings>

```
4\. Replace `YOUR_TEAMCITY_HOST_NAME` with the host name of your TeamCity server.

5\. Place `ivyconf.xml` in the directory where your `build.xml` will be running.

6\. In the same directory create the `ivy.xml` file defining which artifacts to download and where to put them, for example:


```XML
<ivy-module version="1.3">
  <info organisation="YOUR_ORGANIZATION" module="YOUR_MODULE"/>
  <dependencies>
    <dependency org="org" name="BUILD_TYPE_EXT_ID" rev="BUILD_REVISION">
      <include name="ARTIFACT_FILE_NAME_WITHOUT_EXTENSION" ext="ARTIFACT_FILE_NAME_EXTENSION" matcher="exactOrRegexp"/>
    </dependency>
  </dependencies>
</ivy-module>

```

where:
* `YOUR_ORGANIZATION` replace with the name of your organization.
* `YOUR_MODULE` replace with the name of your project or module where artifacts will be used.
* `BUILD_TYPE_EXT_ID` replace with the [external ID](build-configuration.md#Status+Display+for+Set+of+Build+Configurations) of the build configuration whose artifacts are downloaded.
* `BUILD_REVISION` can be either a build number or one of the following strings: `* latest.lastFinished`
  * `latest.lastSuccessful`
  * `latest.lastPinned`
* `TAG_NAME.tcbuildtag` - last build tagged with the TAG_NAME tag


[//]: # (Internal note. Do not delete. "Artifact Dependenciesd15e580.txt")    

* `ARTIFACT_FILE_NAME_WITHOUT_EXTENSION` filename or regular expression of the artifact without the extension part.
* `ARTIFACT_FILE_NAME_EXTENSION` the extension part of the artifact filename.

7\. Modify your `build.xml` file and add tasks for downloading artifacts, for example (applicable for Ant 1.6 and later):

```XML
<<target name="fetchArtifacts" description="Retrieves artifacts for TeamCity" xmlns:ivy="antlib:org.apache.ivy.ant">
     <taskdef uri="antlib:org.apache.ivy.ant"  resource="org/apache/ivy/ant/antlib.xml"/>
       <classpath>
         <pathelement location="${basedir}/lib/ivy-2.0.jar"/>
         <pathelement location="${basedir}/lib/commons-httpclient-3.0.1.jar"/>
         <pathelement location="${basedir}/lib/commons-logging.jar"/>
         <pathelement location="${basedir}/lib/commons-codec-1.3.jar"/>
       </classpath>
     </taskdef>
     <ivy:configure file="${basedir}/ivyconf.xml" />
     <!--<ivy:cleancache />-->
     <ivy:retrieve pattern="${basedir}/[artifact].[ext]"/>
   </target>

```

<note>

* `commons-httpclient`, `commons-logging`, and `commons-codec` are to be in the `classpath` of Ivy tasks.
* To clean the Ivy cache directory before retrieving dependencies, uncomment the `<ivy:cleancache />` element in the example above.

</note>

Artifacts repository is protected by a basic authentication. To access the artifacts, you need to provide credentials to the &lt;ivy:configure/&gt; task. For example:

```XML
<ivy:configure file="${basedir}/ivyconf.xml"
                 host="TEAMCITY_HOST"
                 realm="TeamCity"
                 username="USER_ID"
                 passwd="PASSWORD"/>

```

where `TEAMCITY_HOST` is hostname or IP address of your TeamCity server (without port and servlet context).   
As `USER_ID/PASSWORD` you can use either username/password of a regular TeamCity user (the user should have corresponding permissions to access artifacts of the source build configuration) or system properties `teamcity.auth.userId/teamcity.auth.password`.

## Build-level authentication

The system properties `teamcity.auth.userId` and `teamcity.auth.password` store automatically generated build-unique values which can be used to authenticate on TeamCity server. The values are valid only during the time the build is running. This generated user has limited permissions which allow build-related operations. The primary intent for the user is to use the authentication to download artifacts from other TeamCity builds within the build script.

Using the properties is preferable to using real user credentials since it allows the server to track the artifacts downloaded by your build. If the artifacts were downloaded by the build configuration artifact dependencies or using the supplied properties, the specific artifacts used by the build will be displayed at the __Dependencies__ tab on the __Build Results__ page. In addition, the builds which were used to get the artifacts from, can be configured to have different [clean-up](teamcity-data-clean-up.md) logic.

<seealso>
        <category ref="concepts">
            <a href="dependent-build.md">Dependent Build</a>
        </category>
</seealso>