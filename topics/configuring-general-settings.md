[//]: # (title: Configuring General Settings)
[//]: # (auxiliary-id: Configuring General Settings)

## General Build Configuration Settings

When creating a build configuration, specify the following settings:

<table><tr>

<td>

Setting

</td>

<td>

Description

</td></tr><tr>

<td>

Name

</td>

<td>

The build configuration name.

</td></tr><tr>

<td id="build-configuration-id">

<anchor name="ConfiguringGeneralSettings-BuildconfigurationID"/>

Build Configuration ID

</td>

<td>

A unique [ID](identifier.md) of the configuration across all build configurations and templates in the system automatically generated from the build configuration name, but can also be set manually.    
Make sure you give a globally unique id to the build configuration and prefix it with the project ID.   
After a build configuration is created, its ID can be changed, and it is highly recommended to make corresponding changes to the bookmarked links to the web UI and calls to [REST API](https://www.jetbrains.com/help/teamcity/rest/teamcity-rest-api-documentation.html) using the ID.

</td></tr><tr>

<td>

<anchor name="build-config-description"/>

Description

</td>

<td>

An optional description for the build configuration.

</td></tr><tr>

<td>

<anchor name="ConfiguringGeneralSettings-BuildNumberFormat"/>

Build Number Format

</td>

<td>

A pattern which is resolved and assigned to the [build number](#Build+Number+Format) on the build start.

</td></tr><tr>

<td id="build-counter">

<anchor name="ConfiguringGeneralSettings-buildCounter"/>

Build Counter

</td>

<td>

Specify the counter to be used in build numbering. Each build increases the build counter by 1. Use the _Reset_ link to restore the counter value to 1.

</td></tr>


<tr>

<td id="publish-artifacts">

<anchor name="ConfiguringGeneralSettings-PublishArtifacts"/>

Publish Artifacts

</td>

<td>

Select when to publish artifacts:

* "_Even if build fails_" (default): publish artifacts at the last step of a build if all previous steps have been completed, successfully or not.
* "_Only if build status is successful_": publish artifacts at the last step of a build if all previous steps have been completed successfully. TeamCity checks the current build status on the server before publishing artifacts.
* "_Always, even if build stop command was issued_": publish artifacts for all builds, even for interrupted ones (for example, after the `stop` command was issued or after the time-out, specified in the build failure conditions).

This setting does not affect artifacts publishing configured in a [build script](service-messages.md#Publishing+Artifacts+While+Build+is+in+Progress).

<note>

If the `stop` command is issued during the artifacts publishing, the publishing operation will be stopped regardless of the selected option.

</note>

</td>

</tr>

<tr>

<td>

<anchor name="ConfiguringGeneralSettings-ArtifactPaths"/>

[Artifact Paths](#Artifact+Paths)

</td>

<td>

Patterns to define artifacts of a build. After the first build is run, you can browse the agent [checkout directory](build-checkout-directory.md) to configure artifacts paths.

</td></tr><tr>

<td>

<anchor name="ConfiguringGeneralSettings-buildOptions"/>

[Build Options](#Build+Options)

</td>

<td>

Specify additional options for the builds of this build configuration.   

* [Hanging Build Detection](#Hanging+Build+Detection)
* [Allow Triggering Personal Builds](#Allow+Triggering+Personal+Builds)
* [Enable Status Widget](#Enable+Status+Widget)
    * [HTML Status Widget](#HTML+Status+Widget)
* [Limit Number of Simultaneously Running Builds](#Limit+Number+of+Simultaneously+Running+Builds)

</td></tr></table>

### Build Number Format

In the _Build number format_ field you can specify a pattern which is resolved and assigned to the <emphasis tooltip="build-number">build number</emphasis> on the build start.

[//]: # (Internal note. Do not delete. "Configuring General Settingsd79e124.txt")

The following substitutions are supported in the pattern:

<table><tr>

<td>

Pattern

</td>

<td>

Description

</td></tr><tr>

<td>

`%\build.counter%`

</td>

<td>

The build counter unique for each build configuration. It is maintained by TeamCity and will resolve to a next integer value on each new build start. The current value of the counter can be edited in the _[Build counter](#build-counter)_ field.

</td></tr><tr>

<td>

`%build.vcs.number.<VCS_root_name>%`

</td>

<td>

The revision used for the build of the VCS root with `<VCS_root_name>` name. [Read more](predefined-build-parameters.md) on the property.

</td></tr><tr>

<td>

`%\property.name%`

</td>

<td>

A value of the build property with the corresponding name. All the [Predefined Build Parameters](predefined-build-parameters.md) are supported (including [Reference-only server properties](predefined-build-parameters.md#Predefined+Configuration+Parameters)).

</td></tr></table>

>A build number format example: `1.0.%\build.counter%.%\build.vcs.number.My_Project_svn%`.

Though not required, it is still highly recommended ensuring the build numbers are unique. Please include the build counter in the build number and do not reset the build counter to lesser values. It is also possible to change the build number from within your build script. For details, refer to [Build Script Interaction with TeamCity](service-messages.md#Reporting+Build+Number).

<anchor name="ConfiguringGeneralSettings-artifactPaths"/>

## Artifact Paths

[Build artifacts](build-artifact.md) are files produced by the build which are stored on TeamCity server and can be downloaded from the TeamCity UI or used as artifact dependencies by other builds. On the __General Settings__ page of the build configuration, you can specify patterns for the files on the agent which will be uploaded to the server after the build.

If you have a finished build on an agent, you can use the checkout directory browser ![chechoutdirBrowser.png](chechoutdirBrowser.png) (which lists the checkout directory content on the agent) and select artifacts from the tree. TeamCity will place the paths to them into the input field.

The _Artifact Paths_ field supports relative (to the build checkout directory) and absolute paths. Using relative paths is recommended. You can specify exact file paths or patterns, one per line or comma-separated. Patterns support the `*` and `**` wildcards (see below). Each line can be of the form `[+:]source [=> target]` to include and `-:source [=> target]` to exclude files or directories to publish as build artifacts. The parts enclosed in square brackets are optional. Rules are grouped by the right part and are applied in the order of appearance:

```Shell

+:**/* => target_directory
-:directory1 => target_directory

```

will tell TeamCity to publish all files except for `directory1` into the `target_directory`.

Line format description:

```Shell

file_name|directory_name|wildcard [ => target_directory|target_archive ]

```

Note that although absolute paths are supported in the source part, it is recommended to use paths relative to the [build checkout directory](build-checkout-directory.md).

* `file_name` — to publish the file. The name should be relative to the [build checkout directory](build-checkout-directory.md).
* `directory_name` — to publish all the files and subdirectories within the directory specified. The directory name should be a path relative to the [build checkout directory](build-checkout-directory.md). The files will be published preserving the directories structure under the directory specified (the directory itself will not be included).
* `wildcard` — to publish files matching [Ant-like wildcard](wildcards.md) pattern (only `*` and `**` wildcards are supported). The wildcard should represent a path relative to the build checkout directory. The files will be published preserving the structure of the directories matched by the wildcard (directories matched by "static" text will not be created). That is, TeamCity will create directories starting from the first occurrence of the wildcard in the pattern.
* You can use [build parameters](configuring-build-parameters.md) in the artifacts' specification. For example, use `mylib-%\system.build.number%.zip` to refer to a file with the build number in the name.

The optional part starting with the `=>` symbols and followed by the target directory name can be used to publish the files into the specified target directory. If the target directory is omitted, the files are published in the root of the build artifacts. You can use `.` (dot) as a reference to the build checkout directory.   
The target paths cannot be absolute. Non-relative paths will produce errors during the build. 
* `target_directory` — (optional) the directory in the resulting build's artifacts that will contain the files determined by the left part of the pattern. 
* `target_archive` — (optional) the path to the archive to be created by TeamCity by packing build artifacts determined in the left part of the pattern. TeamCity treats the right part of the pattern as `target_archive` whenever it ends with a [supported archive extension](patterns-for-accessing-build-artifacts.md#Obtaining+Artifacts+from+an+Archive), that is `.zip`, `.7z`, `.jar`, `.tar.gz`, or `.tgz`.

>There is a known issue with inability to exclude artifact paths specified with the `**` wildcard: for example, `-:**/directory`. Such exclude rules will be ignored by TeamCity. As a workaround, use the `-:**/directory/**` format instead. See the [related issue](https://youtrack.jetbrains.com/issue/TW-59469) in our tracker.
> 
{style="warning"}

### Publishing Symlinks

A symbolic link (symlink or soft link) is a Linux file that points to other files or directories and represents their absolute or relative path. If a directory that you need to publish as a build artifact contains symlinks, you can choose one of two possible modes:

* Published archives include symlinks as symlinks. This is the default behavior. You can explicitly decorate an artifact path with the `teamcity:symbolicLinks` attribute to force this behavior.

  ```Plain Text
  #teamcity:symbolicLinks=as-is
  %teamcity.build.checkoutDir%/build=>build.zip
  ```
  {interpolate-variables="false"}

* Published archives include files and folders referenced by symlinks. To enable this behavior, decorate an artifact rule with the `teamcity:symbolicLinks` attribute as follows. 

  ```Plain Text
  #teamcity:symbolicLinks=inline
  %teamcity.build.checkoutDir%/build=>build.zip
  ```
  {interpolate-variables="false"}
  
Note that attributes affect only artifact publishing rules declared directly beneath them. For example, in the sample below only **Archive_A** will contain files and folders referenced by symlinks. **Archive_B** will employ the default behavior and include symlinks as files.

```Plain Text
#teamcity:symbolicLinks=inline
Dir_A=>Archive_A.zip
Dir_B=>Archive_B.zip
```

### Artifacts Paths Examples

* `install.zip` — publish a file named `install.zip` in the build artifacts.
* `dist` — publish the content of the dist directory.
* `target/*.jar` — publish all `jar` files in the target directory.
* `target/**/*.txt=> docs` — publish all the txt files found in the target directory and its subdirectories. The files will be available in the build artifacts under the `docs` directory.
* `reports => reports, distrib/idea*.zip` — publish reports directory as reports and files matching `idea*.zip` from the `distrib` directory into the artifacts root.
* Relative paths inside a zip archive can be used, if needed: `results\result1\Dir1\Dir2 => archive.zip!results/result1/Dir1`.
* The same `target_archive` name can be used multiple times, for example: 
   * `+:*/*.html => report.zip` 
   * `+:*/*.css => report.zip!/css/`
   * `-:*/*.txt => report.zip`

<anchor name="buildOptions"/>

## Build Options

The following options are available to build configurations:

### Hanging Build Detection

Select the _Enable hanging build detection_ option to detect probably "hanging" builds. A build is considered to be "hanging" if its run time significantly exceeds the estimated __average run time__ and if the build has not sent any messages since the estimation was exceeded. To properly detect hanging builds, TeamCity has to estimate the average time builds run based on several builds. Thus, if you have a new build configuration, it may make sense to enable this feature after a couple of builds have run, so that TeamCity would have enough information to estimate the average run time.

### Allow Triggering Personal Builds

You can restrict running [personal builds](personal-build.md) by unchecking the __allow triggering personal builds__ option (on by default).

<anchor name="ConfiguringGeneralSettings-EnableStatusWidget"/>

### Enable Status Widget

This option enables retrieving the status and basic details of the last build in the build configuration without requiring any user authentication. Note that this also allows getting the status of any specific build in the build configuration (however, builds cannot be listed and no other information except the build status (`success/failure/internal error/cancelled`) is available).

The status can be retrieved via the HTML status widget described [below](#HTML+Status+Widget), or via a single icon: with the help of [REST API](https://www.jetbrains.com/help/teamcity/rest/get-build-status-icon.html) or via the __Actions__ menu in __Build Configuration Home__.

### HTML Status Widget

This feature allows you to get an overview of the current project status on your company's website, wiki, Confluence or any other web page.  
When the __Enable status widget__ option is enabled, an HTML snippet can be included into an external web page and will display the current build configuration status.  
For build status icon as a single image, check [REST build status icon](https://www.jetbrains.com/help/teamcity/rest/get-build-status-icon.html).

The following build process information is provided by the status widget:
* The latest build results
* Build number
* Build status
* Link to the latest build artifacts. The status widget doesn't require users log in to TeamCity.

When the feature is enabled, you need to include the following snippets of code in the web page source:

* Add this code sample in the `<head>` section (or alternatively, add the `withCss=true` parameter to _externalStatus.html_):
    
    ```Shell
    <style type="text/css">
    @import "<TeamCity_server_URL>/css/status/externalStatus.css";
    </style>
    ```
    
    
* Insert this code sample where you want to display the build configuration status:

    ```Shell
    <script type="text/javascript" src="<TeamCity_server_URL>/externalStatus.html?js=1">
    </script>
    
    ```

* If you prefer to use plain HTML instead of javascript, omit the `js=1` parameter and use `iframe` instead of the script:


    ```Shell
    <iframe src="<TeamCity_server_URL>/externalStatus.html"/>
    ```

* If you want to include default CSS styles without modifying the `<head>` section, add the `withCss=true` parameter.   
To provide up-to-date status information on specific build configurations, use the following parameter in the URL as many times as needed:

    
    ```Shell
    &buildTypeId=<external build configuration ID>
    
    ```

It is also possible to show the status of all projects build configurations by replacing `&[buildTypeId](identifier.md)=<external build configuration ID>` with `&[projectId](identifier.md)=<external project ID>`. You can select a combination of these parameters to display the needed projects and build configurations on your web page.

You can also download and customize the `externalStatus.css` file (for example, you can disable some columns by using `display: none`; see comments in `externalStatus.css`). However, in this case, you must _not_ include the __withCss=true__ parameter, but provide the CSS styles explicitly, preferably in the `<head>` section, instead.

<anchor name="ConfiguringGeneralSettings-runningBuildsLimit"/>
### Limit Number of Simultaneously Running Builds

You can limit the number of builds that can run simultaneously on all agents. 
This option helps improve allocating resources for your builds and avoid the situation 
when all agents are busy with the builds of a single project. 

You can limit the total number of builds for a build configuration and configure granular limits per branch using the related fields:
- for a build configuration, enter the total maximum number of builds. The number is set to 0 by default allowing an unlimited number of builds to run simultaneously.
- for a build limit per branch, enter the new-line separated list of rules. 
Each rule must follow the `branch:number` pattern, where `branch` is either a [logical branch name](working-with-feature-branches.md#Logical+Branch+Name) or a pattern containing an * 
and `number` specifies the maximum number of builds which can run simultaneously in each branch matching the pattern. 
0 allows an unlimited number of builds in the specified branch.



<seealso>
        <category ref="concepts">
            <a href="managing-builds.md">Build Configuration</a>
        </category>
</seealso>