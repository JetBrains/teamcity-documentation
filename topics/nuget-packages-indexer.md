[//]: # (title: NuGet Packages Indexer)
[//]: # (auxiliary-id: NuGet Packages Indexer)

NuGet packages indexer is an internal TeamCity tool that can index NuGet packages and add them to TeamCity [remote private feeds](using-teamcity-as-nuget-feed.md), with no need for additional authorization.

When the _NuGet packages indexer_ [build feature](adding-build-features.md) is added to a build configuration, all `.nupkg` files published as build artifacts will be indexed and added to the selected NuGet feed. Indexing is performed on the agent side.

<seealso>
        <category ref="troubleshooting">
            <a href="common-problems.md#Problems+with+TeamCity+NuGet+Feed">How to reindex the TeamCity NuGet Feed</a>
        </category>
</seealso>