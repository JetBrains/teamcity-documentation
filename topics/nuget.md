[//]: # (title: NuGet)
[//]: # (auxiliary-id: NuGet)

## Integration Capabilities

TeamCity integrates with [NuGet](https://github.com/nuget/home) package manager and, when NuGet is installed on agents, provides the following capabilities:
* [Private NuGet feeds](using-teamcity-as-nuget-feed.md) based on the builds' published artifacts.
{product="tc"}
* A set of NuGet runners to be used in builds on Windows OS, as well as on Linux and macOS when [Mono](https://www.mono-project.com/docs/getting-started/install/) is installed on the agent. 
    * [NuGet Installer](nuget-installer.md) build runner, which installs and updates NuGet packages.
    * [NuGet Pack](nuget-pack.md) build runner, which builds NuGet packages.
    * [NuGet Publish](nuget-publish.md) build runner, which publishes packages to a feed of your choice.
* [NuGet dependency trigger](nuget-dependency-trigger.md), which allows triggering builds on NuGet feed updates.

>Note that TeamCity Cloud currently doesn't support automatic delivery of tools to [build agents](build-agent.md). To be able to use the NuGet runners, you need to download and install the required version of NuGet on the agent. You can do this manually (only on self-hosted agents) or via any convenient utility step at the beginning of the build (for example, [Command Line](command-line.md)). When configuring a NuGet build step, you will need to specify the path to NuGet relatively to the [build checkout directory](build-checkout-directory.md).
>
{type="warning" product="tcc"}

<snippet include-id="nuget-OS">

__Supported Operating Systems__:   
NuGet build runners are supported on build agents running Windows OS by default. Linux and macOS are supported when [Mono](https://www.mono-project.com/docs/getting-started/install/) is installed on the agent (only NuGet 3.3+ on Mono 4.4.2+ is supported).

</snippet>

<note>

* NuGet build runners require an appropriate version on .NET Framework installed on the agent machine depending on the `NuGet.exe` version: 
    * NuGet versions 2.8.6+ require .NET 4.5+
    * NuGet versions \< 2.8.6 require .NET 4.0.
* To use packages from an authenticated feed, see the [NuGet Feed Credentials](nuget-feed-credentials.md) build feature.
</note>

## Typical Usage Scenarios

* To install packages from a public feed, add the [NuGet Installer](nuget-installer.md) build step.
* To create a package and publish it to a public feed, add the [NuGet Pack](nuget-pack.md) and [NuGet Publish](nuget-publish.md) build steps.
* To create a package and publish it to the internal TeamCity NuGet Server, enable TeamCity as a NuGet Server (see the [dedicated page](using-teamcity-as-nuget-feed.md)), use the [NuGet Pack](nuget-pack.md) build step and [NuGet Publish](nuget-publish.md) build steps.
{product="tc"}
* To trigger a new build when a NuGet package is updated, use the [NuGet Dependency Trigger](nuget-dependency-trigger.md).

## Installing NuGet to TeamCity agents
{product="tc"}

[//]: # (AltHead:installNuGet)

The NuGet trigger and the NuGet-related build runners require the NuGet command line binary configured on the server. They are automatically distributed to agents once configured. Several versions can be installed and a version of your choice can be set as the default one.

>It is recommended to use release versions of NuGet.

To install `NuGet.exe` on TeamCity:
1. Go to the __Administration | Tools__ tab.
2. Click __Install tool and select NuGet.exe__.
3. Select whether you want to download (default) NuGet from the public feed or upload your own NuGet package containing `NuGet.exe`.   
   * If the __Download__ radio button is selected, specify the NuGet version to install on all build agents.
   * If the __Upload__ radio button is selected, specify your own NuGet package.
4. Specify whether this NuGet version will be default using the related checkbox. 
5. Click __Add__ to install the selected NuGet version.

>Installing NuGet on agents results in agents upgrade.

## NuGet Packages Cache Clean-up on Agents

NuGet uses several local caches to avoid downloading packages that are already installed, and to provide offline support. If an agent is running out of the space, TeamCity will try to clean NuGet packages cache on the agent.

The TeamCity NuGet cleaner cleans caches in the following Windows directories:
* `%\%NUGET_PACKAGES%% environment variable` (must be an absolute path)
* `%\%LOCALAPPDATA%%\NuGet\Cache`
* `%\%LOCALAPPDATA%%\NuGet\v3-cache`
* `%\%user.home%\.nuget\packages`

Since TeamCity 2019.2.3, the new automatic package cleaner has been introduced in addition to the existing NuGet cleaner. If .NET SDK is installed on a build agent, TeamCity will use native .NET SDK commands for cleaning. The new cleaner works across all platforms supporting .NET SDK and operates on several levels. It cleans the temporary cache first, and only then gets to cleaning the `.nupkg` files and HTTP requests, if necessary.

## Authentication in private NuGet Feeds

You can use authentication in [build-in NuGet feeds](using-teamcity-as-nuget-feed.md) or the feeds specified in the [NuGet feed credentials](nuget-feed-credentials.md) build feature. The credentials' provider will automatically authenticate requests to these feeds.
{product="tc"}

You can use authentication in the feeds specified in the [NuGet feed credentials](nuget-feed-credentials.md) build feature. The credentials' provider will automatically authenticate requests to these feeds.
{product="tcc"}

API support:
* __NuGet Installer / NuGet Publish runners__
   * __v3__: requires NuGet 4.8+ (Windows), NuGet 4.9+ on Mono
   * __v1/v2__: NuGet 2.0+ is minimum requirement, NuGet 3.5+ is recommended
* __.NET__
   * __v3__: requires .NET CLI 2.1.500
   * __v1/v2__: not supported

## Proxy Configuration

NuGet command line client supports proxy server configuration via the `NuGet.config` file parameters or environment variables. See [NuGet documentation](https://docs.microsoft.com/en-us/nuget/schema/nuget-config-file#config-section) for more details.

<seealso>
        <category ref="admin-guide">
            <a href="nuget-installer.md">NuGet Installer</a>
            <a href="nuget-publish.md">NuGet Publish</a>
            <a href="nuget-pack.md">NuGet Pack</a>
            <a href="nuget-dependency-trigger.md">NuGet Dependency Trigger</a>
        </category>
</seealso>