[//]: # (title: TeamCity 2019.2.4 Release Notes)
[//]: # (auxiliary-id: TeamCity 2019.2.4 Release Notes)


__Build: 72059__


### Feature

* [TW-59412](https://youtrack.jetbrains.com/issue/TW-59412) — Warn on changing trigger in the way which can cause triggering of many builds
* [TW-62075](https://youtrack.jetbrains.com/issue/TW-62075) — Add links to tests history on Compare Builds page
* [TW-65415](https://youtrack.jetbrains.com/issue/TW-65415) — Provide docker images for Windows 1903

### Bug

* [TW-65857](https://youtrack.jetbrains.com/issue/TW-65857) — "Unexpected error during Ajax request processing:" if \*\* or () was set in branch filter in schedule trigger
* [TW-62789](https://youtrack.jetbrains.com/issue/TW-62789) — "First failed in" shown in personal build when test actually succeeded
* [TW-65824](https://youtrack.jetbrains.com/issue/TW-65824) — Right warning about triggering of many builds is displayed only after saving schedule trigger (when magic wand for choosing branches was used)
* [TW-62420](https://youtrack.jetbrains.com/issue/TW-62420) — .NET runner ignore "/clp:NoSummary;NoItemAndPropertyList;ErrorsOnly" command-line parameter
* [TW-65695](https://youtrack.jetbrains.com/issue/TW-65695) — Disabling and then enabling of versioned settings can remove project configuration files from the repository (Subversion)
* [TW-65700](https://youtrack.jetbrains.com/issue/TW-65700) — Subversion commit support incorrectly handles commits where the same file is deleted and then added back
* [TW-65240](https://youtrack.jetbrains.com/issue/TW-65240) — Warning messages are not displayed in the build problem reason
* [TW-65151](https://youtrack.jetbrains.com/issue/TW-65151) — Uncaught TypeError on trying to edit Branches pattern in the Keep cleanup rules.
* [TW-62189](https://youtrack.jetbrains.com/issue/TW-62189) — Missing popup in coverage area on build page in Experimental UI.
* [TW-65784](https://youtrack.jetbrains.com/issue/TW-65784) — Broken link link in SBT runner
* [TW-65566](https://youtrack.jetbrains.com/issue/TW-65566) — Handshake error can be seen when trying to connect to some HTTPS URLs (caused by broken sunec.dll in bundled JRE)
* [TW-65662](https://youtrack.jetbrains.com/issue/TW-65662) — Report tab with # shows "404 - Artifact does not exist"
* [TW-65641](https://youtrack.jetbrains.com/issue/TW-65641) — Disable jgit internal auto git gc
* [TW-65702](https://youtrack.jetbrains.com/issue/TW-65702) — Cannot enable versioned settings in XML format in some project which has a sub project with own versioned settings configuration in the same repository
* [TW-60757](https://youtrack.jetbrains.com/issue/TW-60757) — Kotlin DSL: TeamCity fails to apply generated non-portable DSL settings (override existing settings in VCS case)
* [TW-65606](https://youtrack.jetbrains.com/issue/TW-65606) — Invalid warning message for clean checkout: Clean checkout due to: null
* [TW-64670](https://youtrack.jetbrains.com/issue/TW-64670) — Inconsistent behaviour in AWS S3 artifacts storage default AWS environment
* [TW-65581](https://youtrack.jetbrains.com/issue/TW-65581) — NuGet credentials do not work for .NET runner for external repos
* [TW-45233](https://youtrack.jetbrains.com/issue/TW-45233) — msbuild runner ignores /clp:PerformanceSummary parameter
* [TW-65596](https://youtrack.jetbrains.com/issue/TW-65596) — Maven Artifact Dependency trigger does not pass the environment from the project params to the trigger
* [TW-65557](https://youtrack.jetbrains.com/issue/TW-65557) — Build step script content can't be copied if a user hasn't edit rights for build type
* [TW-65612](https://youtrack.jetbrains.com/issue/TW-65612) — Cannot locate Maven: `teamcity.tool.maven.DEFAULT` doesn't exist or not a directory
* [TW-62871](https://youtrack.jetbrains.com/issue/TW-62871) — NPE in MavenEmbedderSession on a secondary node
* [TW-65529](https://youtrack.jetbrains.com/issue/TW-65529) — Invalid help link for Command in .NET runner
* [TW-65444](https://youtrack.jetbrains.com/issue/TW-65444) — Wrong directory is mention in logs for .NET Cleaner

### Performance Problem

* [TW-65649](https://youtrack.jetbrains.com/issue/TW-65649) — Slow changes collecting in case of broken and then fixed submodule
* [TW-65474](https://youtrack.jetbrains.com/issue/TW-65474) — Slower server startup because of constant interlocking between VcsModificationChecker and VcsModificationsStorageImpl

### Cosmetics

* [TW-65116](https://youtrack.jetbrains.com/issue/TW-65116) — Hovered item overlaps with hide button in the sidebar
* [TW-63128](https://youtrack.jetbrains.com/issue/TW-63128) — Branch in repositories section on changes tab doesn't aligned properly
* [TW-65725](https://youtrack.jetbrains.com/issue/TW-65725) — Remove extra space in the health report "SVN VCS root usage has too broad include rule".
