[//]: # (title: Configuring General Settings)
[//]: # (help-id: Configuring General Settings)

## General Build Configuration Settings

When creating a build configuration, specify the following settings:


<deflist type="full">

<def title="Name">

The build configuration name.

</def>

<def title="Build configuration ID" id="build-configuration-id" help-id="BuildconfigurationID">

A unique [ID](identifier.md) of the configuration across all build configurations and templates in the system automatically generated from the build configuration name, but can also be set manually.    
Make sure you give a globally unique id to the build configuration and prefix it with the project ID.   
After a build configuration is created, its ID can be changed, and it is highly recommended to make corresponding changes to the bookmarked links to the web UI and calls to [REST API](https://www.jetbrains.com/help/teamcity/rest/teamcity-rest-api-documentation.html) using the ID.

</def>

<def title="Description" help-id="build-config-description">

An optional description for the build configuration.

</def>


<def title="Build number format" help-id="ConfiguringGeneralSettings-BuildNumberFormat">

A pattern which is resolved and assigned to the [build number](#Build+Number+Format) on the build start.

</def>

<def title="Build counter" id="build-counter">

Specify the counter to be used in build numbering. Each build increases the build counter by 1. Use the _Reset_ link to restore the counter value to 1.

</def>

<def title="Publish artifacts" id="publish-artifacts">

Select when to publish artifacts:

* "_Even if build fails_" (default): publish artifacts at the last step of a build if all previous steps have been completed, successfully or not.
* "_Only if build status is successful_": publish artifacts at the last step of a build if all previous steps have been completed successfully. TeamCity checks the current build status on the server before publishing artifacts.
* "_Always, even if build stop command was issued_": publish artifacts for all builds, even for interrupted ones (for example, after the `stop` command was issued or after the time-out, specified in the build failure conditions).

This setting does not affect artifacts publishing configured in a [build script](service-messages.md#Publishing+Artifacts+While+Build+is+in+Progress).

<note>

If the `stop` command is issued during the artifacts publishing, the publishing operation will be stopped regardless of the selected option.

</note>

</def>

<def title="Artifact paths">

Patterns to define artifacts of a build. After the first build is run, you can browse the agent [checkout directory](build-checkout-directory.md) to configure artifacts paths.

See [](#Artifact+Paths) for more information.

</def>

<def title="Build options">

Additional [options](#Build+Options) that related to individual builds:

* [Hanging build detection](#Hanging+Build+Detection)
* [Allow triggering personal builds](#Allow+Triggering+Personal+Builds)
* [Enable status widget](#Enable+Status+Widget)
  * [HTML status widget](#HTML+Status+Widget)
* [Running builds limit](#Limit+Number+of+Simultaneously+Running+Builds)

</def>



</deflist>





### Build Number Format

In the _Build number format_ field you can specify a pattern which is resolved and assigned to the <tooltip term="build-number">_build number_</tooltip> on the build start.

<!--[//]: # (Internal note. Do not delete. "Configuring General Settingsd79e124.txt")-->

The following substitutions are supported in the pattern:

<deflist type="medium">

<def title="%\build.counter%">

The build counter unique for each build configuration. It is maintained by TeamCity and will resolve to a next integer value on each new build start. The current value of the counter can be edited in the _[Build counter](#build-counter)_ field.

</def>


<def title="%build.vcs.number.<VCS_root_name>%">

The revision used for the build of the VCS root with `<VCS_root_name>` name. [Read more](predefined-build-parameters.md) on the property.

</def>


<def title="%\property.name%">

A value of the build property with the corresponding name. All the [Predefined Build Parameters](predefined-build-parameters.md) are supported (including [Reference-only server properties](predefined-build-parameters.md#Predefined+Configuration+Parameters)).

</def>

</deflist>


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

  ```
  #teamcity:symbolicLinks=as-is
  %teamcity.build.checkoutDir%/build=>build.zip
  ```
  {ignore-vars="true"}

* Published archives include files and folders referenced by symlinks. To enable this behavior, decorate an artifact rule with the `teamcity:symbolicLinks` attribute as follows. 

  ```
  #teamcity:symbolicLinks=inline
  %teamcity.build.checkoutDir%/build=>build.zip
  ```
  {ignore-vars="true"}
  
Note that attributes affect only artifact publishing rules declared directly beneath them. For example, in the sample below only **Archive_A** will contain files and folders referenced by symlinks. **Archive_B** will employ the default behavior and include symlinks as files.

```
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

This option allows retrieving the status and basic details of the last build in the build configuration without any user authentication. It also allows getting the status of any specific build in the configuration, although builds cannot be listed and nothing beyond the build status (`success/failure/internal error/cancelled`) is exposed.

Once enabled, the status can be retrieved in one of two ways:

* As a single build status icon — via the [REST API](https://www.jetbrains.com/help/teamcity/rest/get-build-status-icon.html), or from the __Get build status icon...__ item in the __Actions__ menu on the [Build Configuration Home](build-configuration-home-page.md) page, which serves the same icon as ready-to-copy snippets.
* As the [HTML status widget](#HTML+Status+Widget) described below.

### HTML Status Widget

When the __[Enable status widget](#Enable+Status+Widget)__ option is turned on in a build configuration's settings, you can embed an HTML snippet into an external web page — a company website, wiki, Confluence page, or anywhere else — to display that configuration's current status. The widget shows the latest build result, its build number and status, and a link to the latest build artifacts. Viewing the status requires no TeamCity login.

<note>

If you only need a single status image rather than the full widget, use the [REST build status icon](https://www.jetbrains.com/help/teamcity/rest/get-build-status-icon.html) or the __Get build status icon...__ dialog described [above](#Enable+Status+Widget).

</note>

The widget is served from `<TeamCity_server_URL>/externalStatus.html`, and it only returns data for build configurations that have the __Enable status widget__ option turned on — so make sure it is enabled for every configuration you want to show. To choose what appears, add at least one of the following parameters to the URL (you can repeat and combine them):

* `buildTypeId=<external build configuration ID>` — a single build [configuration](identifier.md)
* `projectId=<external project ID>` — every build configuration of a [project](identifier.md)

Without any `buildTypeId` or `projectId` parameter, the request matches nothing and the widget renders `External status viewing is not enabled for the requested build configurations` instead of a status.

To use the widget, add this block to the `<head>` section of the page. It loads the widget's default styles from your TeamCity server, so they apply wherever the page is hosted:

```HTML
<style type="text/css">
@import "<TeamCity_server_URL>/css/status/externalStatus.css";
</style>
```

Then insert a script tag where the status should appear. For a single build configuration:

```HTML
<script type="text/javascript"
        src="<TeamCity_server_URL>/externalStatus.html?js=1&buildTypeId=<Build_Configuration_ID>">
</script>
```

For every exposed build configuration of a project:

```HTML
<script type="text/javascript"
        src="<TeamCity_server_URL>/externalStatus.html?js=1&projectId=<Project_Id>">
</script>
```

For a mix of projects and build configurations:

```HTML
<script type="text/javascript"
        src="<TeamCity_server_URL>/externalStatus.html?js=1&projectId=<Project_Id>&buildTypeId=<Build_Configuration_ID>">
</script>
```

If you prefer plain HTML to JavaScript, drop the `js=1` parameter and place the widget in an `<iframe>` instead of a `<script>` tag. Because the iframe content is served by TeamCity itself, append `withCss=true` to pull in the default styles — the `<head>` block above styles only the page it sits on, not the iframe:

```HTML
<iframe src="<TeamCity_server_URL>/externalStatus.html?withCss=true&buildTypeId=<external build configuration ID>"/>
```

The `withCss=true` parameter only works inside the iframe, where the styles are loaded from the same TeamCity server that serves the widget. For the JavaScript snippet, keep the `<head>` block instead: its `@import` points at the TeamCity server directly, so it works from any page.

To customize the widget's appearance, download `externalStatus.css`, edit it (for example, hide columns with `display: none`; see the comments in the file), and host your own copy. In that case, reference your stylesheet from the `<head>` section and do not add `withCss=true`.


### Limit Number of Simultaneously Running Builds
{help-id="ConfiguringGeneralSettings-runningBuildsLimit"}

The **Maximum concurrent builds for this build configuration** setting defines how many builds of this configuration can run at the same time. It helps prevent a single configuration from occupying all available agents. You can specify either:

* a numeric value, where `0` means unlimited
* newline-separated `branch:number` rules to set different limits for different branches

The `branch` value can be either a [logical branch name](working-with-feature-branches.md#Logical+Branch+Name) or a pattern that uses an asterisk (`*`) as a wildcard.

```Text
<default>:0 # no limit for the builds running on default branch
pull/*:1    # no more than one build for each pull request
```

The **If the limit is reached** option controls what happens when the maximum number of concurrent builds is already running. TeamCity can either keep new builds in the queue until a running build finishes, or cancel the oldest running build to make room for a new one.


<seealso>
        <category ref="concepts">
            <a href="managing-builds.md">Build Configuration</a>
        </category>
</seealso>
