[//]: # (title: NuGet Publish)
[//]: # (auxiliary-id: NuGet Publish)

The __NuGet Publish__ build runner is intended to publish (`push`) your NuGet packages to a given feed (custom or default).

<tip>

When using TeamCity as a NuGet server, there are two ways to publish packages to the feed:
* as build artifacts of the [NuGet Pack](nuget-pack.md) build step using the _Publish created packages to build artifacts_ checkbox \- in this case you do not need the NuGet Publish build step
* via the NuGet Publish build step
</tip>


<include src="nuget.md" include-id="nuget-OS"/>
 
<tip>

To view the NuGet Installer's settings in [Kotlin DSL](kotlin-dsl.md), click __View DSL__ in the sidebar.

</tip>

This page describes the NuGet Publish runner options:

<table><tr>

<td>

Option


</td>

<td>

Description


</td></tr><tr>

<td>

NuGet.exe


</td>

<td>

Select a NuGet version to use from the drop\-down list (you need to have [NuGet installed](nuget.md)), or specify a custom path to `NuGet.exe`.

<note>

An appropriate version of .NET Framework installed on the agent machine is required depending on the NuGet.exe version used: NuGet 2.8.6\+ requires .NET 4.5\+, earlier NuGet versions require .NET 4.0.
</note>


</td></tr><tr>

<td>

Packages


</td>

<td>

Specify a newline\-separated list of NuGet package files (.nupkg) to publish to the NuGet feed. List packages individually or use wildcards.


</td></tr><tr>

<td>

API key


</td>

<td>

Specify the API key to access a NuGet packages feed.

To publish to the TeamCity NuGet server, specify the `%teamcity.nuget.feed.api.key%` parameter.


</td></tr><tr>

<td>

Package Source


</td>

<td>

Specify the destination NuGet packages feed URL to push packages to, for example, [`nuget.org`](http://nuget.org). Leave blank to let NuGet decide what package repository to use. If you are using a [TeamCity NuGet feed](using-teamcity-as-nuget-feed.md), select it using the 'magic wand' icon ![magic-wand.png](magic-wand.png) or manually specify the URL from the [NuGet Feed](using-teamcity-as-nuget-feed.md) section of the __Project Settings__.

If you work with an authenticated feed, configure the [NuGet Feed Credentials](nuget-feed-credentials.md) build feature.   

TeamCity allows you to authenticate using private NuGet feeds. Read more in [NuGet](nuget.md#Authentication+in+private+NuGet+Feeds).

#### Replacing existing package version in TeamCity internal feed

When publishing a package with the same version that already exists in a TeamCity internal NuGet feed, the package will be rejected. To force the TeamCity server to replace the existing NuGet package with a new version, append your feed URL obtainable from the __Project Settings__ page with the `?replace=true` parameter, for example, `http://<Teamcity URL>/httpAuth/app/nuget/feed/NuGet/default/v2?replace=true`.


</td></tr><tr>

<td>

Command line parameters

</td>

<td>

Enter additional parameters to use when calling the `nuget push` command.

</td></tr></table>

 <seealso>
        <category ref="admin-guide">
            <a href="nuget-installer.md">NuGet Installer</a>
            <a href="nuget-pack.md">NuGet Pack</a>
        </category>
</seealso>