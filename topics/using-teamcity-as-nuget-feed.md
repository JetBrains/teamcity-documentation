[//]: # (title: Using TeamCity as NuGet Feed)
[//]: # (auxiliary-id: Using TeamCity as NuGet Feed)

If you want to publish your NuGet packages to a limited audience, for example, to use them internally, you can use [TeamCity as a NuGet feed](https://docs.microsoft.com/en-us/nuget/hosting-packages/overview).        
__Since TeamCity 2018.2__, you can configure multiple NuGet feeds for a project in TeamCity.

<note>

TeamCity running on any of the supported operating systems (Windows, Linux, macOS) can be used as a NuGet feed.
</note>

On this page:

<tag-list of="chapter" mode="tree" depth="4"/>

## Enabling NuGet Feed

To start using TeamCity as a NuGet Server, you need to enable the NuGet Feed at the project level.

Go to the __NuGet Feed__ page on the Project Settings page and enable the feed.      
__Since TeamCity 2018.2__, the built-in TeamCity NuGet feed supports API v1/v2/v3. 

On the page you will see the following feed endpoints:
    

   * `http://<teamcityUrl>/<authSchema>/app/nuget/feed/<projectName>/<feedName>/v1`
   * `http://<teamcityUrl>/<authSchema>/app/nuget/feed/<projectName>/<feedName>/v2`
   * `http://<teamcityUrl>/<authSchema>/app/nuget/feed/<projectName>/<feedName>/v3/index.json`
   

If you have enabled [guest user](guest-user.md) you will see the toggle between Basic HTTP authentication and Guest authentication for NuGet feed Endpoints.
* __Basic HTTP authentication__ (with httpAuth prefix) \- to access packages user should have "View project" permission.
* __Guest authentication__ (with guestAuth prefix) \- if a guest user login is enabled, packages will be visible to all users that have access to TeamCity server.

To reference TeamCity project feeds, use parameters as follows:
 
```Shell  
`teamcity.nuget.feed.<authSchema>.<projectName>.<feedName>.<apiVersion>
```   
where:
 * `authSchema` could be `guestAuth`/`httpAuth`
 *`apiVersion` is `v1`/`v2`/`v3`, for instance: `teamcity.nuget.feed.httpAuth._Root.default.v2`.

On upgrading TeamCity to version 2018.2 or later, the deprecated parameters referencing the global NuGet feed will be automatically converted to the references to the default NuGet feed:

<table>
<tr>
<td>

Deprecated references

</td>

<td>

Current References

</td></tr><tr>

<td>

`teamcity.nuget.feed.server`

</td>

<td>

`teamcity.nuget.feed.guestAuth._Root.default.v2`

</td></tr><tr>

<td>

`teamcity.nuget.feed.auth.server`

</td>

<td>

`teamcity.nuget.feed.httpAuth._Root.default.v2`

</td></tr><tr>

<td>

`system.teamcity.nuget.feed.auth.serverRootUrlBased.server`
 
</td>
 
<td>
 
 `teamcity.nuget.feed.httpAuth._Root.default.v2`

</td></tr></table>

## Indexing NuGet Packages

### Indexing packages published as artifacts

By default, TeamCity will not add .nupkg artifacts published by builds into the project NuGet feed. You can select one of the following options:
* to index packages published by selected build configurations only, add the [NuGet packages indexer](nuget-packages-indexer.md) build feature to these build configurations
* to index all `.nupkg` files published as build artifacts in the project, enable __Automatic NuGet Packages Indexing__ in the __NuGet Feed__ section of the __Project Settings__
* use the [NuGet Pack](nuget-pack.md) build step with the _Publish created packages to build artifacts_ checkbox.  
Besides, the `.nupkg` files indexing is performed by the agent itself while publishing build artifacts.

<note>

Deleting a NuGet Feed with all its contents from a project will remove all [NuGet Packages Indexer](nuget-packages-indexer.md) features pointing to this feed.
</note>

### Using NuGet push command

To publish the `.nupkg` file into TeamCity NuGet feed during the build, you can specify the NuGet feed URL as a package source and the `%teamcity.nuget.feed.api.key%` value as a feed key in the following build steps:
*  [NuGet Publish](nuget-publish.md)
* .[NET CLI](net-cli-dotnet.md) with `nuget push` command

### Symbol Packages

[//]: # (AltHead:symbols)

Publishing of NuGet [symbol packages](http://docs.nuget.org/ndocs/create-packages/symbol-packages) to an internal TeamCity feed may cause issues when using an external source server. See the [corresponding issue](https://youtrack.jetbrains.com/issue/TW-25512) in our public tracker.

## Using TeamCity NuGet Feed

You can add TeamCity NuGet feeds as package sources package on your developer machine, for example, to use packages during development: use the [`nuget sources`](https://docs.microsoft.com/en-us/nuget/tools/cli-ref-sources) command or NuGet package management in your IDE.

You can use TeamCity NuGet feeds to restore packages in your builds: when the [NuGet Installer](nuget-installer.md) or [NuGet Publish](nuget-publish.md) build step is used, TeamCity will use the [credentials provider](https://docs.microsoft.com/en-us/nuget/reference/extensibility/nuget-exe-credential-providers) to automatically authenticate requests to the private TeamCity NuGet feeds.

<tip>

The packages available in the feed are bound to the builds' artifacts: they are removed from the feed when the artifacts of the build which produced them are [cleaned up](https://confluence.jetbrains.com/display/TCD18/Clean-Up). To avoid cleanup of artifacts in a specific build, [pin this build](pinned-build.md).
</tip>

 Internet Explorer settings may need to be set to trust the TeamCity Server when working in a mixed authentication environment.

[//]: # (Internal note. Do not delete. "Using TeamCity as NuGet Feedd342e197.txt")

__ __