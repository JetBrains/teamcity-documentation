[//]: # (title: TeamCity 2026.1 Release Notes)
[//]: # (help-id: TeamCity 2026.1 Release Notes)

**Build 012345, 11 May 2026**

### Feature

* [**TW-92582**](https://youtrack.jetbrains.com/issue/TW-92582) — Multi-repo support for pipelines using VCS roots from parent project
* [**TW-92756**](https://youtrack.jetbrains.com/issue/TW-92756) — Support of MCP protocol to enable 3rd party integrations
* [**TW-82657**](https://youtrack.jetbrains.com/issue/TW-82657) — GitHub App connection: provide dynamic credentials to build steps.
* [**TW-96595**](https://youtrack.jetbrains.com/issue/TW-96595) — Allow to configure system webhooks for self-managed GitLab 
* [**TW-42994**](https://youtrack.jetbrains.com/issue/TW-42994) — It should be possible to resolve value of reverse.dep.*** parameter in context of the head build
* [**TW-99633**](https://youtrack.jetbrains.com/issue/TW-99633) — Add preset to create read-only token
* [**TW-87288**](https://youtrack.jetbrains.com/issue/TW-87288) — Make the 'Build Name' value in the Parameters description of the Commit Status Publisher configurable
* [**TW-77495**](https://youtrack.jetbrains.com/issue/TW-77495) — Perforce Shelve Trigger / Personal Build should run "p4 resolve"
* [**TW-79126**](https://youtrack.jetbrains.com/issue/TW-79126) — TeamCity Hashicorp Vault Google Cloud auth method
* [**TW-77514**](https://youtrack.jetbrains.com/issue/TW-77514) — support unshelving multiple perforce shelves with rest API
* [**TW-34709**](https://youtrack.jetbrains.com/issue/TW-34709) — Support for SAML Authentication
* [**TW-1858**](https://youtrack.jetbrains.com/issue/TW-1858) — Cancel an obsolete running build if there are more recent changes/queued builds
* [**TW-43295**](https://youtrack.jetbrains.com/issue/TW-43295) — Ability to pause build queue via REST api call
* [**TW-97730**](https://youtrack.jetbrains.com/issue/TW-97730) — Implement "same-chain" artifact dependency option

### Task

* [**TW-93722**](https://youtrack.jetbrains.com/issue/TW-93722) — Discontinue support for Java versions earlier than 21 on both the server and agent
* [**TW-99770**](https://youtrack.jetbrains.com/issue/TW-99770) — Provide better instructions for MCP for finding build configurations/pipelines related to repository
* [**TW-93287**](https://youtrack.jetbrains.com/issue/TW-93287) — SLNX Solution support
* [**TW-65203**](https://youtrack.jetbrains.com/issue/TW-65203) — Enable virtual host addressing in S3 by default
* [**TW-93979**](https://youtrack.jetbrains.com/issue/TW-93979) — Migrate bundled AWS-related plugins to SDK v2 
* [**TW-86630**](https://youtrack.jetbrains.com/issue/TW-86630) — Log the Token Name of the User Access Token when used in requests
* [**TW-96506**](https://youtrack.jetbrains.com/issue/TW-96506) — Update bundled version of the dotCover Command Line Tools to 2025.1.7 version
* [**TW-96509**](https://youtrack.jetbrains.com/issue/TW-96509) — Update ReSharper Command Line Tool to 2025.2.3 version
* [**TW-99771**](https://youtrack.jetbrains.com/issue/TW-99771) — Provide better instructions for MCP for working with pipelines
* [**TW-97421**](https://youtrack.jetbrains.com/issue/TW-97421) — Support AI Assistant in trial Enterprise license
* [**TW-98507**](https://youtrack.jetbrains.com/issue/TW-98507) — Gradle integration: use the --version command to detect the Gradle version
* [**TW-77925**](https://youtrack.jetbrains.com/issue/TW-77925) — Deprecate Java < 21 for starting a server
* [**TW-95544**](https://youtrack.jetbrains.com/issue/TW-95544) — Merge runners: group dotnet commands in UI
* [**TW-98704**](https://youtrack.jetbrains.com/issue/TW-98704) — Gradle integration: get rid of the GradleConnector usage
* [**TW-98869**](https://youtrack.jetbrains.com/issue/TW-98869) — Gradle integration: use MultiCommandBuildSessionFactory to allow execution of multiple sequential commands in a container
* [**TW-95056**](https://youtrack.jetbrains.com/issue/TW-95056) — Add a max limit to the disk size of a build configuration parameter
* [**TW-79999**](https://youtrack.jetbrains.com/issue/TW-79999) — SakuraUI: Impossible to send a link to the failed test
* [**TW-94330**](https://youtrack.jetbrains.com/issue/TW-94330) — Add commit date in addition to authored date to Change entity in rest
* [**TW-98203**](https://youtrack.jetbrains.com/issue/TW-98203) — Add progress to copy/delete operations in artifacts-migration-tool
* [**TW-96441**](https://youtrack.jetbrains.com/issue/TW-96441) — Add retries for PowerShell detection command
* [**TW-97738**](https://youtrack.jetbrains.com/issue/TW-97738) — Comments to builds are hardly searchable
* [**TW-96827**](https://youtrack.jetbrains.com/issue/TW-96827) — Implement batching for the ignored tests
* [**TW-97332**](https://youtrack.jetbrains.com/issue/TW-97332) — Update default JaCoCo tool version
* [**TW-97955**](https://youtrack.jetbrains.com/issue/TW-97955) — Add the ability to copy code snippets in the AI Assistant
* [**TW-96267**](https://youtrack.jetbrains.com/issue/TW-96267) — Make TeamCity agent scripts compatible with Java 24 & 25
* [**TW-97446**](https://youtrack.jetbrains.com/issue/TW-97446) — Switch to git log command for the build revisions calculation
* [**TW-96384**](https://youtrack.jetbrains.com/issue/TW-96384) — Support configurable Git shallow clone depth (e.g. --depth=2)
* [**TW-96513**](https://youtrack.jetbrains.com/issue/TW-96513) — Update Maven tool to 3.9.11 version


### Pipeline Enhancements

* [**TW-98715**](https://youtrack.jetbrains.com/issue/TW-98715) — Add Swabra, free disk space, build cache, xml report processing features to pipeline jobs
* [**TW-96208**](https://youtrack.jetbrains.com/issue/TW-96208) — Pipeline overview
* [**TW-99230**](https://youtrack.jetbrains.com/issue/TW-99230) — Support VCS hosting build features configuration in Pipelines
* [**TW-95286**](https://youtrack.jetbrains.com/issue/TW-95286) — Snapshot dependencies for Pipelines
* [**TW-94763**](https://youtrack.jetbrains.com/issue/TW-94763) — Support Pipelines as Kotlin object in DSL
* [**TW-100134**](https://youtrack.jetbrains.com/issue/TW-100134) — Run custom build for pipelines
* [**TW-99308**](https://youtrack.jetbrains.com/issue/TW-99308) — Pipeline fails to start with "Cannot locate Maven" if bundled maven version was chosen in pipeline (version that exists on server)
* [**TW-97361**](https://youtrack.jetbrains.com/issue/TW-97361) — It's unclear from the UI and docs, where job artifacts will be published
* [**TW-94020**](https://youtrack.jetbrains.com/issue/TW-94020) — Cloud images are not listed in incompatible agents when the agent is starting
* [**TW-98149**](https://youtrack.jetbrains.com/issue/TW-98149) — Running a personal pipeline with a broken yaml results in 500 error and a broken pipeline run page
* [**TW-97481**](https://youtrack.jetbrains.com/issue/TW-97481) — The tooltip on the 'Save' button is hard to read (Pipeline Editing page)
* [**TW-98480**](https://youtrack.jetbrains.com/issue/TW-98480) — Pipeline build runners auto-generate YAML with quotes in boolean values
* [**TW-97647**](https://youtrack.jetbrains.com/issue/TW-97647) — The popup with actions is not shown for pipeline dependencies
* [**TW-97010**](https://youtrack.jetbrains.com/issue/TW-97010) — Show UI error when trying to override imported parameter on the pipeline level
* [**TW-96907**](https://youtrack.jetbrains.com/issue/TW-96907) — Import Parameters: Expand name and value text fields in case of multiline input
* [**TW-96436**](https://youtrack.jetbrains.com/issue/TW-96436) — No confirmation for the 'Cancel run' action
* [**TW-96280**](https://youtrack.jetbrains.com/issue/TW-96280) — Show in build log opens Problem tab instead
* [**TW-96162**](https://youtrack.jetbrains.com/issue/TW-96162) — Misplaced VCS root name on Edit repository page for existing VCS root
* [**TW-95598**](https://youtrack.jetbrains.com/issue/TW-95598) — Order of pipelines steps resets in Editing Job page in UI after saving the changes (the pipeline runs steps in the changed order)
* [**TW-95437**](https://youtrack.jetbrains.com/issue/TW-95437) — Modification of a job output breaks the pipeline with dependencies
* [**TW-95358**](https://youtrack.jetbrains.com/issue/TW-95358) — Text area for multi-line script step is too narrow
* [**TW-99105**](https://youtrack.jetbrains.com/issue/TW-99105) — Pipeline cancelation reason is not shown in Pipeline run status badge
* [**TW-97524**](https://youtrack.jetbrains.com/issue/TW-97524) — Muting test in the selected pipeline doesn't work
* [**TW-97389**](https://youtrack.jetbrains.com/issue/TW-97389) — No errors about incompatible agents in pipeline settings, if parameter references can't be resolved on the Pipeline level
* [**TW-98826**](https://youtrack.jetbrains.com/issue/TW-98826) — Problems tree on should correctly display problems from pipeline run
* [**TW-99146**](https://youtrack.jetbrains.com/issue/TW-99146) — Pipeline editing is broken with no clear reason if there are problems with inherited connections
* [**TW-99104**](https://youtrack.jetbrains.com/issue/TW-99104) — Pipeline status badge shows "Failed" status and icon instead of "Failed to start"
* [**TW-99096**](https://youtrack.jetbrains.com/issue/TW-99096) — "Changes since last successful build" is available in pipelines but returns error 400
* [**TW-98935**](https://youtrack.jetbrains.com/issue/TW-98935) — Edit pipeline indicator is hidden behind the pipeline name
* [**TW-99442**](https://youtrack.jetbrains.com/issue/TW-99442) — Pipeline Kotlin DSL if not generated if Commit Status Publisher or Pull Requests are enabled
* [**TW-99466**](https://youtrack.jetbrains.com/issue/TW-99466) — "Provide missing secure values" from pipeline-related server health errors should lead to parent project "Tokens"
* [**TW-99249**](https://youtrack.jetbrains.com/issue/TW-99249) — The 'Agent Terminal' button is not displayed on the pipeline run page when parallel tests are used
* [**TW-99489**](https://youtrack.jetbrains.com/issue/TW-99489) — It's possible to add the same snapshot dependency twice in Pipeline YAML
* [**TW-99383**](https://youtrack.jetbrains.com/issue/TW-99383) — Steps are not disabled in Pipeline Jobs
* [**TW-98922**](https://youtrack.jetbrains.com/issue/TW-98922) — Click at banner closes the settings panel in Pipelines
* [**TW-94636**](https://youtrack.jetbrains.com/issue/TW-94636) — TCP Merge: YAML/Visual view is not preserved between pipelines
* [**TW-98827**](https://youtrack.jetbrains.com/issue/TW-98827) — Tests tree on should correctly display problems from pipeline run
* [**TW-98974**](https://youtrack.jetbrains.com/issue/TW-98974) — Update the colour of the selected pipeline error state
* [**TW-97704**](https://youtrack.jetbrains.com/issue/TW-97704) — Jump to the pipeline name in the create pipeline form
* [**TW-98864**](https://youtrack.jetbrains.com/issue/TW-98864) — Misaligned parameter names in Pipeline settings in Safari
* [**TW-97394**](https://youtrack.jetbrains.com/issue/TW-97394) — "Copy answer" button is hidden if CSAT is disabled
* [**TW-98769**](https://youtrack.jetbrains.com/issue/TW-98769) — Display dependencies for pipeline runs in the build details
* [**TW-97367**](https://youtrack.jetbrains.com/issue/TW-97367) — Pipelines use HTML URL for creating new VCS roots from connections
* [**TW-97281**](https://youtrack.jetbrains.com/issue/TW-97281) — Too strict DSL validation checks for Pipeline Job steps
* [**TW-97424**](https://youtrack.jetbrains.com/issue/TW-97424) — 'VCS root not accessible from configuration' error message should be adjusted to take pipelines into account
* [**TW-97425**](https://youtrack.jetbrains.com/issue/TW-97425) — 'Failed to resolve artifacts from ...' error message should be adjusted to take pipelines into account
* [**TW-97292**](https://youtrack.jetbrains.com/issue/TW-97292) — Collapsed "Outputs" sections for Pipeline jobs shows "Shared files: object Object"
* [**TW-98237**](https://youtrack.jetbrains.com/issue/TW-98237) — Disabled and disconnected agents are shown as compatible for queued Pipeline Jobs
* [**TW-95622**](https://youtrack.jetbrains.com/issue/TW-95622) — Better handling of cases of incorrect pipelines URL openings
* [**TW-97146**](https://youtrack.jetbrains.com/issue/TW-97146) — Show the dotnet devenv action in the pipeline Editor
* [**TW-96674**](https://youtrack.jetbrains.com/issue/TW-96674) — Pipelines: Docker/NPM Integration allows saving with empty URL, causing error during job execution

### Bug

* [**TW-80432**](https://youtrack.jetbrains.com/issue/TW-80432) — Kubernetes Plugin does not allow to configure proxy to cluster
* [**TW-85749**](https://youtrack.jetbrains.com/issue/TW-85749) — Personal patch is not applied for Parallel Tests and Matrix builds virtual dependencies
* [**TW-100717**](https://youtrack.jetbrains.com/issue/TW-100717) — Add a link to the update doc for "agent can't be upgraded" warning
* [**TW-99775**](https://youtrack.jetbrains.com/issue/TW-99775) — Adding build type to favorites fails
* [**TW-100013**](https://youtrack.jetbrains.com/issue/TW-100013) — 'Cannot find a node' error in build VCS changes calculation
* [**TW-95112**](https://youtrack.jetbrains.com/issue/TW-95112) — Rename Docker Repository -> Docker Registry
* [**TW-99275**](https://youtrack.jetbrains.com/issue/TW-99275) — Parameter name disappears when configuring custom agent requirement in Firefox
* [**TW-99022**](https://youtrack.jetbrains.com/issue/TW-99022) — The bars on the optimization chart are stuck to the top edge
* [**TW-94106**](https://youtrack.jetbrains.com/issue/TW-94106) — "Save on TeamCity server" button has no feedback
* [**TW-97451**](https://youtrack.jetbrains.com/issue/TW-97451) — Change of the value type key case can lead to missing statistic values in the database
* [**TW-95293**](https://youtrack.jetbrains.com/issue/TW-95293) — Dependency build fails to load build settings when the dependency relationship is defined only in a DSL feature branch
* [**TW-97326**](https://youtrack.jetbrains.com/issue/TW-97326) — Perforce virtual streams: only latest change is detected from two separate submissions
* [**TW-96950**](https://youtrack.jetbrains.com/issue/TW-96950) — VCS root uses SSH keys specified in .ssh/config on the server machine, if "Uploaded key" is selected, but the key is not valid
* [**TW-98038**](https://youtrack.jetbrains.com/issue/TW-98038) — Inherited S3 storage is inaccessible in subprojects when “Limit build access permissions” is enabled and AWS connection uses IAM Role
* [**TW-96881**](https://youtrack.jetbrains.com/issue/TW-96881) — Last run deletion leads to empty table with non-functioning links
* [**TW-77853**](https://youtrack.jetbrains.com/issue/TW-77853) — Versioned Settings Change Log tab is empty for a user with "View build configuration settings" and without "Edit project" permission
* [**TW-97745**](https://youtrack.jetbrains.com/issue/TW-97745) — Old artifacts Cleaner cache files remain in data directory, if keepArtifactsNewCache is enabled
* [**TW-97791**](https://youtrack.jetbrains.com/issue/TW-97791) — Do not show parameter as unresolved when it's used only in step which requires existence of that parameter
* [**TW-98071**](https://youtrack.jetbrains.com/issue/TW-98071) — Patch for select is added when the parameter is edited via UI
* [**TW-96738**](https://youtrack.jetbrains.com/issue/TW-96738) — AWS Connection: old access key is not removed from AWS when using the Rotate Keys button. 
* [**TW-95821**](https://youtrack.jetbrains.com/issue/TW-95821) — OAuth authentication doesn't work with default port of Idea Built-in server
* [**TW-96429**](https://youtrack.jetbrains.com/issue/TW-96429) — Optimize PowerShell detection on Windows by avoiding launching the PowerShell executable when possible
* [**TW-96434**](https://youtrack.jetbrains.com/issue/TW-96434) — Fix PowerShell detection on Windows for PowerShell Core edition
* [**TW-97531**](https://youtrack.jetbrains.com/issue/TW-97531) — Cloud instances synchronization thread fails to save some of the cloud instances to the database
* [**TW-100496**](https://youtrack.jetbrains.com/issue/TW-100496) — Incorrect generated Pre-signed S3 URLs for HEAD method
* [**TW-82851**](https://youtrack.jetbrains.com/issue/TW-82851) — Error: No enum constant javax.lang.model.element.Modifier.SEALED while compiling a build
* [**TW-77295**](https://youtrack.jetbrains.com/issue/TW-77295) — IntelliJ IDEA projects runner doesn't work if run under java 16 and later: No signature of method: org.codehaus.groovy.tools.RootLoader.getPackage() is applicable for argument types: (java.lang.String)
* [**TW-99919**](https://youtrack.jetbrains.com/issue/TW-99919) — Azure Pat connection: TFS root isn't recognized during build configuration creation
* [**TW-94700**](https://youtrack.jetbrains.com/issue/TW-94700) — Improve versioned settings errors health report
* [**TW-100276**](https://youtrack.jetbrains.com/issue/TW-100276) — Overridden dependency parameter has original value, if all same-level dependent builds except one were skipped
* [**TW-94482**](https://youtrack.jetbrains.com/issue/TW-94482) — "Could not start new instances. Quota exceeded" warning message on cloud image - K8S
* [**TW-87567**](https://youtrack.jetbrains.com/issue/TW-87567) — AWS Connections with IAM Roles can't access connections from parent projects
* [**TW-99684**](https://youtrack.jetbrains.com/issue/TW-99684) — It's unclear how to overcome error about inaccessible dependencies in Build configurations
* [**TW-99407**](https://youtrack.jetbrains.com/issue/TW-99407) — Exception on attempt to finish a build if there are many unresolved output parameters
* [**TW-100380**](https://youtrack.jetbrains.com/issue/TW-100380) — Test status is shown incorrectly if test runs are sorted by status on the test history page
* [**TW-100102**](https://youtrack.jetbrains.com/issue/TW-100102) — Build fails with "The build was triggered in the branch ... which does not correspond to any branch monitored by the build VCS roots..." erroneously if it has VCS root attached which is also a settings VCS root in some other project
* [**TW-92128**](https://youtrack.jetbrains.com/issue/TW-92128) — Simplify locating thread dumps reported on execution timeout
* [**TW-99243**](https://youtrack.jetbrains.com/issue/TW-99243) — A failing bootstrap step does not prevent subsequent steps from running
* [**TW-99821**](https://youtrack.jetbrains.com/issue/TW-99821) — p4 unshelve: running p4 clean may remove undesired files
* [**TW-98160**](https://youtrack.jetbrains.com/issue/TW-98160) — K8S executor: executor requires a template image to have java installed in /opt/java
* [**TW-96398**](https://youtrack.jetbrains.com/issue/TW-96398) — Swabra build feature creates checkout directory snapshot before sources checkout when used with a Bootstrap build step
* [**TW-97967**](https://youtrack.jetbrains.com/issue/TW-97967) — K8s executor: requirements via env. variables are incorrectly resolved
* [**TW-99639**](https://youtrack.jetbrains.com/issue/TW-99639) — Update AI disable feedback
* [**TW-99267**](https://youtrack.jetbrains.com/issue/TW-99267) — Incorrect parameter resolution for a not yet started build
* [**TW-97570**](https://youtrack.jetbrains.com/issue/TW-97570) — AI Assistant in TeamCity Cloud can suggest on-prem or incorrect description if feature is setup differently in Cloud (comparing to on-prem)
* [**TW-97566**](https://youtrack.jetbrains.com/issue/TW-97566) — AI Assistant in TeamCity Cloud can suggest on-prem description if feature doesn't exist in Cloud
* [**TW-93185**](https://youtrack.jetbrains.com/issue/TW-93185) — Could not create the main application servlet: ReadOnlyException: Writing to file "/storage/system/pluginData/cloudProfileIdx" is prohibited by TeamCity node restrictions
* [**TW-99137**](https://youtrack.jetbrains.com/issue/TW-99137) — It's unclear why server was not started, if teamcity-server.sh couldn't find minimal Java
* [**TW-99604**](https://youtrack.jetbrains.com/issue/TW-99604) — Sakura Agent Page: Miscellaneous section is missing
* [**TW-95473**](https://youtrack.jetbrains.com/issue/TW-95473) — Unrelated projects in the projects popup of the test history page
* [**TW-19752**](https://youtrack.jetbrains.com/issue/TW-19752) — VCS root 'Test connection' only shows one failed mapping at a time in Perforce
* [**TW-99922**](https://youtrack.jetbrains.com/issue/TW-99922) — Azure TFS VCS Root can't connect to repository
* [**TW-100061**](https://youtrack.jetbrains.com/issue/TW-100061) — Windows Docker images: the dotCover { … } step fails due to non-canonical ACLs
* [**TW-82689**](https://youtrack.jetbrains.com/issue/TW-82689) — Personal Access Tokens for Jira Server in Jira Integration are not supported
* [**TW-99008**](https://youtrack.jetbrains.com/issue/TW-99008) — Secondary node REST API returns finished build with status FAILURE but empty build problems
* [**TW-97024**](https://youtrack.jetbrains.com/issue/TW-97024) — Automatically interrupt long running HTTP requests
* [**TW-99978**](https://youtrack.jetbrains.com/issue/TW-99978) — Wrong JDK version for agentInstaller.exe on the Agent distributions page
* [**TW-67712**](https://youtrack.jetbrains.com/issue/TW-67712) — Changing AMI in Amazon Cloud Profile terminates the instances running from old AMI even if they are running the builds
* [**TW-98671**](https://youtrack.jetbrains.com/issue/TW-98671) — Commit status publisher Build Feature description doesn't show the correct and sufficient information 
* [**TW-93456**](https://youtrack.jetbrains.com/issue/TW-93456) — Matrix builds: Reverse dependencies not resolved in matrix builds
* [**TW-99475**](https://youtrack.jetbrains.com/issue/TW-99475) — Update Java 21.0.6 -> 21.0.10 in Windows installer and Docker images
* [**TW-99640**](https://youtrack.jetbrains.com/issue/TW-99640) — Feedback form is not shown after admin chose the "Keep AI Assistant disabled"
* [**TW-95465**](https://youtrack.jetbrains.com/issue/TW-95465) — Incorrect link in snapshot dependency in Sakura UI
* [**TW-96694**](https://youtrack.jetbrains.com/issue/TW-96694) — Pause Build Queue/Resume Build Queue is missing from classic UI
* [**TW-99757**](https://youtrack.jetbrains.com/issue/TW-99757) — Remove the vertical scrollbar
* [**TW-93396**](https://youtrack.jetbrains.com/issue/TW-93396) — K8S Cloud Profile, Windows Pod Template can break
* [**TW-98965**](https://youtrack.jetbrains.com/issue/TW-98965) — Passwords can be shown in plain text in the thread names if thread name captures the post request parameters
* [**TW-99478**](https://youtrack.jetbrains.com/issue/TW-99478) — Constant warnings "Failed to find telegraf executable in PATH" in teamcity-agent.log
* [**TW-95283**](https://youtrack.jetbrains.com/issue/TW-95283) — Perforce: `forceTrust` option from build parameters is ignored in some cases
* [**TW-97821**](https://youtrack.jetbrains.com/issue/TW-97821) — S3 retry mechanism for error code 0: check all possible scenarios are covered
* [**TW-96602**](https://youtrack.jetbrains.com/issue/TW-96602) — "Stop build" button partially obscured in classic UI
* [**TW-99349**](https://youtrack.jetbrains.com/issue/TW-99349) — Remove of rephrase health report about JAVA < 21 on agents
* [**TW-98442**](https://youtrack.jetbrains.com/issue/TW-98442) — Prompt password parameter is provided in encrypted form if a custom value is specified
* [**TW-99379**](https://youtrack.jetbrains.com/issue/TW-99379) — Kotlin DSL generates "disabled" raw parameter for Job Steps
* [**TW-95561**](https://youtrack.jetbrains.com/issue/TW-95561) — TCP Steps: "(and 0 more line)" can be shown in the step settings if all the script is visible
* [**TW-99128**](https://youtrack.jetbrains.com/issue/TW-99128) — Virtual configuration can inherit triggers from the default template
* [**TW-97951**](https://youtrack.jetbrains.com/issue/TW-97951) — Review the empty states of the main navigation
* [**TW-98197**](https://youtrack.jetbrains.com/issue/TW-98197) — Don’t show CSAT after major update for users who rarely visit the server
* [**TW-99203**](https://youtrack.jetbrains.com/issue/TW-99203) — Free disk space UI: prefill the default 3gb value for the feature
* [**TW-91601**](https://youtrack.jetbrains.com/issue/TW-91601) — Node.js Runner: Build Step Auto-detection suggest a deprecated package for eslint which ends up as a build problem 
* [**TW-98865**](https://youtrack.jetbrains.com/issue/TW-98865) — Remove "Copy link to test" from dropdown for HTTP servers
* [**TW-52584**](https://youtrack.jetbrains.com/issue/TW-52584) — Order of SBuild.getFailureReasons() (add timestamp to build problem to allow their sorting)
* [**TW-99023**](https://youtrack.jetbrains.com/issue/TW-99023) — The Log In button is stuck to the left edge
* [**TW-98832**](https://youtrack.jetbrains.com/issue/TW-98832) — Fix design review comments
* [**TW-93063**](https://youtrack.jetbrains.com/issue/TW-93063) — Versioned Settings ignore "custom path" when TeamCity automatically adds ID to requirements set without ID in XML-files.
* [**TW-95431**](https://youtrack.jetbrains.com/issue/TW-95431) — Job-level artifacts: impossible to download via the button
* [**TW-99075**](https://youtrack.jetbrains.com/issue/TW-99075) — NullPointerException inside assignRolesDialogContent_jsp
* [**TW-99021**](https://youtrack.jetbrains.com/issue/TW-99021) — Long branch names extend beyond the block
* [**TW-98176**](https://youtrack.jetbrains.com/issue/TW-98176) — Delete the hardcode of specific version of Node in Node.js runner
* [**TW-98967**](https://youtrack.jetbrains.com/issue/TW-98967) — Dependency preview state remains when user navigates away
* [**TW-97351**](https://youtrack.jetbrains.com/issue/TW-97351) — FUS events can be lost if users click too fast
* [**TW-96093**](https://youtrack.jetbrains.com/issue/TW-96093) — Benchmark plugin: the benchmark is stuck because the connectivity between created agents and the server cannot be verified
* [**TW-98092**](https://youtrack.jetbrains.com/issue/TW-98092) — "Repository not found" errors during token refresh
* [**TW-95159**](https://youtrack.jetbrains.com/issue/TW-95159) — Slack notifier should raise a system problem if it failed to send a message
* [**TW-98101**](https://youtrack.jetbrains.com/issue/TW-98101) — Show the reaction on copying the code snippets from AI Assistant
* [**TW-93646**](https://youtrack.jetbrains.com/issue/TW-93646) — "Invalid target pool for image" must not fail whole cloud profile
* [**TW-98746**](https://youtrack.jetbrains.com/issue/TW-98746) — Create the Kotlin DSL setting for the 'Build Name' value in the Parameters description of the Commit Status Publisher
* [**TW-97668**](https://youtrack.jetbrains.com/issue/TW-97668) — Some Launch Templates are missing from the Cloud Image settings
* [**TW-91965**](https://youtrack.jetbrains.com/issue/TW-91965) — Reintroduce the Project Pool option on EC2 UI
* [**TW-97753**](https://youtrack.jetbrains.com/issue/TW-97753) — Project Change Log shows only 2 pages in Sakura
* [**TW-95537**](https://youtrack.jetbrains.com/issue/TW-95537) — Broken documentation link for GitHub Enterprise API in GitHub Release build step and commit status publisher settings
* [**TW-98123**](https://youtrack.jetbrains.com/issue/TW-98123) — Agent temp directories may not be correctly set when running an agent on Java 21
* [**TW-98622**](https://youtrack.jetbrains.com/issue/TW-98622) — Compare Builds shows the same test in different rows
* [**TW-94858**](https://youtrack.jetbrains.com/issue/TW-94858) — Kubernetes Cloud Profile image settings are sometimes not available in the web UI
* [**TW-89324**](https://youtrack.jetbrains.com/issue/TW-89324) — KeepArtifactsCleanerCache consumes a lot of disk space
* [**TW-97344**](https://youtrack.jetbrains.com/issue/TW-97344) — Kubernetes Cloud images: Cannot view image settings for read-only projects
* [**TW-97337**](https://youtrack.jetbrains.com/issue/TW-97337) — Delete image dialog is empty in K8s cloud profile
* [**TW-97343**](https://youtrack.jetbrains.com/issue/TW-97343) — Kubernetes cloud images: Do not show "Delete image" link for read-only projects
* [**TW-97630**](https://youtrack.jetbrains.com/issue/TW-97630) — S3 400 Status with connection reset
* [**TW-98113**](https://youtrack.jetbrains.com/issue/TW-98113) — Incorrect link to the queued build page
* [**TW-97729**](https://youtrack.jetbrains.com/issue/TW-97729) — Builds suitable for reuse are only checked when a shared resource is available
* [**TW-97259**](https://youtrack.jetbrains.com/issue/TW-97259) — A lot of "Error occurred while parsing frontend analytics events" warning on secondary node logs
* [**TW-98251**](https://youtrack.jetbrains.com/issue/TW-98251) — Don't show the re-encryption health report for after the update
* [**TW-97318**](https://youtrack.jetbrains.com/issue/TW-97318) — Cryptic error at attempt to create project/configuration from git URL with wrong token
* [**TW-98252**](https://youtrack.jetbrains.com/issue/TW-98252) — Allow to run re-encryption with the same key, if previous reencryption is marked as successful
* [**TW-97854**](https://youtrack.jetbrains.com/issue/TW-97854) — Sporadic build-scoped tokens related message in logs 
* [**TW-96844**](https://youtrack.jetbrains.com/issue/TW-96844) — Read-only secondary node cannot generate new encryption key
* [**TW-78885**](https://youtrack.jetbrains.com/issue/TW-78885) — "Expand all" tests button on Build Overview does not expand them all
* [**TW-97731**](https://youtrack.jetbrains.com/issue/TW-97731) — Acquire VCS auth token: Unclear error 'Parameter "state" is missing', if browser URL doesn't match server URL
* [**TW-97874**](https://youtrack.jetbrains.com/issue/TW-97874) — Clicking the link to issues in changes popup does nothing
* [**TW-94894**](https://youtrack.jetbrains.com/issue/TW-94894) — The executor in the StatisticsPublisher class doesn't have a queue limit
* [**TW-98055**](https://youtrack.jetbrains.com/issue/TW-98055) — Indefinitely running build after Normal Executor RejectedExecutionException
* [**TW-97896**](https://youtrack.jetbrains.com/issue/TW-97896) — Use git fetch with --no-show-forced-updates option
* [**TW-94397**](https://youtrack.jetbrains.com/issue/TW-94397) — Jira Integration throws No Content to Map Exception
* [**TW-98048**](https://youtrack.jetbrains.com/issue/TW-98048) — Fix width for "New Project" button in Firefox
* [**TW-98073**](https://youtrack.jetbrains.com/issue/TW-98073) — Broken TestName comparator
* [**TW-97739**](https://youtrack.jetbrains.com/issue/TW-97739) — Jira Cloud status publishing logs messages into the teamcity-server.log
* [**TW-97345**](https://youtrack.jetbrains.com/issue/TW-97345) — New build creation flow: Create duplicate VCS root from other Repository Url lands the user on the connection creation page
* [**TW-94572**](https://youtrack.jetbrains.com/issue/TW-94572) — Error "Failed to read "usage-statistics" plugin state" in teamcity-server.log after update to 2025.07
* [**TW-97094**](https://youtrack.jetbrains.com/issue/TW-97094) — S3 storage: introduction of converters for acl
* [**TW-80951**](https://youtrack.jetbrains.com/issue/TW-80951) — Incorrect order of build problems in the new UI
* [**TW-66822**](https://youtrack.jetbrains.com/issue/TW-66822) — Create cloud profile. Unable to set Kubernetes namespace using Magic Wand.
* [**TW-97582**](https://youtrack.jetbrains.com/issue/TW-97582) — Build Statistics are published in build_data_storage with a delay
* [**TW-97612**](https://youtrack.jetbrains.com/issue/TW-97612) — Wait time reason displayed as "Other" instead of showing detailed resource/agent info
* [**TW-90524**](https://youtrack.jetbrains.com/issue/TW-90524) — Protocol.ResponseProcessCookies - Invalid cookie header
* [**TW-96021**](https://youtrack.jetbrains.com/issue/TW-96021) — Auto-completion and resolving by hover doesn't work for branch specifications
* [**TW-95973**](https://youtrack.jetbrains.com/issue/TW-95973) — TCP Run in Docker: Autocomplete hangs on search
* [**TW-86142**](https://youtrack.jetbrains.com/issue/TW-86142) — Sakura UI: cannot select text on the Change Log tab on records with merge requests
* [**TW-96057**](https://youtrack.jetbrains.com/issue/TW-96057) — Run in Docker (Dockerfile): after switching to Path, execution still uses Command-line content
* [**TW-95188**](https://youtrack.jetbrains.com/issue/TW-95188) — Do not skip update to revision from VCS if project settings can't be changed via UI
* [**TW-96954**](https://youtrack.jetbrains.com/issue/TW-96954) — Failed to load build settings from VCS for perforce sparse streams
* [**TW-96402**](https://youtrack.jetbrains.com/issue/TW-96402) — Blocked output due to artifact preprocessing
* [**TW-2209**](https://youtrack.jetbrains.com/issue/TW-2209) — Agent can be outdated with "Some plugins are out of date" message
* [**TW-97449**](https://youtrack.jetbrains.com/issue/TW-97449) — Wait reasons values with long description value can be lost because of description truncation
* [**TW-79391**](https://youtrack.jetbrains.com/issue/TW-79391) — Versioned Settings status is updated on secondary nodes with delay
* [**TW-96916**](https://youtrack.jetbrains.com/issue/TW-96916) — [Docker] Plugin build failed with java.lang.ClassNotFoundException
* [**TW-96926**](https://youtrack.jetbrains.com/issue/TW-96926) — Do not auto-assign mutes/investigations on timed out builds
* [**TW-94936**](https://youtrack.jetbrains.com/issue/TW-94936) — Changes in settings are not detached from a build configuration without builds in default branch
* [**TW-97274**](https://youtrack.jetbrains.com/issue/TW-97274) — Slow opening of a custom build dialog in case of a re-run of a build chain with auto-generated builds
* [**TW-94903**](https://youtrack.jetbrains.com/issue/TW-94903) — Cancelling a build during a checkout may cause 'Cannot read file C:\Users\builduser\.config\jgit\config' failures and corrupted Git mirrors
* [**TW-96994**](https://youtrack.jetbrains.com/issue/TW-96994) — Start build log blocks for Matrix Builds and Parallel tests with capital letter
* [**TW-96453**](https://youtrack.jetbrains.com/issue/TW-96453) — Do not allow to add a build chain to the queue if it references an already deleted build promotion
* [**TW-96899**](https://youtrack.jetbrains.com/issue/TW-96899) — Matrix build feature doesn't respect the max running builds setting
* [**TW-96781**](https://youtrack.jetbrains.com/issue/TW-96781) — Webhook + Pull Request: strange warning in VCS logs
* [**TW-96065**](https://youtrack.jetbrains.com/issue/TW-96065) — Hide field "Gradle wrapper path" if "Use gradle wrapper" is disabled
* [**TW-96833**](https://youtrack.jetbrains.com/issue/TW-96833) — Trigger may trigger builds in branches coming from DSL repository

### Performance Problem

* [**TW-98373**](https://youtrack.jetbrains.com/issue/TW-98373) — Copy build step action is hanging (no slow requests on the backend)
* [**TW-100420**](https://youtrack.jetbrains.com/issue/TW-100420) — VCS modifications cleaner produces slow running queries while removing unreachable commits
* [**TW-96974**](https://youtrack.jetbrains.com/issue/TW-96974) — Removal of an agent is too slow and holds multiple locks
* [**TW-99549**](https://youtrack.jetbrains.com/issue/TW-99549) — Too much memory is consumed by overview controller if VCS problem is reported
* [**TW-99557**](https://youtrack.jetbrains.com/issue/TW-99557) — Allow controlling the amount of text which we put to VcsException in case of an error
* [**TW-100111**](https://youtrack.jetbrains.com/issue/TW-100111) — Pipelines: Constant high CPU usage when trying to save pipeline with cyclic dependency in it
* [**TW-99747**](https://youtrack.jetbrains.com/issue/TW-99747) — Potentially unnecessary Git ls-remote operation is performed for every checking for changes
* [**TW-88121**](https://youtrack.jetbrains.com/issue/TW-88121) — swagger.json request processing occupies too much memory
* [**TW-73554**](https://youtrack.jetbrains.com/issue/TW-73554) — Consider disabling fetching of all of the fields in REST API requests
* [**TW-98259**](https://youtrack.jetbrains.com/issue/TW-98259) — Unnecessary computations are performed in the CommonBranchSpec probably because of the different order of the pull requests returned by the PullRequests build feature
* [**TW-98287**](https://youtrack.jetbrains.com/issue/TW-98287) — TeamCity performs fetch with ^refs/tags/*,+refs/*:refs/* refspec as soon as the number of branches in repository reaches 200
* [**TW-97164**](https://youtrack.jetbrains.com/issue/TW-97164) — The process of versioned settings applying (after a change in repository) produces many status updates for the projects hierarchy
* [**TW-98341**](https://youtrack.jetbrains.com/issue/TW-98341) — Severe performance issues on agent pages
* [**TW-98185**](https://youtrack.jetbrains.com/issue/TW-98185) — Should preload build promotions from DB in the SecuredBuildHistory::findEntries method
* [**TW-98083**](https://youtrack.jetbrains.com/issue/TW-98083) — Slow removal of a large project tree (slow ProblemMutingServiceImpl.projectRemoved listener)
* [**TW-97495**](https://youtrack.jetbrains.com/issue/TW-97495) — Open terminal link slows down displaying of the parts of the agent details page because of isLocal agent check
* [**TW-94867**](https://youtrack.jetbrains.com/issue/TW-94867) — S3 storage using AWS connection is significanly less performant comparing to static credentials
* [**TW-97990**](https://youtrack.jetbrains.com/issue/TW-97990) — Forbid direct imports from 'lodash'
* [**TW-90056**](https://youtrack.jetbrains.com/issue/TW-90056) — Slow opening of the "Promote build" dialog 
* [**TW-97168**](https://youtrack.jetbrains.com/issue/TW-97168) — High CPU usage due to heavy computations inside PullRequestsBranchSpecsConflict health report
* [**TW-97300**](https://youtrack.jetbrains.com/issue/TW-97300) — Big traffic from custom_data table produced by performUptodateCheck method
* [**TW-96958**](https://youtrack.jetbrains.com/issue/TW-96958) — Slow app/metrics request (takes minutes) on the secondary nodes because of VirtualAgentsManagerImpl.getStartingAgentsNum
* [**TW-93850**](https://youtrack.jetbrains.com/issue/TW-93850) — Remove read lock on Git mirror directory in getCurrentState API


### Security

20 security problems have been fixed.
To learn more about fixed vulnerabilities directly related to TeamCity, check out our [Security Bulletin](https://www.jetbrains.com/privacy-security/issues-fixed/?product=TeamCity&version=2026.1).

Security bulletins are typically published few days after the release date.


