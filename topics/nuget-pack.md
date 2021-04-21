[//]: # (title: NuGet Pack)
[//]: # (auxiliary-id: NuGet Pack)

The _NuGet Pack_ build runner allows building a NuGet package from a given specification file. If you want to publish this package, add a [NuGet Publish](nuget-publish.md) build step.

<include src="nuget.md" include-id="nuget-OS"/>

>To view the NuGet Installer's settings in [Kotlin DSL](kotlin-dsl.md), click __View as code__ in the sidebar.

Configure the following options of the NuGet Pack runner:

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

Select a NuGet version to use from the drop-down menu ([NuGet](nuget.md) must be installed), or specify a custom path to `NuGet.exe`.

<note>

An appropriate version of .NET Framework must be installed on the agent machine, depending on the used `NuGet.exe` version: NuGet 2.8.6+ requires .NET 4.5+, earlier NuGet versions require .NET 4.0.
</note>

</td></tr><tr>

<td>

Specification files

</td>

<td>

Enter path(s) to `csproj` or `nuspec` file(s). You can specify as many specification files here as you need. Wildcards are supported. If you specify here a `csproj` file, you won't have to redefine the version number and copyright information in the spec file.

</td></tr><tr>

<td>

Prefer project files to .nuspec

</td>

<td>

Check the box to use the project file (if exists,that is `.csproj` or `.vbproj`) for every matched `.nuspec` file.

</td></tr><tr>

<td>

Version

</td>

<td>

Specify the package version. Overrides the version number from the `nuspec` file. You can use the TeamCity variable `%build.number%` here.

</td></tr><tr>

<td>

Base Directory

</td>

<td>

Select an option from the drop-down menu to specify the directory where the files defined in the `nuspec` file are located (the directory against which the paths in `<files></files>` from `nuspec` are resolved; usually some `bin` directory). If _Use explicit directory_ is enabled and this field is left blank, TeamCity will use the build checkout directory as the base directory.

</td></tr><tr>

<td>

Output directory

</td>

<td>

Specify the path where to put the generated NuGet package.

</td></tr><tr>

<td>

Сlean output directory

</td>

<td>

Clean the directory before packing.

</td></tr><tr>

<td>

Publish created packages to build artifacts

</td>

<td>

If you are using TeamCity as a NuGet repository, select this option to publish packages to the TeamCity's NuGet server and be able to use them as regular TeamCity artifacts.

</td></tr><tr>

<td>

Exclude files

</td>

<td>

Specify one or more wildcard patterns to exclude when creating a package. Equivalent to the `-Exclude` argument of `NuGet.exe`.

</td></tr><tr>

<td>

Properties

</td>

<td>

Semicolon or new-line separated list of package creation properties. For example, to make a release build, you define here `Configuration=Release`.

</td></tr><tr>

<td>

Options


</td>

<td>

__Create tool package__ — check the box to place the output files of the project to the `tool` folder.    
__Include sources and symbols__ — check the box to create a package containing sources and symbols. When specified with a nuspec, it creates a regular NuGet package file and the corresponding symbols package (needed for publishing the sources to [Symbolsource](http://www.symbolsource.org/) )

</td></tr><tr>

<td>

Command line parameters

</td>

<td>

Set additional command line parameters to be passed to `NuGet.exe`.

</td></tr></table>

<seealso>
        <category ref="admin-guide">
            <a href="nuget-installer.md">NuGet Installer</a>
            <a href="nuget-publish.md">NuGet Publish</a>
        </category>
</seealso>
