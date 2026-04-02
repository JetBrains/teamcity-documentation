[//]: # (title: What's New in TeamCity On-Premises 2026.1)

<snippet id="2026-1-tc" instance="tc">


## TeamCity CLI

TeamCity 2026.1 adds a new way to work with your TeamCity instances: TeamCity CLI. Alongside the browser-based UI and the extensive [REST API](https://www.jetbrains.com/help/teamcity/rest/teamcity-rest-api-documentation.html), you can now use a command-line tool to interact with TeamCity directly from the terminal.

Install the CLI on any machine to check build statuses, start new builds, investigate failures, and handle many other routine tasks without leaving the command line.

<img src="showcase.gif" alt="TeamCity CLI in action" border-effect="rounded"/>

[Learn more...](teamcity-cli.md)


## Integration with AI Agents

Version 2026.1 also makes it easier to connect AI tools such as chatbots and agentic IDEs to TeamCity. You can choose between two integration options:

* The TeamCity `<server-url>/app/mcp` endpoint provides [MCP tools](ai-agent-integration.md#TeamCity+MCP) that let AI agents interact with TeamCity.
* The TeamCity CLI includes an [agent skill](ai-agent-integration.md#TeamCity+CLI) that helps agents work with TeamCity through terminal commands.

[Learn more...](ai-agent-integration.md)


## Pipeline Enhancements

* You can now run pipelines [with one-time custom settings](running-custom-build.md).

    <img src="pipelines-run-custom-build.png" width="706" alt="Run build buttons in TeamCity"/>

## Release Cycle Updates

Starting with this release, we return to the pre-2022 versioning scheme: major TeamCity versions will use the “YYYY.N” format, where “N” indicates the release number rather than the month. This change makes the release cycle more predictable and better aligned with other JetBrains products.

This year, we also expect to separate the release cadence for TeamCity On-Premises and Cloud. On-Premises will continue to receive two major releases per year. Cloud, on the other hand, will be updated more frequently, so new features and improvements become available sooner without waiting for a major On-Premises release.


## Dynamic Build Step Credentials

The new [](build-scoped-token.md) feature lets your builds securely generate short-lived GitHub access tokens (up to 60 minutes) on the fly. Pass them to build steps as parameters to enable seamless access to repositories.

<img src="dk-build-scoped-token-settings.png" width="706" alt="Main settings"/>

[Learn more...](build-scoped-token.md)


## SSH Known Hosts

The [SSH Keys](ssh-keys-management.md) page now includes additional options that allow TeamCity to verify VCS providers it connects to, and abort any additional operations if the host's public key does not match any of the known entries.

<img src="ssh-known-hosts.png" width="706" alt="SSH Known hosts"/>

[Learn more...](ssh-keys-management.md#Known+SSH+Hosts)

## Miscellaneous Enhancements

* The [HashiCorp Vault Connection](hashicorp-vault.md#Set+Up+a+Vault+Connection) now supports authentication via [Google Cloud Platform authentication](https://developer.hashicorp.com/vault/docs/auth/gcp).

* All TeamCity build configurations now automatically record agent hardware usage during builds. This change introduces the following updates:

    * The **PerfMon** build feature is no longer required and has been renamed to **Performance Monitor (Legacy)**.
    * The corresponding tab on the **Build Results** page is now called [Performance Monitor](build-results-page.md#Performance+Monitor+Tab), reflecting that the data is no longer tied to the deprecated feature.
    * A new `teamcity.perfmon.feature.enabled` parameter allows you to disable CPU, disk, and memory usage collection for specific build configurations or projects.


* When building [Perforce shelved changelists](integrating-teamcity-with-perforce.md#Running+Builds+on+Perforce+Shelved+Files), earlier versions of TeamCity replaced checked-out files with corresponding shelved ones. Starting with version 2026.1, TeamCity uses a more sophisticated approach by running `p4 resolve` after unshelving, allowing it to detect and resolve conflicting changes.

* For security reasons, Git VCS roots no longer support [local and UNC file URLs](git.md#Supported+Git+Protocols) by default. To re-enable them, set the `teamcity.git.allowFileUrl=true` [internal property](server-startup-properties.md#TeamCity+Internal+Properties).

* Users with trial TeamCity Enterprise licenses can now use the [](ai-assistant.md).

* Kubernetes [cloud profiles](setting-up-teamcity-for-kubernetes.md#Kubernetes+Cloud+Profile+Configuration) and [connections](configuring-connections.md#Kubernetes) now include settings that allow you to configure outgoing connections behind a proxy.

* When choosing the **Shallow clone** [Git checkout policy](git.md#git-checkout-policy), you can now add the `teamcity.git.agent.shallowCloneDepth` and `teamcity.git.agent.submodules.shallowCloneDepth` parameters to set the [`--depth`](https://git-scm.com/docs/git-clone) attribute.

</snippet>
