[//]: # (title: NuGet Installer)
[//]: # (auxiliary-id: NuGet Installer)

The _NuGet Installer_ build runner performs NuGet [Command-Line Package Restore](http://docs.nuget.org/docs/reference/package-restore#Command-Line_Package_Restore). It can also (optionally) automatically update package dependencies to the most recent ones.

<include src="nuget.md" include-id="nuget-OS"/>

<note>

Make sure that sources that you check out from VCS ([VCS Settings](configuring-vcs-settings.md)) include the folder called `packages` from your solution folder.
</note>

NuGet Installer settings:

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

Select NuGet version to use from the drop-down menu (you need to have [NuGet installed](nuget.md)) or specify a custom path to `NuGet.exe`.

<note>

An appropriate version of .NET Framework installed on the agent machine is required depending on the NuGet.exe version used: NuGet 2.8.6\+ requires .NET 4.5\+, earlier NuGet versions require .NET 4.0.
</note>


</td></tr><tr>

<td>

Path to solution file

</td>

<td>

Specify the path to your solution file (.sln) where packages are to be installed.

</td></tr><tr>

<td>

Restore Mode

</td>

<td>

Select `NuGet.exe restore` (requires NuGet 2.7\+) to restore all packages for an entire solution. The `NuGet.exe install` command is used to restore packages for versions prior to NuGet 2.7, but only for a single `packages.config` file.

</td></tr><tr>

<td>

Restore Options

</td>

<td>

If needed, select:

* __Exclude version from package folder names__: Equivalent to the `-ExcludeVersion` option of the `NuGet.exe install` command. If enabled, the destination folder will contain only the package name, not the version number.
* __Disable looking up packages from local machine cache__: Equivalent to the `-NoCache` option of the `NuGet.exe `

</td></tr><tr>

<td>

Packages Sources

</td>

<td>

Specify the NuGet package sources. If left blank, [`http://nuget.org`](http://nuget.org/) is used to search for your packages.

If you are using a [TeamCity NuGet feed](using-teamcity-as-nuget-feed.md), select it using the 'magic wand' icon ![magic-wand.png](magic-wand.png) or manually specify the URL from the NuGet Feed section of __Project Settings__.
{product="tc"}

If you use packages from an authenticated feed, configure the [NuGet Feed Credentials](nuget-feed-credentials.md) build feature.   

TeamCity allows you to authenticate using private NuGet feeds. Read more in [NuGet](nuget.md#Authentication+in+private+NuGet+Feeds).


</td></tr><tr>

<td>

Update Packages

</td>

<td>

__Update packages with help of NuGet update command__: Uses the `NuGet.exe update` command to update all packages under the solution. The package versions and constraints are taken from `packages.config` files.

</td></tr><tr>

<td>

Update Mode

</td>

<td>

Select one of the following:

* __Update via solution file__ — TeamCity uses Visual Studio solution file (.sln) to create the full list of NuGet packages to install. This option updates packages for the entire solution.
* __Update via packages.config__ — Select to update packages via calls to `NuGet.exe update Packages.Config` for each `packages.config` file under the solution.

</td></tr><tr>

<td>

Update Options

</td>

<td>

* __Include pre-release packages__: Equivalent to the `-Prerelease` option of the `NuGet.exe update` command
* __Perform safe update__: Equivalent to the `-Safe` option of the `NuGet.exe update` command, that looks for updates with the highest version available within the same major and minor version as the installed package.

</td></tr></table>

>To view the NuGet Installer's settings in [Kotlin DSL](kotlin-dsl.md), click __View as code__ in the sidebar.

See the [NuGet documentation](http://docs.nuget.org/docs/reference/command-line-reference) for the complete `NuGet.exe` command line reference.

When you add the NuGet Installer runner to your build configuration, each finished build will have the __NuGet Packages__ tab listing the packages used.

 <seealso>
        <category ref="blog">
            <a href="http://blog.jetbrains.com/teamcity/2013/08/nuget-package-restore-with-teamcity/">NuGet Package Restore with TeamCity</a>
        </category>
        <category ref="admin-guide">
            <a href="nuget-pack.md">NuGet Pack</a>
            <a href="nuget-publish.md">NuGet Publish</a>
        </category>
</seealso>