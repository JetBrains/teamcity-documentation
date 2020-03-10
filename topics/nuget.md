[//]: # (title: NuGet)
[//]: # (auxiliary-id: NuGet)

On this page:

<tag-list of="chapter" mode="tree" depth="4"/>

## Integration Capabilities

TeamCity integrates with [NuGet](https://github.com/nuget/home) package manager and when [NuGet is installed](#Installing+NuGet+to+TeamCity+agents) provides the following capabilities:
* [Private NuGet feeds](using-teamcity-as-nuget-feed.md) based on the builds' published artifacts.
* A set of NuGet runners to be used in builds on Windows OS, as well as on Linux and MacOS when [Mono](http://www.mono-project.com/docs/getting-started/install/) is installed on the agent. 
    * [NuGet Installer](nuget-installer.md) build runner, which installs and updates NuGet packages.
    * [NuGet Pack](nuget-pack.md) build runner, which builds NuGet packages.
    * [NuGet Publish](nuget-publish.md) build runner, which publishes packages to a feed of your choice.
* [NuGet dependency trigger](nuget-dependency-trigger.md), which allows triggering builds on NuGet feed updates.


<note include-id="nuget-OS">

__Supported Operating Systems__

NuGet build runners are supported on build agents running Windows OS by default. Linux and macOS are supported when [Mono](http://www.mono-project.com/docs/getting-started/install/) is installed on the agent (only NuGet 3.3\+ on Mono 4.4.2\+ is supported).

</note>

<note>

* NuGet build runners require an appropriate version on .NET Framework installed on the agent machine depending on the NuGet.exe version: 
    * NuGet versions 2.8.6+ require .NET 4.5+
    * NuGet versions \< 2.8.6 require .NET 4.0.
* To use packages from an authenticated feed, see the [NuGet Feed Credentials](nuget-feed-credentials.md) build feature.
</note>

## Typical Usage Scenarios

* To install packages from a public feed, add the [NuGet Installer](nuget-installer.md) build step.
* To create a package and publish it to a public feed, add the [NuGet Pack](nuget-pack.md) and [NuGet Publish](nuget-publish.md) build steps.
* To create a package and publish it to the internal TeamCity NuGet Server, enable TeamCity as a NuGet Server (see the [dedicated page](using-teamcity-as-nuget-feed.md)), use the [NuGet Pack](nuget-pack.md) build step and [NuGet Publish](nuget-publish.md) build steps.
* To trigger a new build when a NuGet package is updated, use the [NuGet Dependency Trigger](nuget-dependency-trigger.md).

## Installing NuGet to TeamCity agents

[//]: # (AltHead:installNuGet)

The NuGet trigger and the NuGet-related build runners require the NuGet command line binary configured on the server. They are automatically distributed to agents once configured. Several versions can be installed and a version of your choice can be set as the default one.

<tip>
It is recommended to use release versions of NuGet.
</tip>

To install `NuGet.exe` on TeamCity:
1. Go to the __Administration | Tools__ tab.
2. Click __Install tool and select NuGet.exe__.
3. Select whether you want to download (default) NuGet from the public feed or upload your own NuGet package containing `NuGet.exe`.   
   * If the __Download__ radio button is selected, specify the NuGet version to install on all build agents.
   * If the __Upload__ radio button is selected, specify your own NuGet package.
4. Specify whether this NuGet version will be default using the related checkbox. 
5. Click __Add__ to install the selected NuGet version.
 

<tip>

Installing NuGet on agents results in agents upgrade.
</tip>

## NuGet Packages Cache Clean-up on Agents

NuGet uses several local caches to avoid downloading packages that are already installed, and to provide offline support. If an agent is running out of the space, TeamCity will try to clean NuGet packages cache on the agent.

The caches in the following directories will be cleaned:
* `%%NUGET_PACKAGES%% environment variable` (must be an absolute path)
* `%%LOCALAPPDATA%%\NuGet\Cache`
* `%%LOCALAPPDATA%%\NuGet\v3-cache`
* `%%user.home%\.nuget\packages`

## Authentication in private NuGet Feeds

You can use authentication in [build-in NuGet feeds](using-teamcity-as-nuget-feed.md) or the feeds specified in the [NuGet feed credentials](nuget-feed-credentials.md) build feature. The credentials provider will automatically authenticate requests to these feeds.   

API support:
* __NuGet Installer / NuGet Publish runners__
   * __v3__: supported __since TeamCity 2018.2__, requires NuGet 4.8+ (Windows), NuGet 4.9+ on Mono
   * __v1/v2__: NuGet 2.0+ is minimum requirement, NuGet 3.5+ is recommended
* __.NET CLI__
   * __v3__: requires .NET CLI 2.1.500
   * __v1/v2__: not supported

## Proxy Configuration

NuGet command line client supports proxy server configuration via the `NuGet.config` file parameters or environment variables. See [NuGet documentation](https://docs.microsoft.com/en-us/nuget/schema/nuget-config-file#config-section) for more details.

__  __

__See also:__



__Administrator's Guide__: [NuGet Installer](nuget-installer.md) | [NuGet Publish](nuget-publish.md) | [NuGet Pack](nuget-pack.md) | [NuGet Dependency Trigger](nuget-dependency-trigger.md)

__ __