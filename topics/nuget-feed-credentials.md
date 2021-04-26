[//]: # (title: NuGet Feed Credentials)
[//]: # (auxiliary-id: NuGet Feed Credentials)

When using NuGet packages from an external authenticated feed during a build on TeamCity, the credentials for connecting to that feed have to be specified.

Adding this information to source control is not a secure practice, so TeamCity provides the __NuGet Feed Credentials__ [build feature](adding-build-features.md) which allows interacting with feeds that require authentication.

When editing the build configuration, from the list of available [Build Features](adding-build-features.md) select __NuGet Feed Credentials__. In the dialog that is opened, specify the feed URL and the credentials to connect to the feed. To view the settings in [Kotlin DSL](kotlin-dsl.md), click __View as code__.

You can add this build feature to any build configuration, one for every feed that requires authentication.

When using TeamCity as the [internal NuGet server](using-teamcity-as-nuget-feed.md), the credentials specified via this build feature are ignored; the internal TeamCity authentication is used.
{product="tc"}
