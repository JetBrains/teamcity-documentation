[//]: # (title: Pass Data in a Chain)
[//]: # (help-id: Pass Data in a Chain;Artifact Dependencies)


<show-structure for="chapter" depth="2"/>

Members of a [build chain](build-chain.md) can exchange two kinds of data:

* **Files** — through [artifact dependencies](#Artifact+dependencies). An upstream build publishes files; a downstream build downloads them before it starts.
* **Values** — through [output parameters](#Parameters). A name-value pair set in one object is read by another further down the chain.

Both mechanisms flow in the direction of the chain: data moves from upstream to downstream, never the reverse.

## Artifact dependencies

An _artifact dependency_ reuses the output ([artifacts](build-artifact.md)) of one build in another. When configured, TeamCity downloads the required files to the agent before the downstream build starts.

Artifacts can be taken from:

* A build of an upstream object in the same chain.
* A build of a configuration that is not part of the chain.
* A previous build of the same configuration.

> An artifact dependency only transfers files — it does not enforce build order or revision synchronization. To guarantee that artifacts come from a build on the same sources, pair the artifact dependency with a [snapshot dependency](configuring-dependencies.md#Snapshot+dependencies) and choose the **Build from the same chain** source option.
>
{style="tip"}

### Configuring artifact dependencies

For build configurations, add an artifact dependency on the **Dependencies** page of configuration settings.

<img src="dk-add-artifact-dependency.png" width="706" alt="Add artifact dependency"/>

The dialog has the following key settings:

<img src="dk-artifact-dependency-dialog.png" width="706" alt="Artifact dependency dialog"/>

<deflist type="full">
<def title="Depend on">

The configuration whose artifacts you want to use.

</def>
<def title="Get artifacts from" id="artifact-dep-get-from" help-id="artifact-dep-get-from">

Which build of the source configuration to take artifacts from:

* **Latest successful build** / **Latest pinned build** / **Latest finished build** — the most recent matching build.
* **Build from the same chain** — the same-chain build. Use this together with a snapshot dependency. If the source is outside the chain, the build fails.
* **Build from the same chain or last finished** — same as above, but falls back to the latest finished build when the source is not in the chain.
* **Build with specified build number** / **Latest finished build with specified tag** — a specific build, identified by number or tag.

> When pinning to a specific build, account for your [clean-up policy](teamcity-data-clean-up.md): regular builds may be deleted, leaving the dependency pointing at a non-existent build. A build referenced by number is protected from clean-up.
>
{style="note"}

</def>
<def title="Artifact rules">

Which files to download and where to place them. See [Artifact rules](#Artifact+rules) below.

</def>
<def title="Clean destination paths before downloading artifacts">

Deletes the contents of the destination directories before downloading, applied to all inclusive rules.

</def>
</deflist>

In pipelines, artifact dependencies are configured in YAML. This involves two separate pipelines, each with its own configuration file.

First, a job in the **upstream** pipeline publishes the file via a `files-publication` block:

```yaml
# Upstream pipeline
jobs:
  Build:
    steps:
      - type: gradle
        tasks: clean build
    files-publication:
      - path: ./build/libs/todo.jar
        share-with-jobs: true
        publish-artifact: true
```

Then a job in the **downstream** pipeline imports it via a `download-artifacts` block, referencing the upstream pipeline declared in its `dependencies`:

```yaml
# Downstream pipeline
jobs:
  Docker:
    steps:
      - type: script
        script-content: docker build -t myapp:%build.number% .
    download-artifacts:
      - UpstreamPipeline_ID:    # same ID as in the 'dependencies' block
          from: dependency
          artifact-rules: todo.jar=>./build/libs
          clean-destination: true
dependencies:
  - UpstreamPipeline_ID:
      reuse: none
```

### Artifact rules

An artifact rule specifies which artifacts to download and where to store them. Each rule goes on its own line and uses the following syntax:

```
[+:|-:|?:]SourcePath[!ArchivePath][=>DestinationPath]
```

<deflist type="medium">
<def title="Prefix">

* `+:` — include (default; `mylib.dll` and `+:mylib.dll` are identical).
* `-:` — exclude a file or pattern from the download.
* `?:` — optional include. If the file is missing, the build continues with a warning instead of failing.

</def>
<def title="SourcePath">

Path relative to the source build's artifacts directory. Accepts a file, a directory, or [Ant-like wildcards](wildcards.md). The source directory structure is preserved starting from the first wildcard.

</def>
<def title="ArchivePath">

Extracts files from a downloaded archive (`zip`, `7z`, `jar`, `tar`, `tar.gz`, and more). For example, `release.zip!*.dll` extracts the `.dll` files from the archive root.

</def>
<def title="DestinationPath">

Target directory on the agent, relative to the build checkout directory. If omitted, artifacts go to the checkout root. Ignored for `-:` rules.

</def>
</deflist>

Examples:

```
# Download a directory tree into lib/ (a/b/c/file.txt → lib/c/file.txt)
a/b/**=>lib

# Download all text files, preserving structure
**/*.txt=>lib

# Extract all DLLs from matching archives
release-*.zip!*.dll=>dlls

# Download everything except one file
**/*.txt=>texts
-:bad/exclude.txt

# Optional file — build continues even if it is missing
?:output.txt
```

The `?:` prefix is especially useful in [partial chains](run-build-chains.md#Partial+chain+execution): if an upstream build that normally provides a file is skipped, an optional rule keeps the downstream build from failing.

## Parameters

Chain members exchange values through [build parameters](configuring-build-parameters.md). This section is a quick overview — for the full syntax, examples, and edge cases, see [](use-parameters-in-build-chains.md).

A value is shared down the chain by declaring it as an **output parameter** in the upstream object. Input parameters stay private to their owner; output parameters are visible to downstream objects.

<deflist type="full">
<def title="Read an upstream value">

A downstream object reads an upstream output parameter via `dep.<upstream-ID>.<param-name>`.

</def>
<def title="Read another job's value">

Within a single pipeline, a job reads a preceding job's parameter via `job.<job-ID>.<param-name>`.

</def>
<def title="Override an upstream value">

A downstream object writes back to an upstream input parameter via `override.dep.<upstream-ID>.<param-name>`. This is the only case where data flows against the chain direction, and it is resolved before the upstream build starts.

</def>
</deflist>

Output parameters are designed to be read by *other* objects, not by the one that declares them. If a configuration or pipeline references its own output parameter, the reference does not resolve: TeamCity treats the unresolved `%...%` as an [implicit agent requirement](configuring-agent-requirements.md#Implicit+Requirements), and the build fails to start with a "no compatible agents" error. The snippet below demonstrates this.

<include from="pipeline-settings.md" element-id="output-param-in-self"/>

For build configurations, the equivalent declaration uses `params` and `outputParams` blocks. See [](use-parameters-in-build-chains.md) for complete examples of all three patterns above, the `*` wildcard for overriding multiple objects at once, and conflict resolution rules.

## Advanced concepts

### Build-level authentication
{id="build-level-auth" help-id="build-level-auth"}

The system properties `system.teamcity.auth.userId` and `system.teamcity.auth.password` store automatically generated build-unique values which can be used to authenticate on TeamCity server. The values are valid only during the time the build is running. This generated user has limited permissions which allow build-related operations. The primary intent for the user is to use the authentication to download artifacts from other TeamCity builds within the build script.

Using the properties is preferable to using real user credentials since it allows the server to track the artifacts downloaded by your build. If the artifacts were downloaded by the build configuration artifact dependencies or using the supplied properties, the specific artifacts used by the build will be displayed at the __Dependencies__ tab on the __Build Results__ page. In addition, the builds which were used to get the artifacts from, can be configured to have different [clean-up](teamcity-data-clean-up.md) logic.


### Configuring artifact dependencies using Ant build script

This section describes how to download TeamCity build artifacts inside the build script. These instructions can also be used to download artifacts from outside of TeamCity.

To handle artifact dependencies between builds, this solution is more complicated than configuring dependencies in the TeamCity UI but allows for greater flexibility. For example, managing dependencies this way will allow you to start a personal build and verify that your build is still compatible with dependencies.

To configure dependencies via Ant build script:

1. Download Ivy.

    <tip>
    TeamCity itself acts as an Ivy repository. You can read more about the Ivy dependency manager [here](https://ant.apache.org/ivy/).
    </tip>

2. Add Ivy to the classpath of your build.

3. Create the `ivyconf.xml` file that contains some meta information about TeamCity repository. This file is to have the following content:

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

4. Replace `YOUR_TEAMCITY_HOST_NAME` with the host name of your TeamCity server.

5. Place `ivyconf.xml` in the directory where your `build.xml` will be running.

6. In the same directory create the `ivy.xml` file defining which artifacts to download and where to put them, for example:

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
    * `BUILD_TYPE_EXT_ID` replace with the [external ID](changing-build-configuration-status.md#Status+Display+for+Set+of+Build+Configurations) of the build configuration whose artifacts are downloaded.
    * `BUILD_REVISION` can be either a build number or one of the following strings: `* latest.lastFinished`
        * `latest.lastSuccessful`
        * `latest.lastPinned`
    * `TAG_NAME.tcbuildtag` - last build tagged with the TAG_NAME tag
    
    <!--[//]: # (Internal note. Do not delete. "Artifact Dependenciesd15e580.txt")-->
    
    * `ARTIFACT_FILE_NAME_WITHOUT_EXTENSION` filename or regular expression of the artifact without the extension part.
    * `ARTIFACT_FILE_NAME_EXTENSION` the extension part of the artifact filename.

7. Modify your `build.xml` file and add tasks for downloading artifacts, for example (applicable for Ant 1.6 and later):

    ```XML
    <target name="fetchArtifacts" description="Retrieves artifacts for TeamCity" xmlns:ivy="antlib:org.apache.ivy.ant">
     <taskdef uri="antlib:org.apache.ivy.ant"  resource="org/apache/ivy/ant/antlib.xml"/>
       <classpath>
         <pathelement location="${basedir}/lib/ivy-2.0.jar"/>
         <pathelement location="${basedir}/lib/commons-httpclient-3.0.1.jar"/>
         <pathelement location="${basedir}/lib/commons-logging.jar"/>
         <pathelement location="${basedir}/lib/commons-codec-1.3.jar"/>
       </classpath>
     </taskdef>
     <ivy:configure file="${basedir}/ivyconf.xml" />
     <ivy:cleancache />
     <ivy:retrieve pattern="${basedir}/[artifact].[ext]"/>
    </target>
    ```
    
    > * `commons-httpclient`, `commons-logging`, and `commons-codec` are to be in the `classpath` of Ivy tasks.
    > * To clean the Ivy cache directory before retrieving dependencies, uncomment the `<ivy:cleancache />` element in the example above.
    >
    {style="note"}
    
Artifacts repository is protected by a basic authentication. To access the artifacts, you need to provide credentials to the &lt;ivy:configure/&gt; task. For example:
    
```XML
<ivy:configure file="${basedir}/ivyconf.xml"
                 host="TEAMCITY_HOST"
                 realm="TeamCity"
                 username="USER_ID"
                 passwd="PASSWORD"/>

```

where `TEAMCITY_HOST` is hostname or IP address of your TeamCity server (without port and servlet context).   
As `USER_ID`/`PASSWORD` you can use either username/password of a regular TeamCity user (the user should have corresponding permissions to access artifacts of the source build configuration) or system properties `system.teamcity.auth.userId`/`system.teamcity.auth.password`.

<!--


This page details configuration of the TeamCity artifact dependencies that allow you to pass files from one build to another. For example, a typical [](deployment-build-configuration.md) publishes files generated by other (production) configurations.

Artifacts can be passed from:

* Configurations running before the target configuration within the same [](build-chain.md).

* Separate configurations that are not parts of the same build chain with the target configuration.

* Previous builds of the same configuration.

> TeamCity [automatically frees disk space](free-disk-space.md#artifacts-automatic-space) for resolving artifact dependencies based on the artifacts' size. You don't need to configure it manually.
> 
{style="tip"}

## Configuring Artifact Dependencies Using Web UI

To add an artifact dependency to a build configuration:

1. When [editing a build configuration](creating-and-editing-build-configurations.md), open the __Dependencies__ page.

    <img src="dk-add-artifact-dependency.png" width="706" alt="Add artifact dependency"/>

2. Click __Add new artifact dependency__ and specify the following settings:

    <img src="dk-artifact-dependency-dialog.png" width="706" alt="Add artifact dependency dialog"/>

    * **Depend on** — the build configuration for the current build configuration to depend on.
  
    * **Get artifacts from** — the type of build whose artifacts are to be taken:
        {id="artifact-dep-get-from" help-id="artifact-dep-get-from"}

        * **Latest successful build** — Imports artifacts from the most recent successful dependency build revision (the latest change ID).
        * **Latest [pinned build](build-actions.md#Pin+Build)** — Imports artifacts from the pinned dependency build with the most recent revision (the latest change ID).
        * **Latest finished build** — Imports artifacts from the most recent build, regardless of whether it succeeded or failed.
        * **Build from the same chain** — Imports artifacts from the most recent successful build of a configuration or pipeline only if it belongs to the same [build chain](build-chain.md) as the target build. If the artifact source is outside that chain, the build fails.
        * **Build from the same chain or last finished** — Imports artifacts from the most recent successful build of the same-chain configuration or pipeline. If the source and target do not belong to the same build chain, artifacts are taken from the latest finished build instead.
        * **Build with specified build number**
        * **Latest finished build with specified tag**
        
        > When selecting the build configuration, take your [clean-up policy settings](teamcity-data-clean-up.md) into account.
        > Builds are cleaned and deleted on a regular basis, thus the build configuration could become dependent on a non-existent build. When artifacts are taken from a build with a specific number, then the specific build will not be deleted during clean-up.
        > 
        {style="note"}

    * **Build number** — the exact [build number](configuring-general-settings.md#Build+Number+Format) of the artifact. This field is available if you have selected build with specific build number in the_ __Get artifacts from__ list.   

    * **Build tag** — the tag of the build whose artifacts to use. TeamCity retrieves artifacts from the latest finished build (successful or failed) with this tag. This field appears when "Last finished build with specified tag" is selected in the **Get artifacts from list**.

    * **Build branch filter** — allows setting a [branch filter](branch-filter.md) to limit source builds only to those in the matching branches. If not specified, the default branch is used. This field appears if the dependency has a [branch specified](working-with-feature-branches.md#Configuring+Branches) in the VCS root settings.

    * **Artifacts Rules** — string expressions that specify which files and folders should be downloaded, and where they should be stored. See this section for more information: [](#Artifacts+Rules).

    * **Clean destination paths before downloading artifacts** — check this option to delete the content of the destination directories before copying artifacts. It will be applied to all inclusive rules.

3. Click **Save** to add your new dependency.

At any point you can launch a build with [custom artifact dependencies](running-custom-build.md#Other+Ways+to+Start+Custom+Builds).

## Artifacts Rules

An artifact rule specifies which artifacts of the source build should be downloaded and where on the agent storage they should be stored.


>Watch our **video guide** on how to [work with artifact rules](https://www.youtube.com/watch?v=AXITbn7bNyA).

Each individual artifact rule should start from a new line and have the following syntax:

```
[+:|-:|?:]SourcePath[!ArchivePath][=>DestinationPath]
```

The order of a rules is irrelevant. A single artifact can be downloaded to multiple locations via separate rules. If multiple rules target the same artifact file and specify the same download directory, the most specific rule (the one with the longest prefix before the first wildcard symbol) is applied.

> Click the ![ArtifactsBrowserIcon.png](ArtifactsBrowserIcon.png) icon to invoke the Artifact Browser. TeamCity will try to locate artifacts according to the specified settings and show them in a tree. Select the required artifacts from the tree and TeamCity will place the paths to them into the input field.
>
{style="tip"}

### Prefix

* The `+:` prefix specifies a mandatory inclusive artifact dependency. This is the default prefix, meaning that all rules without prefixes are treated as inclusive (the `+:mylib.dll` and `mylib.dll` rules are identical).

* The `-:` prefix allows you to exclude a specific file from the download or unpacking. For example, if directory you wish to pass as an artifact dependency includes a few irrelevant for a build files, you may either go over all required files and include them using the `+:directory/file` syntax, or add the entire directory (`+:directory`) and exclude a few ignored files (`-:directory/junkfile`).

* The `?:` prefix allows you to create optional inclusive dependencies. If a build cannot obtain an artifact referenced in the `+:...` rule, this build fails with the "Failed to resolve artifact dependency" message. The `?:...` rule allows you to run a dependent build anyway (with a warning that the required artifact was not found printed in the build log). You can use the `?:` prefix to label as optional both standalone files (`?:/myfile.txt`) and files from [archives](#Archive+Path) (`?:dist.zip!myfile.txt `). See the following example for more information: [](#Optional+dependency).



### Source Path

The `SourcePath` should be relative to the artifacts' directory of the "source" build. The path can either identify a specific file, directory, or use wildcards to match multiple files. [Ant-like wildcards](wildcards.md) are supported.   

Downloaded artifacts will keep the "source" directory structure starting with the first `*` or `?` wildcard.

### Archive Path

`ArchivePath` is used to extract downloaded [compressed](configuring-general-settings.md#Artifact+Paths) artifacts. `zip`, `7zip`, `jar`, `tar`, and `tar.gz` are supported.   

`ArchivePath` follows general rules for `SourcePath`: ant-like wildcards are allowed, the files matched inside the archive will be placed in the directory corresponding to the first wildcard match (relative to destination path). For example, the `release.zip!*.dll` rule will extract all .dll files residing in the root of the `release.zip` artifact.

### Destination Path

`DestinationPath` specifies the destination directory on the agent where downloaded artifacts are to be placed. If the path is relative (which is recommended), it will be resolved against the build checkout directory. If needed, the destination directories can be cleaned before downloading artifacts. If the destination path is empty, artifacts will be downloaded directly to the checkout root.

Destination paths are ignored for exclusive (starting with the `-:` prefix) rules.

### Examples

#### Download all files from the target directory

To copy all files of from the target `a/b` directory of the source build to the `lib` directory of a dependent build, add the `a/b/**=>lib` rule. Copied files preserve their source hierarchy. For example, the `a/b/c/file.txt` file hosted in a sub-directory will be placed into the `lib/c/file.txt` folder.

#### Download all files that match the pattern

Wildcards allow you to specify patterns and that match multiple files as once. For example, the `**/*.txt=>lib` rule downloads all text files to the agent's `lib` directory.

Note that rules preserve the original directories structure. For example, the `a/b/c/file.txt` file will be stored as `lib/a/b/c/file.txt`.


#### Extract files from an archive

The [](#Archive+Path) portion of an artifact rule allows you to specify which files from the target archive the dependent build should download.

* The `a.zip!**=>destination` rule unpacks the entire archive to the `destination` library. The archive is unpacked as is, with the inner hierarchy of subfolders preserved.

* The `release-*.zip!*.dll=>dlls` rule extracts `*.dll` libraries from all archives matching the `release-*.zip` pattern, and saves these libraries to the `dlls` directory.

* The `a.zip!a/b/c/**/*.dll=>dlls` rule extracts all `.dll` files from `a/b/c` and its subdirectories into the `dlls` directory. Files matching this rule will be stored without the `a/b/c` path.


#### Exclude irrelevant files

Start an artifact rule with the `-:` prefix to tell TeamCity a file or files that matches this pattern should not be downloaded by the dependent build.

* Two following rules result in downloading all text files from all directories, apart from the `exclude.txt` file located in the `bad` directory. Downloaded files are saved to the `texts` folder.

  ```
  **/*.txt=>texts
  -:bad/exclude.txt
  ```


* The following set of rules finds all archives that start with `release`, unpacks its `.dll` libraries, and saves them to the `dlls` directory. The `Bad.dll` file from the `release-0.0.1.zip` archive is skipped.

  ```
  +:release-*.zip!**/*.dll=>dlls
  -:release-0.0.1.zip!Bad.dll
  ```

* The combination below results in dowloading all artifacts to the `target` directory. The dependent build will completely ignore the `excl` directory except for the `excl/must_have.txt` file.

  ```
  **/*.*=>target 
  -:excl/**/*.*
  +:excl/must_have.txt=>target
  ```

#### Download hidden artifacts

The artifacts placed under the `.teamcity` directory are considered [hidden](build-artifact.md#Hidden+Artifacts). These artifacts are ignored by wildcards by default.   
If you want to include files from the `.teamcity` directory for any purpose, be sure to add the artifact path starting with `.teamcity` explicitly, for example:

* `.teamcity/properties/*.properties`
* `.teamcity/*.*`

#### Optional dependency

The following [](kotlin-dsl.md) code illustrates a configuration that produces the `output.txt` artifact file.

```Kotlin
import jetbrains.buildServer.configs.kotlin.*
import jetbrains.buildServer.configs.kotlin.buildSteps.script

object ConfigA : BuildType({
    name = "ConfigA"
    artifactRules = "+:output.txt"
    steps {
        script {
            id = "simpleRunner"
            scriptContent = """
                touch output.txt
                echo "Config A running..." > output.txt
            """.trimIndent()
        }
    }
})
```

The dependent build configuration uses this `output.txt` file in its own script. However, the `if ... else` statement of the script allows for an alternative course of actions in case the target file is missing.


```Kotlin
import jetbrains.buildServer.configs.kotlin.*
import jetbrains.buildServer.configs.kotlin.buildSteps.script

object ConfigB : BuildType({
    name = "ConfigB"
    vcs {
        cleanCheckout = true
    }
    steps {
        script {
            id = "simpleRunner"
            scriptContent = """
                FILE=output.txt   
                
                if [ ! -f ${'$'}FILE ]
                then
                  echo "No source file exists"
                else cat ${'$'}FILE
                fi
            """.trimIndent()
        }
    }

    dependencies {
        dependency(ConfigA) {
            snapshot {
                reuseBuilds = ReuseBuilds.NO
            }
            artifacts {
                artifactRules = "?:output.txt"
            }
        }
    }
})
```

The `?:output.txt` dependency ensures the dependent build can start even if the target file was not found. If this happens, the build log will include a corresponding warning.

<img src="dk-relativeBuild-failed.png" width="706" alt="Optional dependency warning"/>



## Downloading Artifacts to Agent Home Directory

By default, artifacts can be dowloaded only to the [agent work directory](agent-work-directory.md), downloading to the [agent home directory](agent-home-directory.md) is prohibited. To override the defaults, set custom rules to download artifacts by specifying the comma-separated paths in the [`buildAgent.properties`](configure-agent-installation.md): `teamcity.artifactDependenciesResolution.bannedList` and `teamcity.artifactDependenciesResolution.allowedList`. Adding a path to the banned list forbids artifacts download to this directory unless it is present in the allowed list.



## Configuring Artifact Dependencies Using Ant Build Script

This section describes how to download TeamCity build artifacts inside the build script. These instructions can also be used to download artifacts from outside of TeamCity.

To handle artifact dependencies between builds, this solution is more complicated than configuring dependencies in the TeamCity UI but allows for greater flexibility. For example, managing dependencies this way will allow you to start a personal build and verify that your build is still compatible with dependencies.

To configure dependencies via Ant build script:

1\. Download Ivy.

<tip>

TeamCity itself acts as an Ivy repository. You can read more about the Ivy dependency manager [here](https://ant.apache.org/ivy/).
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
* `BUILD_TYPE_EXT_ID` replace with the [external ID](changing-build-configuration-status.md#Status+Display+for+Set+of+Build+Configurations) of the build configuration whose artifacts are downloaded.
* `BUILD_REVISION` can be either a build number or one of the following strings: `* latest.lastFinished`
  * `latest.lastSuccessful`
  * `latest.lastPinned`
* `TAG_NAME.tcbuildtag` - last build tagged with the TAG_NAME tag


[//]: # (Internal note. Do not delete. "Artifact Dependenciesd15e580.txt")   

* `ARTIFACT_FILE_NAME_WITHOUT_EXTENSION` filename or regular expression of the artifact without the extension part.
* `ARTIFACT_FILE_NAME_EXTENSION` the extension part of the artifact filename.

7\. Modify your `build.xml` file and add tasks for downloading artifacts, for example (applicable for Ant 1.6 and later):

```XML
<target name="fetchArtifacts" description="Retrieves artifacts for TeamCity" xmlns:ivy="antlib:org.apache.ivy.ant">
 <taskdef uri="antlib:org.apache.ivy.ant"  resource="org/apache/ivy/ant/antlib.xml"/>
   <classpath>
     <pathelement location="${basedir}/lib/ivy-2.0.jar"/>
     <pathelement location="${basedir}/lib/commons-httpclient-3.0.1.jar"/>
     <pathelement location="${basedir}/lib/commons-logging.jar"/>
     <pathelement location="${basedir}/lib/commons-codec-1.3.jar"/>
   </classpath>
 </taskdef>
 <ivy:configure file="${basedir}/ivyconf.xml" />
 <ivy:cleancache />
 <ivy:retrieve pattern="${basedir}/[artifact].[ext]"/>
</target>
```

> * `commons-httpclient`, `commons-logging`, and `commons-codec` are to be in the `classpath` of Ivy tasks.
> * To clean the Ivy cache directory before retrieving dependencies, uncomment the `<ivy:cleancache />` element in the example above.
>
{style="note"}

Artifacts repository is protected by a basic authentication. To access the artifacts, you need to provide credentials to the &lt;ivy:configure/&gt; task. For example:

```XML
<ivy:configure file="${basedir}/ivyconf.xml"
                 host="TEAMCITY_HOST"
                 realm="TeamCity"
                 username="USER_ID"
                 passwd="PASSWORD"/>

```

where `TEAMCITY_HOST` is hostname or IP address of your TeamCity server (without port and servlet context).   
As `USER_ID`/`PASSWORD` you can use either username/password of a regular TeamCity user (the user should have corresponding permissions to access artifacts of the source build configuration) or system properties `system.teamcity.auth.userId`/`system.teamcity.auth.password`.

## Build-level authentication
{id="build-level-auth" help-id="build-level-auth"}

The system properties `system.teamcity.auth.userId` and `system.teamcity.auth.password` store automatically generated build-unique values which can be used to authenticate on TeamCity server. The values are valid only during the time the build is running. This generated user has limited permissions which allow build-related operations. The primary intent for the user is to use the authentication to download artifacts from other TeamCity builds within the build script.

Using the properties is preferable to using real user credentials since it allows the server to track the artifacts downloaded by your build. If the artifacts were downloaded by the build configuration artifact dependencies or using the supplied properties, the specific artifacts used by the build will be displayed at the __Dependencies__ tab on the __Build Results__ page. In addition, the builds which were used to get the artifacts from, can be configured to have different [clean-up](teamcity-data-clean-up.md) logic.


-->