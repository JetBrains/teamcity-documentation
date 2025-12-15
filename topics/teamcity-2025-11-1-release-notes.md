[//]: # (title: TeamCity 2025.11.1 Release Notes)
[//]: # (help-id: TeamCity 2025.11.1 Release Notes)

**Build 207998, 15 December 2025**

### Bug

* [**TW-96960**](https://youtrack.jetbrains.com/issue/TW-96960) — AI Assistant and buttons "Analyze it" are available for guest user (but they can't use them)
* [**TW-89324**](https://youtrack.jetbrains.com/issue/TW-89324) — KeepArtifactsCleanerCache consumes a lot of disk space
* [**TW-97621**](https://youtrack.jetbrains.com/issue/TW-97621) — Add information about cloud images without registered agents to the obsolete Java on agents health report
* [**TW-97379**](https://youtrack.jetbrains.com/issue/TW-97379) — java.lang.StackOverflowError on clean build
* [**TW-97757**](https://youtrack.jetbrains.com/issue/TW-97757) — Actualize supported IDE versions
* [**TW-96896**](https://youtrack.jetbrains.com/issue/TW-96896) — Build step with a publishArtifacts service message can finish before artifacts finish uploading
* [**TW-86480**](https://youtrack.jetbrains.com/issue/TW-86480) — Pull request build feature does not provide parameters for composite builds
* [**TW-97644**](https://youtrack.jetbrains.com/issue/TW-97644) — TeamCity builds the wrong branch after an empty merge commit.
* [**TW-97252**](https://youtrack.jetbrains.com/issue/TW-97252) — .NET runner: support for MSBuild Tools 2026
* [**TW-96017**](https://youtrack.jetbrains.com/issue/TW-96017) — Run in Docker Dockerfile command-line editor: caret jumps to end on edit
* [**TW-95439**](https://youtrack.jetbrains.com/issue/TW-95439) — Job-level artifacts: "Use shared files" option for dependency is enabled by default when the artifact is not shared
* [**TW-97012**](https://youtrack.jetbrains.com/issue/TW-97012) — New lines can be lost during streaming
* [**TW-97576**](https://youtrack.jetbrains.com/issue/TW-97576) — New build creation flow: wrong default checkout policy after creation
* [**TW-97518**](https://youtrack.jetbrains.com/issue/TW-97518) — Build Status Colors Not Accessible for Color-Blind Users
* [**TW-96298**](https://youtrack.jetbrains.com/issue/TW-96298) — Build overview: difference in the height of the failed tests
* [**TW-97487**](https://youtrack.jetbrains.com/issue/TW-97487) — Perfmon build feature is not added by default to build configurations created via the new flow
* [**TW-95045**](https://youtrack.jetbrains.com/issue/TW-95045) — Corrupted tokens stored in the database are not processed correctly
* [**TW-97430**](https://youtrack.jetbrains.com/issue/TW-97430) — Build using VCS root with a parameter reference which depends on the output parameter provided by its dependency cannot collect changes
* [**TW-96696**](https://youtrack.jetbrains.com/issue/TW-96696) — Enterprise Plus license: remove information that is inapplicable for Staging license
* [**TW-96729**](https://youtrack.jetbrains.com/issue/TW-96729) — Enterprise Plus license: Improve texts on staging server, if main True-up license is not activated
* [**TW-97497**](https://youtrack.jetbrains.com/issue/TW-97497) — Upgrade Agent Java remains inactive if agent is running under JVM with ARM architecture even if there is an ARM JDK available
* [**TW-95834**](https://youtrack.jetbrains.com/issue/TW-95834) — Changing a cloud agent image with "Terminate running instances" checkbox unticked terminates the instances anyway
* [**TW-96520**](https://youtrack.jetbrains.com/issue/TW-96520) — Unclear "Internal error" error message in the build log
* [**TW-95807**](https://youtrack.jetbrains.com/issue/TW-95807) — Error calling method CloudEventListener.instanceAgentMatched during the server restart
* [**TW-97529**](https://youtrack.jetbrains.com/issue/TW-97529) — New Project Creation: First DSL Import Attempt Fails, Subsequent Attempts Succeed
* [**TW-89993**](https://youtrack.jetbrains.com/issue/TW-89993) — BUILD_FINISHED webhook payload is missing TimeSpentInQueue statistic
* [**TW-97532**](https://youtrack.jetbrains.com/issue/TW-97532) — Invalid argument is provided for the idsGroupsRemoved multi node event
* [**TW-96466**](https://youtrack.jetbrains.com/issue/TW-96466) — Unexpected Error: Property dotCoverCommandLineKey not found on type on the Settings page
* [**TW-94359**](https://youtrack.jetbrains.com/issue/TW-94359) — dotCover: HideAutoProperties argument doesn't work
* [**TW-94903**](https://youtrack.jetbrains.com/issue/TW-94903) — Cancelling a build during a checkout may cause 'Cannot read file `C:\Users\builduser\.config\jgit\config` failures and corrupted Git mirrors
* [**TW-93643**](https://youtrack.jetbrains.com/issue/TW-93643) — Microsoft SQL Server Management Studio (SSMS) MSBuild is picked over Visual Studios Build Tools MSBuild
* [**TW-97590**](https://youtrack.jetbrains.com/issue/TW-97590) — Quick switch build configuration settings not working when pipelines are enabled
* [**TW-97749**](https://youtrack.jetbrains.com/issue/TW-97749) — Restore inline visibility of build comments without requiring an extra click

### Pipeline Enhancements

* [**TW-95961**](https://youtrack.jetbrains.com/issue/TW-95961) — TCP Run in Docker: Missing “no result” message in Docker Image autocomplete
* [**TW-96109**](https://youtrack.jetbrains.com/issue/TW-96109) — Trends view cannot be opened with error 404, if there are pipelines on the server
* [**TW-96939**](https://youtrack.jetbrains.com/issue/TW-96939) — Original imported transitive parameter is used in jobs for overriden parameters
* [**TW-97003**](https://youtrack.jetbrains.com/issue/TW-97003) — Pipeline that has failed to start cannot show an error
* [**TW-97124**](https://youtrack.jetbrains.com/issue/TW-97124) — New Pull Request doesn't trigger the run for a last PR (builds and pipelines)
* [**TW-96659**](https://youtrack.jetbrains.com/issue/TW-96659) — Autocomplete pop-up is not scrollable with more than N elements
* [**TW-97354**](https://youtrack.jetbrains.com/issue/TW-97354) — Health report about PR branches in the VCS trigger for pipelines, part 2
* [**TW-97026**](https://youtrack.jetbrains.com/issue/TW-97026) — Imported Parameters: Suggestions in steps show all parameters from parent, not the imported ones
* [**TW-97579**](https://youtrack.jetbrains.com/issue/TW-97579) — Latest Visual Studio build tools not available within new dotnet runners (VS2026)




### Performance Problem

* [**TW-97726**](https://youtrack.jetbrains.com/issue/TW-97726) — TeamCity 2025.11: High CPU usage from multiple Git CredentialsHelper processes
* [**TW-89253**](https://youtrack.jetbrains.com/issue/TW-89253) — Updating the login message can cause the UI to be unresponsive
* [**TW-97475**](https://youtrack.jetbrains.com/issue/TW-97475) — Checking for changes intermittently taking multiple hours to complete
* [**TW-97168**](https://youtrack.jetbrains.com/issue/TW-97168) — High CPU usage due to heavy computations inside PullRequestsBranchSpecsConflict health report


### Security

Two security problems have been fixed.
To learn more about fixed vulnerabilities directly related to TeamCity, check out our [Security Bulletin](https://www.jetbrains.com/privacy-security/issues-fixed/?product=TeamCity&version=2025.11.1).

Security bulletins are typically published few days after the release date.


