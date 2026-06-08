[//]: # (title: What's New in TeamCity Cloud)

<show-structure for="chapter" depth="2"/>

## Build ???, 15 June 2026

TeamCity Cloud is entering a new phase with smaller, more frequent updates. Rather than following the TeamCity On-Premises release schedule, it will now ship improvements on a faster cadence. This means you will get fixes, performance enhancements, and requested changes sooner.

While this release does not introduce major new features, it delivers more than 70 fixes and minor improvements. See the Full changelog section for the complete list of changes.


### Full changelog
{id=changelog-2026.2.1}

<deflist collapsible="true">
    <def title="Minor improvements" default-state="collapsed">
        <ul>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-55009"><b>TW-55009</b></a> — Projects import: preserve IDs on importing to empty server, make a clear warning that IDs are changed when they do</li>
        </ul>
    </def>
    <def title="Pipeline enhancements" default-state="collapsed">
        <ul>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-100427"><b>TW-100427</b></a> — Pipelines: Edit Repository: CSP and PR toggles are available for Any Git Repo with auth type - Anonymous</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-96418"><b>TW-96418</b></a> — Pipeline integrations: bring the "Test connection" back</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-95617"><b>TW-95617</b></a> — Pipeline yaml parse error should contain location in the yaml</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-97441"><b>TW-97441</b></a> — Pipelines: Agent Requirement: RAM Amount condition doesn't work</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-100917"><b>TW-100917</b></a> — Adding tags to a pipeline leads to "Error while loading data", the pipeline is inaccessible</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-100374"><b>TW-100374</b></a> — Pipelines: Edit Repository: Missing CSP status in the repository component description</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-94668"><b>TW-94668</b></a> — Pipeline is broken after project copying</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99711"><b>TW-99711</b></a> — An empty Repository entity is created if the user switches from the pipeline setting to the Admin area</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-93400"><b>TW-93400</b></a> — Pipelines: highlight repository names by exact match, not by symbols</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99898"><b>TW-99898</b></a> — Pipeline steps description isn't aligned with steps if docker file is used in some of the steps</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-100226"><b>TW-100226</b></a> — Do not show the text about DSL inconsistency from the DSL tab of pipelines stored in YAML</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99293"><b>TW-99293</b></a> — POM location is in the Advanced Maven settings for pipelines</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-96700"><b>TW-96700</b></a> — Cannot create Pipeline, if there is a Build configuration with the same name on the same project level</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-100063"><b>TW-100063</b></a> — /app/pipeline MCP requests do not work when user has active 2FA</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99693"><b>TW-99693</b></a> — Broken build if reusing a pipeline with failed jobs in a build chain</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99861"><b>TW-99861</b></a> — Limit Job Id and Job name length</li>
        </ul>
    </def>
    <def title="Fixed bugs" default-state="collapsed">
        <ul>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-43738"><b>TW-43738</b></a> — Allow Run Custom Build dialog to be resized (to show long values in drop-downs)</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-100713"><b>TW-100713</b></a> — Build cannot resolve dep. parameters, if it has optional artifact dependency on another skipped conditional dependency</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-94605"><b>TW-94605</b></a> — "Unmet requirements: exists:" for incompatible agents instead of clear description</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-96778"><b>TW-96778</b></a> — No "Log in" next to the newly created connection in the dropdown</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99890"><b>TW-99890</b></a> — Horizontal sidebar behaviour is unclear and unstable</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-96205"><b>TW-96205</b></a> — AWS SDK2: remove "(SDK Attempt Count: 1)" from the errors, return back the error code and service name</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-100707"><b>TW-100707</b></a> — CONNECT: 401 on access to K8s via proxy with login and password</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-94724"><b>TW-94724</b></a> — Creation flow: repository description is always absent for the connections</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-98233"><b>TW-98233</b></a> — NPM integration has unnesasary text and jumps</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-87045"><b>TW-87045</b></a> — No reaction after activation of the security patch (it is unclear whether the installation has actually started)</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-92752"><b>TW-92752</b></a> — “Store password and API tokens outside of VCS” gets hidden when you disable UI-managed project settings.</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99648"><b>TW-99648</b></a> — Using the Filter button on Change Log tab of Versioned Settings leads to 403 CRSF error</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99122"><b>TW-99122</b></a> — Perforce shelve trigger starts a build on excluded stream if feature branches support is disabled</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-100650"><b>TW-100650</b></a> — VCS trigger doesn't trigger build on empty branch</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-92837"><b>TW-92837</b></a> — Free Disk Space should be able to delete volumes of docker builder</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-98678"><b>TW-98678</b></a> — Failed DSL compilation due to invalid cache after moving a Perforce label</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-100234"><b>TW-100234</b></a> — Failed to resolve artifact dependency in multinode setup with external artifact storage</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99808"><b>TW-99808</b></a> — Build features UI: placeholder in the search field refers to non-existing items</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-100476"><b>TW-100476</b></a> — All compatible agents are outdated and cannot be upgraded” is shown even though some agents are currently undergoing the upgrade process</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-95156"><b>TW-95156</b></a> — Deadlock on attempt to save data to test_metadata table</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-100169"><b>TW-100169</b></a> — Artifact is missing from the TeamCity web UI while existing in the external storage</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-95958"><b>TW-95958</b></a> — TCP Run in Docker: Docker Image autocomplete prioritizes loose substring matches over prefix</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-100305"><b>TW-100305</b></a> — The Add Step dialog is not switched automatically to "All" when searching</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-100620"><b>TW-100620</b></a> — "Force virtual host addressing" checkbox doesn't work</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-100745"><b>TW-100745</b></a> — Cloud Image can't be moved to a pool not containing the project that contains this image.</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-100891"><b>TW-100891</b></a> — Dorm: Canceled build without agent cannot be displayed: "You do not have enough permissions to view requested agent pool"</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-100946"><b>TW-100946</b></a> — .NET builds with qualifier agent requirements starting with "Exists=&gt;" cannot find compatible agents if there are incompatible agents in the same pool</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-101089"><b>TW-101089</b></a> — Lots of duplicate entry errors on attempt to store a metric id to the dictionary table</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-100761"><b>TW-100761</b></a> — Perforce failing on newlines in changelist description</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-78597"><b>TW-78597</b></a> — Agent alternate IP address ignored by TeamCity</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-100890"><b>TW-100890</b></a> — Dorm tenant cannot see its own agent pool with error "You do not have enough permissions to view requested agent pool"</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99613"><b>TW-99613</b></a> — Working better with non-default branches using MCP</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-101033"><b>TW-101033</b></a> — Cleanup of build type groups can fail with an unknown error</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-100760"><b>TW-100760</b></a> — Rake build steps are broken (plugin is damaged)</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-100744"><b>TW-100744</b></a> — Authorize virtual agents action should validate agent pool for cloud agents</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-100212"><b>TW-100212</b></a> — [S3] Uploads fail with 403 status code</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-100985"><b>TW-100985</b></a> — Build-scoped tokens UX: add descriptions and docs link</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-100573"><b>TW-100573</b></a> — Properties for key preserving on rotation (time.min and time.days) are not working</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-100962"><b>TW-100962</b></a> — TeamCity cleanup does not remove cancelled multi-node tasks</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-97236"><b>TW-97236</b></a> — Creation flow: handle Unauthorized exception in the UI</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-100377"><b>TW-100377</b></a> — Excessive `/user` calls during repository listing in BitBucket Cloud connection</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-96422"><b>TW-96422</b></a> — Provide more information about the origin of the TeamCity server workspaces</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99396"><b>TW-99396</b></a> — IndexOutOfBoundsException in org.jetbrains.teamcity.fus.client.events.TeamCityListener</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99974"><b>TW-99974</b></a> — Mark test ignored if it was interrupted and parent block gets closed</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-97638"><b>TW-97638</b></a> — Stop build command from the server may cancel the step set to be always executed</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99340"><b>TW-99340</b></a> — Messages "Agent will upgrade" and "Agent can't be upgraded automatically" can be shown at the same time</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-101176"><b>TW-101176</b></a> — Incorrect "mcpClientToolName" sent to FUS during connection to MCP with Gemini, Copilot and some other tools</li>
        </ul>
    </def>
    <def title="Resolved performance issues" default-state="collapsed">
        <ul>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-100646"><b>TW-100646</b></a> — Project import loads the whole custom_data_body table</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-100629"><b>TW-100629</b></a> — Saving settings on Windows can be slow</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-100864"><b>TW-100864</b></a> — Repeating changes collection operations holds a reference to a previous operation</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-97758"><b>TW-97758</b></a> — Add a primary key to the ignored_tests table to help improve performance of DB replication</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-77267"><b>TW-77267</b></a> — BuildPTRIndexer$BuildTask.beforeExecute consumes CPU in case of a large queue</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-98141"><b>TW-98141</b></a> — PageExtensionsInterceptor is called for non-WEB POST requests</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-100723"><b>TW-100723</b></a> — Calculation of unprocessed node events takes a lot of time</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-100562"><b>TW-100562</b></a> — Processing of the events blocks on VCS commits cache initialization</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-100626"><b>TW-100626</b></a> — Slow loading of VCS leads to UI slowness</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-100647"><b>TW-100647</b></a> — Server cleanup duration ~3x regression on SQL Server after 2025.11.3 upgrade</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-100970"><b>TW-100970</b></a> — Too big thread name with lots of "commit" strings</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-97583"><b>TW-97583</b></a> — Duplicated IDs in the where clause of the SQL query can cause the PostgreSQL planner to switch to a full table scan</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-101005"><b>TW-101005</b></a> — Avoid loading the entire VCS root DAG of commits in memory</li>
        </ul>
    </def>
    <def title="Security" default-state="collapsed">
        Four security problems have been fixed. To learn more about fixed vulnerabilities directly related to TeamCity, check out our <a href="https://www.jetbrains.com/privacy-security/issues-fixed/?product=TeamCity+Cloud">Security Bulletin</a>.
        <note>Security bulletins are typically published a few days after the release date.</note>
    </def>
</deflist>




## Build 222577, 5 June 2026


### Java 21 Migration

TeamCity server and build agents no longer support Java versions older than Java 21. See this article for upgrade instructions: [](java-21.md).

> This requirement only defines the Java version needed for the server and agents to start successfully; it does not restrict the Java versions your TeamCity projects can build, test, or deploy against.

### TeamCity CLI

This update adds a new way to work with your TeamCity instances: TeamCity CLI. Alongside the browser-based UI and the extensive [REST API](https://www.jetbrains.com/help/teamcity/rest/teamcity-rest-api-documentation.html), you can now use a command-line tool to interact with TeamCity directly from the terminal.

Install the CLI on any machine to check build statuses, start new builds, investigate failures, and handle many other routine tasks without leaving the command line.

<img src="showcase.gif" alt="TeamCity CLI in action" border-effect="rounded"/>

[Learn more...](teamcity-cli.md)


### Integration with AI Agents

This update makes it easier to connect AI tools such as chatbots and agentic IDEs to TeamCity. You can choose between two integration options:

* The TeamCity `<server-url>/app/mcp` endpoint provides [MCP tools](ai-agent-integration.md#TeamCity+MCP) that let AI agents interact with TeamCity.
* The TeamCity CLI includes an [agent skill](ai-agent-integration.md#TeamCity+CLI) that helps agents work with TeamCity through terminal commands.

[Learn more...](ai-agent-integration.md)


### Pipeline Enhancements

* This version introduces a major enhancement for fully integrating pipelines into your CI/CD workflows: you can now [include them in build chains](pipeline-settings.md#Pipeline+Dependencies). This lets you create fine-grained setups with pipeline-to-pipeline, pipeline-to-configuration, and configuration-to-pipeline dependencies.

    <img src="dk-pipeline-dependency.png" width="706" alt="Pipeline dependency"/>

* We redesigned the build details side panel to include all familiar [Build Results](build-results-page.md) tabs (such as Overview, Build Log, Parameters, and more), giving you a complete view of each build. It now also includes a pipeline/job switch, so you can quickly filter these tabs by job, making pipelines easier to inspect, debug, and troubleshoot.

    <img src="pipelines-job-switch.png" width="706" alt="Pipeline-Job switch in the side panel"/>

* You can now use the [Custom build dialog](running-custom-build.md) that allows you to run pipelines with one-time custom settings.

    <img src="pipelines-run-custom-build.png" width="706" alt="Run build buttons in TeamCity"/>

* Jobs can now use the following build features, previously available only for [build configurations](adding-build-features.md):

    <img src="pipelines-build-features.png" width="706" alt="Build features in pipelines"/>

    * [](build-files-cleaner-swabra.md)
    * [](free-disk-space.md)
    * [](build-cache.md)
    * [](xml-report-processing.md)

* When editing pipeline [**Repositories**](pipeline-settings.md#Repository) settings, you can now add repositories from existing VCS roots owned by this parent project. Previously, the option to reuse a root was only available when you create a new pipeline.

* All pipelines that connect to Git repositories using HTTP credentials support repository options that [allow these pipelines to track pull requests and publish run statuses](pipeline-settings.md#Repository). Previously, these options were available only to pipelines created via a configured TeamCity OAuth connection.

    <img src="pipelines-csp-and-pr.png" width="706" alt="CSP and PR in pipelines"/>

* Pipeline parameters were redesigned to better support pipeline dependencies and provide a more consistent, intuitive model.

    * Jobs no longer define separate input and output parameters. Instead, all job parameters are automatically available to downstream jobs in the same pipeline.
    * Pipeline parameters can now be explicitly marked as input or output, giving you finer control over which values are exposed to downstream configurations and pipelines.
    * Parameters from parent projects no longer need to be imported as they are now automatically available to child pipelines and jobs.

* If a parent project enables its [versioned settings](storing-project-settings-in-version-control.md), pipeline YAML is automatically converted to DSL. Currently, the DSL support in pipelines is limited: changes made via TeamCity UI are displayed on a new **Kotlin DSL** tab, but are not committed automatically and prevent you from running the pipeline. Commit these changes manually to a remote `.kts` file to restore the pipeline's ability to run.

    <img src="pipelines-dsl.png" width="706" alt="DSL in pipelines"/>


### Dynamic Build Step Credentials

The new [](build-scoped-token.md) feature lets your builds securely generate short-lived GitHub access tokens (up to 60 minutes) on the fly. Pass them to build steps as parameters to enable seamless access to repositories.

<img src="dk-build-scoped-token-settings.png" width="706" alt="Main settings"/>

[Learn more...](build-scoped-token.md)


### SSH Known Hosts

The [SSH Keys](ssh-keys-management.md) page now includes additional options that allow TeamCity to verify VCS providers it connects to, and abort any additional operations if the host's public key does not match any of the known entries.

<img src="ssh-known-hosts.png" width="706" alt="SSH Known hosts"/>

[Learn more...](ssh-keys-management.md#Known+SSH+Hosts)

### Third-party Integration Enhancements

#### Git

* For security reasons, Git VCS roots no longer support [local and UNC file URLs](git.md#Supported+Git+Protocols) by default.

* When choosing the **Shallow clone** [Git checkout policy](git.md#git-checkout-policy), you can now add the `teamcity.git.agent.shallowCloneDepth` and `teamcity.git.agent.submodules.shallowCloneDepth` parameters to set the [`--depth`](https://git-scm.com/docs/git-clone) attribute.

* The [GitLab CE/EE connection](configuring-connections.md#GitLab) now allows you to configure integration with system webhooks. This enhancement allows TeamCity to receive near-instant notifications about new repository changes, as opposed to periodically [polling the repository](project-administrator-guide.md#Collecting+Changes).

* The [](commit-status-publisher.md) build feature now includes the option to set up a custom build configuration name when posting statuses to Git VCS providers (GitHub, GitLab, Bitbucket, and more).

    <img src="csp-custom-build-name.png" width="706" alt="CSP statuses in GitHub"/>

#### Perforce

* When building [Perforce shelved changelists](integrating-teamcity-with-perforce.md#Running+Builds+on+Perforce+Shelved+Files), earlier versions of TeamCity replaced checked-out files with corresponding shelved ones. Starting with this update, TeamCity uses a more sophisticated approach by running `p4 resolve` after unshelving, allowing it to detect and resolve conflicting changes.

  > Due to exclusive UAsset file checkout, this change can negatively affect Unreal Engine projects built in parallel. You can restore the previous behavior by adding the `teamcity.internal.perforce.useUnshelve=false` property to your project or server internal properties.

* You can now specify multiple [shelved changelist IDs](integrating-teamcity-with-perforce.md#Running+Builds+on+Perforce+Shelved+Files) when triggering a custom build for Perforce build configurations.

#### Kubernetes

* Kubernetes [cloud profiles](setting-up-teamcity-for-kubernetes.md#Kubernetes+Cloud+Profile+Configuration) and [connections](configuring-connections.md#Kubernetes) now include settings that allow you to configure outgoing connections behind a proxy.

#### HashiCorp Vault

* The [HashiCorp Vault Connection](hashicorp-vault.md#Set+Up+a+Vault+Connection) now supports authentication via [Google Cloud Platform authentication](https://developer.hashicorp.com/vault/docs/auth/gcp).

#### Jira

* When configuring [connections to on-premises Jira instances](jira.md), you can now choose between authentication via regular username/password credentials or a personal access tokens issued on the issue tracker side.

### Miscellaneous Enhancements

* All TeamCity build configurations now automatically record agent hardware usage during builds. This change introduces the following updates:

    * The **PerfMon** build feature is no longer required and has been renamed to **Performance Monitor (Legacy)**.
    * The corresponding tab on the **Build Results** page is now called [Performance Monitor](build-results-page.md#Performance+Monitor+Tab), reflecting that the data is no longer tied to the deprecated feature.
    * A new `teamcity.perfmon.feature.enabled` parameter allows you to disable CPU, disk, and memory usage collection for specific build configurations or projects.


* Users with trial TeamCity Enterprise licenses can now use [](ai-assistant.md).

* The list of available [Get artifacts from...](artifact-dependencies.md#artifact-dep-get-from) options now includes **Build from the same chain** that fails the build if both target and source configuration/pipeline do not belong to the same build chain. Previously, only the **Build from the same chain or last finished** option was available.

* In addition to the existing **Maximum concurrent builds for this build configuration** setting in [general build configuration settings](configuring-general-settings.md#Limit+Number+of+Simultaneously+Running+Builds), the new **If the limit is reached** option lets you choose whether TeamCity should queue excess builds or cancel the oldest running ones to free up capacity.

* We have implemented the [`override.dep.`](use-parameters-in-build-chains.md#Override+parameters+of+upstream+objects) parameter prefix that may completely replace the older `reverse.dep.` syntax in future TeamCity versions. New parameters can resolve parameter references and do not forcibly push edits to configurations/pipelines that do not have matching parameters.

*  The [SAML Authentication](configuring-authentication-settings.md#HTTP+SAML+2.0) plugin is now bundled with TeamCity, so you no longer need to install it separately to enable authentication through external SSO providers.

* We have [improved](https://youtrack.jetbrains.com/issue/TW-98507/) the Gradle version selection logic in our Gradle plugin. This change only adds a few info-level entries to the build logs and introduces no visible behavior changes, but it should improve the plugin’s overall stability.

### Full changelog
{id=changelog-2026.1}

<deflist collapsible="true">
    <def title="Implemented features" default-state="collapsed">
        <ul>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-92756"><b>TW-92756</b></a> — Support of MCP protocol to enable 3rd party integrations</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-82657"><b>TW-82657</b></a> — GitHub App connection: provide dynamic credentials to build steps.</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-96595"><b>TW-96595</b></a> — Allow to configure system webhooks for self-managed GitLab</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-42994"><b>TW-42994</b></a> — It should be possible to resolve value of reverse.dep.*** parameter in context of the head build</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99633"><b>TW-99633</b></a> — Add preset to create read-only token</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-87288"><b>TW-87288</b></a> — Make the 'Build Name' value in the Parameters description of the Commit Status Publisher configurable</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-77495"><b>TW-77495</b></a> — Perforce Shelve Trigger / Personal Build should run "p4 resolve"</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-79126"><b>TW-79126</b></a> — TeamCity Hashicorp Vault Google Cloud auth method</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-77514"><b>TW-77514</b></a> — support unshelving multiple perforce shelves with rest API</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-34709"><b>TW-34709</b></a> — Support for SAML Authentication</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-1858"><b>TW-1858</b></a> — Cancel an obsolete running build if there are more recent changes/queued builds</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-43295"><b>TW-43295</b></a> — Ability to pause build queue via REST api call</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-97730"><b>TW-97730</b></a> — Implement "same-chain" artifact dependency option</li>
        </ul>
    </def>
    <def title="Minor improvements" default-state="collapsed">
        <ul>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-93722"><b>TW-93722</b></a> — Discontinue support for Java versions earlier than 21 on both the server and agent</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-93287"><b>TW-93287</b></a> — SLNX Solution support</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-65203"><b>TW-65203</b></a> — Enable virtual host addressing in S3 by default</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-93979"><b>TW-93979</b></a> — Migrate bundled AWS-related plugins to SDK v2</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-86630"><b>TW-86630</b></a> — Log the Token Name of the User Access Token when used in requests</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-96506"><b>TW-96506</b></a> — Update bundled version of the dotCover Command Line Tools to 2025.1.7 version</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-96509"><b>TW-96509</b></a> — Update ReSharper Command Line Tool to 2025.2.3 version</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-97421"><b>TW-97421</b></a> — Support AI Assistant in trial Enterprise license</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-98507"><b>TW-98507</b></a> — Gradle integration: use the --version command to detect the Gradle version</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-98704"><b>TW-98704</b></a> — Gradle integration: get rid of the GradleConnector usage</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-98869"><b>TW-98869</b></a> — Gradle integration: use MultiCommandBuildSessionFactory to allow execution of multiple sequential commands in a container</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-95056"><b>TW-95056</b></a> — Add a max limit to the disk size of a build configuration parameter</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-79999"><b>TW-79999</b></a> — SakuraUI: Impossible to send a link to the failed test</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-94330"><b>TW-94330</b></a> — Add commit date in addition to authored date to Change entity in rest</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-98203"><b>TW-98203</b></a> — Add progress to copy/delete operations in artifacts-migration-tool</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-96441"><b>TW-96441</b></a> — Add retries for PowerShell detection command</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-97738"><b>TW-97738</b></a> — Comments to builds are hardly searchable</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-96827"><b>TW-96827</b></a> — Implement batching for the ignored tests</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-97332"><b>TW-97332</b></a> — Update default JaCoCo tool version</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-97955"><b>TW-97955</b></a> — Add the ability to copy code snippets in the AI Assistant</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-96267"><b>TW-96267</b></a> — Make TeamCity agent scripts compatible with Java 24 &amp; 25</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-97446"><b>TW-97446</b></a> — Switch to git log command for the build revisions calculation</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-96384"><b>TW-96384</b></a> — Support configurable Git shallow clone depth (e.g. --depth=2)</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-96513"><b>TW-96513</b></a> — Update Maven tool to 3.9.11 version</li>
        </ul>
    </def>
    <def title="Pipeline enhancements" default-state="collapsed">
        <ul>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-92582"><b>TW-92582</b></a> — Multi-repo support for pipelines using VCS roots from parent project</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-96208"><b>TW-96208</b></a> — Pipeline overview</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-98715"><b>TW-98715</b></a> — Add Swabra, free disk space, build cache, xml report processing features to pipeline jobs</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99230"><b>TW-99230</b></a> — Support VCS hosting build features configuration in Pipelines</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-95286"><b>TW-95286</b></a> — Snapshot dependencies for Pipelines</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-94763"><b>TW-94763</b></a> — Support Pipelines as Kotlin object in DSL</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-100134"><b>TW-100134</b></a> — Run custom build for pipelines</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-95544"><b>TW-95544</b></a> — Merge Pipeline runners: group dotnet commands in UI</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99770"><b>TW-99770</b></a> — Provide better instructions for MCP for finding build configurations/pipelines related to repository</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99308"><b>TW-99308</b></a> — Pipeline fails to start with "Cannot locate Maven" if bundled maven version was chosen in pipeline (version that exists on server)</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-97361"><b>TW-97361</b></a> — It's unclear from the UI and docs, where job artifacts will be published</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-98149"><b>TW-98149</b></a> — Running a personal pipeline with a broken yaml results in 500 error and a broken pipeline run page</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-97481"><b>TW-97481</b></a> — The tooltip on the 'Save' button is hard to read (Pipeline Editing page)</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-98480"><b>TW-98480</b></a> — Pipeline build runners auto-generate YAML with quotes in boolean values</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-97647"><b>TW-97647</b></a> — The popup with actions is not shown for pipeline dependencies</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-97010"><b>TW-97010</b></a> — Parameters: Show UI error when trying to override imported parameter on the pipeline level</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-96907"><b>TW-96907</b></a> — Import Parameters: Expand name and value text fields in case of multiline input</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-96436"><b>TW-96436</b></a> — No confirmation for the 'Cancel run' action</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-96280"><b>TW-96280</b></a> — Pipelines investigations: Show in build log opens Problem tab instead</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-96162"><b>TW-96162</b></a> — Misplaced VCS root name on Edit repository page for existing VCS root</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-95598"><b>TW-95598</b></a> — Order of pipelines steps resets in Editing Job page in UI after saving the changes (the pipeline runs steps in the changed order)</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-95437"><b>TW-95437</b></a> — TCP Merge: modification of a job output breaks the pipeline with dependencies</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-97524"><b>TW-97524</b></a> — Muting test in the selected pipeline doesn't work</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-97389"><b>TW-97389</b></a> — No errors about incompatible agents in pipeline settings, if parameter references can't be resolved on the Pipeline level</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-98826"><b>TW-98826</b></a> — Problems tree on should correctly display problems from pipeline run</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99146"><b>TW-99146</b></a> — Pipeline editing is broken with no clear reason if there are problems with inherited connections</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99104"><b>TW-99104</b></a> — Pipeline status badge shows "Failed" status and icon instead of "Failed to start"</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99105"><b>TW-99105</b></a> — Pipeline cancelation reason is not shown in Pipeline run status badge</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99008"><b>TW-99008</b></a> — Secondary node REST API returns finished build with status FAILURE but empty build problems</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99466"><b>TW-99466</b></a> — "Provide missing secure values" from pipeline-related server health errors should lead to parent project "Tokens"</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99249"><b>TW-99249</b></a> — The 'Agent Terminal' button is not displayed on the pipeline run page when parallel tests are used</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-98922"><b>TW-98922</b></a> — Click at banner closes the settings panel in Pipelines</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99096"><b>TW-99096</b></a> — "Changes since last successful build" is available in pipelines but returns error 400</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-98935"><b>TW-98935</b></a> — Edit pipeline indicator is hidden behind the pipeline name</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99383"><b>TW-99383</b></a> — Steps are not disabled in Pipeline Jobs</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-94636"><b>TW-94636</b></a> — YAML/Visual view is not preserved between pipelines</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-98827"><b>TW-98827</b></a> — Tests tree on should correctly display problems from pipeline run</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-98974"><b>TW-98974</b></a> — Update the colour of the selected pipeline error state</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-97704"><b>TW-97704</b></a> — Jump to the pipeline name in the create pipeline form</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-98864"><b>TW-98864</b></a> — Misaligned parameter names in Pipeline settings in Safari</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-98769"><b>TW-98769</b></a> — Display dependencies for pipeline runs in the build details</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-97367"><b>TW-97367</b></a> — Pipelines use html url for creating new VCS roots from connections</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-98237"><b>TW-98237</b></a> — Disabled and disconnected agents are shown as compatible for queued Pipeline Jobs</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-95622"><b>TW-95622</b></a> — Better handling of cases of incorrect pipelines url openings</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-97146"><b>TW-97146</b></a> — Show the dotnet devenv action in the pipeline Editor</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-97424"><b>TW-97424</b></a> — 'VCS root not accessible from configuration' error message should be adjusted to take pipelines into account</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-97292"><b>TW-97292</b></a> — Collapsed "Outputs" sections for Pipeline jobs shows "Shared files: object Object"</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-96674"><b>TW-96674</b></a> — Docker/NPM Integration allows saving with empty URL, causing error during job execution</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-100111"><b>TW-100111</b></a> — Constant high CPU usage when trying to save pipeline with cyclic dependency in it</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-95431"><b>TW-95431</b></a> — Job-level artifacts: impossible to download via the button</li>
        </ul>
    </def>
    <def title="Fixed bugs" default-state="collapsed">
        <ul>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-97326"><b>TW-97326</b></a> — Perforce virtual streams: only latest change is detected from two separate submissions</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-93185"><b>TW-93185</b></a> — Could not create the main application servlet: ReadOnlyException: Writing to file "/storage/system/pluginData/cloudProfileIdx" is prohibited by TeamCity node restrictions</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-94700"><b>TW-94700</b></a> — Improve versioned settings errors health report</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99684"><b>TW-99684</b></a> — It's unclear how to overcome error about inaccessible dependencies in Build configurations</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99821"><b>TW-99821</b></a> — p4 unshelve: running p4 clean may remove undesired files</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-100013"><b>TW-100013</b></a> — 'Cannot find a node' error in build VCS changes calculation</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99243"><b>TW-99243</b></a> — A failing bootstrap step does not prevent subsequent steps from running</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-96398"><b>TW-96398</b></a> — Swabra build feature creates checkout directory snapshot before sources checkout when used with a Bootstrap build step</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-95293"><b>TW-95293</b></a> — Dependency build fails to load build settings when the dependency relationship is defined only in a DSL feature branch</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-80432"><b>TW-80432</b></a> — Kubernetes Plugin does not allow to configure proxy to cluster</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-85749"><b>TW-85749</b></a> — Personal patch is not applied for Parallel Tests and Matrix builds virtual dependencies</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99775"><b>TW-99775</b></a> — Adding build type to favorites fails</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-95112"><b>TW-95112</b></a> — Rename Docker Repository to Docker Registry</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-94020"><b>TW-94020</b></a> — TCP Merge: cloud images are not listed in incompatible agents when the agent is starting</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99275"><b>TW-99275</b></a> — Parameter name disappears when configuring custom agent requirement in Firefox</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99022"><b>TW-99022</b></a> — The bars on the optimization chart are stuck to the top edge</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-94106"><b>TW-94106</b></a> — "Save on TeamCity server" button has no feedback</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-95358"><b>TW-95358</b></a> — TCP Merge: text area for multi-line script step is too narrow</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-97451"><b>TW-97451</b></a> — Change of the value type key case can lead to missing statistic values in the database</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-96950"><b>TW-96950</b></a> — VCS root uses SSH keys specified in .ssh/config on the server machine, if "Uploaded key" is selected, but the key is not valid</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-98038"><b>TW-98038</b></a> — Inherited S3 storage is inaccessible in subprojects when “Limit build access permissions” is enabled and AWS connection uses IAM Role</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-96881"><b>TW-96881</b></a> — Last run deletion leads to empty table with non-functioning links</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-77853"><b>TW-77853</b></a> — Versioned Settings Change Log tab is empty for a user with "View build configuration settings" and without "Edit project" permission</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-97745"><b>TW-97745</b></a> — Old artifacts Cleaner cache files remain in data directory, if keepArtifactsNewCache is enabled</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-97791"><b>TW-97791</b></a> — Do not show parameter as unresolved when it's used only in step which requires existence of that parameter</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-98071"><b>TW-98071</b></a> — Patch for select is added when the parameter is edited via UI</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-96738"><b>TW-96738</b></a> — AWS Connection: old access key is not removed from AWS when using the Rotate Keys button.</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-95821"><b>TW-95821</b></a> — OAuth authentication doesn't work with default port of Idea Built-in server</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-96429"><b>TW-96429</b></a> — Optimize PowerShell detection on Windows by avoiding launching the PowerShell executable when possible</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-96434"><b>TW-96434</b></a> — Fix PowerShell detection on Windows for PowerShell Core edition</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-97531"><b>TW-97531</b></a> — Cloud instances synchronization thread fails to save some of the cloud instances to the database</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-100496"><b>TW-100496</b></a> — Incorrect generated Pre-signed S3 URLs for HEAD method</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-82851"><b>TW-82851</b></a> — Error: No enum constant javax.lang.model.element.Modifier.SEALED while compiling a build</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-77295"><b>TW-77295</b></a> — IntelliJ IDEA projects runner doesn't work if run under java 16 and later: No signature of method: org.codehaus.groovy.tools.RootLoader.getPackage() is applicable for argument types: (java.lang.String)</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99919"><b>TW-99919</b></a> — Azure Pat connection: TFS root isn't recognized during build configuration creation</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-94482"><b>TW-94482</b></a> — "Could not start new instances. Quota exceeded" warning message on cloud image - K8S</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-87567"><b>TW-87567</b></a> — AWS Connections with IAM Roles can't access connections from parent projects</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99407"><b>TW-99407</b></a> — Exception on attempt to finish a build if there are many unresolved output parameters</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-100380"><b>TW-100380</b></a> — Test status is shown incorrectly if test runs are sorted by status on the test history page</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-100102"><b>TW-100102</b></a> — Build fails with "The build was triggered in the branch ... which does not correspond to any branch monitored by the build VCS roots..." erroneously if it has VCS root attached which is also a settings VCS root in some other project</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-92128"><b>TW-92128</b></a> — Simplify locating thread dumps reported on execution timeout</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-98160"><b>TW-98160</b></a> — K8S executor: executor requires a template image to have java installed in /opt/java</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-97967"><b>TW-97967</b></a> — K8s executor: requirements via env. variables are incorrectly resolved</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99639"><b>TW-99639</b></a> — Update AI disable feedback</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99267"><b>TW-99267</b></a> — Incorrect parameter resolution for a not yet started build</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99137"><b>TW-99137</b></a> — It's unclear why server was not started, if teamcity-server.sh couldn't find minimal Java</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99604"><b>TW-99604</b></a> — Sakura Agent Page: Miscellaneous section is missing</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-95473"><b>TW-95473</b></a> — Unrelated projects in the projects popup of the test history page</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-19752"><b>TW-19752</b></a> — VCS root 'Test connection' only shows one failed mapping at a time in Perforce</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99922"><b>TW-99922</b></a> — Azure TFS VCS Root can't connect to repository</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-100061"><b>TW-100061</b></a> — Windows Docker images: the dotCover { … } step fails due to non-canonical ACLs</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-82689"><b>TW-82689</b></a> — Personal Access Tokens for Jira Server in Jira Integration are not supported</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-97024"><b>TW-97024</b></a> — Automatically interrupt long running HTTP requests</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-67712"><b>TW-67712</b></a> — Changing AMI in Amazon Cloud Profile terminates the instances running from old AMI even if they are running the builds</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-98671"><b>TW-98671</b></a> — Commit status publisher Build Feature description doesn't show the correct and sufficient information</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-93456"><b>TW-93456</b></a> — Matrix builds: Reverse dependencies not resolved in matrix builds</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99640"><b>TW-99640</b></a> — Feedback form is not shown after admin chose the "Keep AI Assistant disabled"</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-95465"><b>TW-95465</b></a> — Incorrect link in snapshot dependency in Sakura UI</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-96694"><b>TW-96694</b></a> — Pause Build Queue/Resume Build Queue is missing from classic UI</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99757"><b>TW-99757</b></a> — Remove the vertical scrollbar</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-93396"><b>TW-93396</b></a> — K8S Cloud Profile Windows Pod Template can break</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99478"><b>TW-99478</b></a> — Constant warnings "Failed to find telegraf executable in PATH" in teamcity-agent.log</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-95283"><b>TW-95283</b></a> — Perforce: `forceTrust` option from build parameters is ignored in some cases</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-97821"><b>TW-97821</b></a> — S3 retry mechanism for error code 0: check all possible scenarios are covered</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-96602"><b>TW-96602</b></a> — "Stop build" button partially obscured in classic UI</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-98442"><b>TW-98442</b></a> — Prompt password parameter is provided in encrypted form if a custom value is specified</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-95561"><b>TW-95561</b></a> — TCP Steps: "(and 0 more line)" can be shown in the step settings if all the script is visible</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99128"><b>TW-99128</b></a> — Virtual configuration can inherit triggers from the default template</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-97951"><b>TW-97951</b></a> — Review the empty states of the main navigation</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-98197"><b>TW-98197</b></a> — Don’t show CSAT after major update for users who rarely visit the server</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99203"><b>TW-99203</b></a> — Free disk space UI: prefill the default 3gb value for the feature</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-91601"><b>TW-91601</b></a> — Node.js Runner: Build Step Auto-detection suggest a deprecated package for eslint which ends up as a build problem</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-98865"><b>TW-98865</b></a> — Remove "Copy link to test" from dropdown for HTTP servers</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-52584"><b>TW-52584</b></a> — Order of SBuild.getFailureReasons() (add timestamp to build problem to allow their sorting)</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99023"><b>TW-99023</b></a> — The Log In button is stuck to the left edge</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-93063"><b>TW-93063</b></a> — Versioned Settings ignore "custom path" when TeamCity automatically adds ID to requirements set without ID in XML-files.</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99075"><b>TW-99075</b></a> — NullPointerException inside assignRolesDialogContent_jsp</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99021"><b>TW-99021</b></a> — Long branch names extend beyond the block</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-98176"><b>TW-98176</b></a> — Delete the hardcode of specific version of Node in Node.js runner</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-98967"><b>TW-98967</b></a> — Dependency preview state remains when user navigates away</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-97394"><b>TW-97394</b></a> — "Copy answer" button is hidden if CSAT is disabled</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-97351"><b>TW-97351</b></a> — FUS events can be lost if users click too fast</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-96093"><b>TW-96093</b></a> — Benchmark plugin: the benchmark is stuck because the connectivity between created agents and the server cannot be verified</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-98092"><b>TW-98092</b></a> — "Repository not found" errors during token refresh</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-95159"><b>TW-95159</b></a> — Slack notifier should raise a system problem if it failed to send a message</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-98101"><b>TW-98101</b></a> — Show the reaction on copying the code snippets from AI Assistant</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-93646"><b>TW-93646</b></a> — "Invalid target pool for image" must not fail whole cloud profile</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-98746"><b>TW-98746</b></a> — Create the Kotlin DSL setting for the 'Build Name' value in the Parameters description of the Commit Status Publisher</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-97668"><b>TW-97668</b></a> — Some EC2 Launch Templates are missing from the Cloud Image settings</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-91965"><b>TW-91965</b></a> — Reintroduce the Project Pool option on EC2 UI</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-97753"><b>TW-97753</b></a> — Project Change Log shows only 2 pages in Sakura</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-95537"><b>TW-95537</b></a> — Broken documentation link for GitHub Enterprise API in GitHub Release build step and commit status publisher settings</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-98123"><b>TW-98123</b></a> — Agent temp directories may not be correctly set when running an agent on Java 21</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-98622"><b>TW-98622</b></a> — Compare Builds shows the same test in different rows</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-94858"><b>TW-94858</b></a> — Kubernetes Cloud Profile image settings are sometimes not available in the web UI</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-89324"><b>TW-89324</b></a> — KeepArtifactsCleanerCache consumes a lot of disk space</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-97344"><b>TW-97344</b></a> — Kubernetes Cloud images: Cannot view image settings for read-only projects</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-97337"><b>TW-97337</b></a> — Delete image dialog is empty in K8s cloud profile</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-97343"><b>TW-97343</b></a> — Kubernetes cloud images: Do not show "Delete image" link for read-only projects</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-97630"><b>TW-97630</b></a> — 400 Status with connection reset</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-98113"><b>TW-98113</b></a> — Incorrect link to the queued build page</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-97729"><b>TW-97729</b></a> — Builds suitable for reuse are only checked when a shared resource is available</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-97259"><b>TW-97259</b></a> — A lot of "Error occurred while parsing frontend analytics events" warning on secondary node logs</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-98251"><b>TW-98251</b></a> — Don't show the re-encryption health report for after the update</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-97318"><b>TW-97318</b></a> — Cryptic error at attempt to create project/configuration from git URL with wrong token</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-98252"><b>TW-98252</b></a> — Allow to run re-encryption with the same key, if previous reencryption is marked as successful</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-97854"><b>TW-97854</b></a> — Sporadic build-scoped tokens related message in logs</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-96844"><b>TW-96844</b></a> — Read-only secondary node cannot generate new encryption key</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-78885"><b>TW-78885</b></a> — "Expand all" tests button on Build Overview does not expand them all</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-97731"><b>TW-97731</b></a> — Acquire VCS auth token: Unclear error 'Parameter "state" is missing', if browser URL doesn't match server URL</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-97874"><b>TW-97874</b></a> — Clicking the link to issues in changes popup does nothing</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-94894"><b>TW-94894</b></a> — The executor in the StatisticsPublisher class doesn't have a queue limit</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-98055"><b>TW-98055</b></a> — Indefinitely running build after Normal Executor RejectedExecutionException</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-97896"><b>TW-97896</b></a> — Use git fetch with --no-show-forced-updates option</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-94397"><b>TW-94397</b></a> — Jira Integration throws No Content to Map Exception</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-98048"><b>TW-98048</b></a> — Fix width for "New Project" button in Firefox</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-98073"><b>TW-98073</b></a> — Broken TestName comparator</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-97739"><b>TW-97739</b></a> — Jira Cloud status publishing logs messages into the teamcity-server.log</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-97345"><b>TW-97345</b></a> — New build creation flow: Create duplicate VCS root from other Repository Url lands the user on the connection creation page</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-94572"><b>TW-94572</b></a> — Error "Failed to read "usage-statistics" plugin state" in teamcity-server.log after update to 2025.07</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-97094"><b>TW-97094</b></a> — S3 storage: introduction of converters for acl</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-80951"><b>TW-80951</b></a> — Incorrect order of build problems in the new UI</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-66822"><b>TW-66822</b></a> — Create cloud profile. Unable to set Kubernetes namespace using Magic Wand.</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-97582"><b>TW-97582</b></a> — Build Statistics are published in build_data_storage with a delay</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-97612"><b>TW-97612</b></a> — Wait time reason displayed as "Other" instead of showing detailed resource/agent info</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-90524"><b>TW-90524</b></a> — Protocol.ResponseProcessCookies - Invalid cookie header</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-96021"><b>TW-96021</b></a> — Auto-completion and resolving by hover doesn't work for branch specifications</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-95973"><b>TW-95973</b></a> — TCP Run in Docker: Autocomplete hangs on search</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-86142"><b>TW-86142</b></a> — Sakura UI: cannot select text on the Change Log tab on records with merge requests</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-96057"><b>TW-96057</b></a> — Run in Docker (Dockerfile): after switching to Path, execution still uses Command-line content</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-95188"><b>TW-95188</b></a> — Do not skip update to revision from VCS if project settings can't be changed via UI</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-96954"><b>TW-96954</b></a> — Failed to load build settings from VCS for perforce sparse streams</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-96402"><b>TW-96402</b></a> — Blocked output due to artifact preprocessing</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-2209"><b>TW-2209</b></a> — Agent can be outdated with "Some plugins are out of date" message</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-97449"><b>TW-97449</b></a> — Wait reasons values with long description value can be lost because of description truncation</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-79391"><b>TW-79391</b></a> — Versioned Settings status is updated on secondary nodes with delay</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-96916"><b>TW-96916</b></a> — Plugin build failed with java.lang.ClassNotFoundException</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-96926"><b>TW-96926</b></a> — Do not auto-assign mutes/investigations on timed out builds</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-94936"><b>TW-94936</b></a> — Changes in settings are not detached from a build configuration without builds in default branch</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-97274"><b>TW-97274</b></a> — Slow opening of a custom build dialog in case of a re-run of a build chain with auto-generated builds</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-94903"><b>TW-94903</b></a> — Cancelling a build during a checkout may cause 'Cannot read file C:\Users\builduser\.config\jgit\config' failures and corrupted Git mirrors</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-96994"><b>TW-96994</b></a> — Start build log blocks for Matrix Builds and Parallel tests with capital letter</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-96453"><b>TW-96453</b></a> — Do not allow to add a build chain to the queue if it references an already deleted build promotion</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-96899"><b>TW-96899</b></a> — Matrix build feature doesn't respect the max running builds setting</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-96781"><b>TW-96781</b></a> — Webhook + Pull Request: strange warning in VCS logs</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-96065"><b>TW-96065</b></a> — Hide field "Gradle wrapper path" if "Use gradle wrapper" is disabled</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-96833"><b>TW-96833</b></a> — Trigger may trigger builds in branches coming from DSL repository</li>
        </ul>
    </def>
    <def title="Resolved performance issues" default-state="collapsed">
        <ul>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-96974"><b>TW-96974</b></a> — Removal of an agent is too slow and holds multiple locks</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-98373"><b>TW-98373</b></a> — Copy build step action is hanging (no slow requests on the backend)</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-100420"><b>TW-100420</b></a> — VCS modifications cleaner produces slow running queries while removing unreachable commits</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99549"><b>TW-99549</b></a> — Too much memory is consumed by overview controller if VCS problem is reported</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99557"><b>TW-99557</b></a> — Allow controlling the amount of text which we put to VcsException in case of an error</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99747"><b>TW-99747</b></a> — Potentially unnecessary Git ls-remote operation is performed for every checking for changes</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-88121"><b>TW-88121</b></a> — swagger.json request processing occupies too much memory</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-73554"><b>TW-73554</b></a> — Consider disabling fetching of all of the fields in REST API requests</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-98259"><b>TW-98259</b></a> — Unnecessary computations are performed in the CommonBranchSpec probably because of the different order of the pull requests returned by the PullRequests build feature</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-98287"><b>TW-98287</b></a> — TeamCity performs fetch with ^refs/tags/*,+refs/*:refs/* refspec as soon as the number of branches in repository reaches 200</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-97164"><b>TW-97164</b></a> — The process of versioned settings applying (after a change in repository) produces many status updates for the projects hierarchy</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-98341"><b>TW-98341</b></a> — Severe performance issues on agent pages</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-98185"><b>TW-98185</b></a> — Should preload build promotions from DB in the SecuredBuildHistory::findEntries method</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-98083"><b>TW-98083</b></a> — Slow removal of a large project tree (slow ProblemMutingServiceImpl.projectRemoved listener)</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-97495"><b>TW-97495</b></a> — Open terminal link slows down displaying of the parts of the agent details page because of isLocal agent check</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-94867"><b>TW-94867</b></a> — S3 storage using AWS connection is significanly less performant comparing to static credentials</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-97990"><b>TW-97990</b></a> — Forbid direct imports from 'lodash'</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-90056"><b>TW-90056</b></a> — Slow opening of the "Promote build" dialog</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-97168"><b>TW-97168</b></a> — High CPU usage due to heavy computations inside PullRequestsBranchSpecsConflict health report</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-97300"><b>TW-97300</b></a> — Big traffic from custom_data table produced by performUptodateCheck method</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-96958"><b>TW-96958</b></a> — Slow app/metrics request (takes minutes) on the secondary nodes because of VirtualAgentsManagerImpl.getStartingAgentsNum</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-93850"><b>TW-93850</b></a> — Remove read lock on Git mirror directory in getCurrentState API</li>
        </ul>
    </def>
    <def title="Security" default-state="collapsed">
        21 security problems have been fixed. To learn more about fixed vulnerabilities directly related to TeamCity, check out our <a href="https://www.jetbrains.com/privacy-security/issues-fixed/?product=TeamCity+Cloud">Security Bulletin</a>.
        <note>Security bulletins are typically published a few days after the release date.</note>
    </def>
</deflist>



## Roadmap

See the [TeamCity Roadmap](https://www.jetbrains.com/teamcity/roadmap/#teamcity-roadmap) and [](pipelines-roadmap.md) articles to learn about future updates.


## Update TeamCity Cloud
{instance="tcc"}

JetBrains maintains TeamCity Cloud servers, so no action is needed on your part. During an update, your instance is briefly unavailable. We will notify you beforehand via email.

If you do not see the latest features described here, your instance may not be upgraded yet. [Contact our support team](troubleshooting.md) for assistance.


## Your Feedback Matters

We place a high value on your feedback and encourage you to share your thoughts and suggestions. See this link for more information: [](troubleshooting.md).


