[//]: # (title: NuGet Dependency Trigger)
[//]: # (auxiliary-id: NuGet Dependency Trigger)

The _NuGet dependency trigger_ allows starting a new build if a NuGet packages update is detected in the NuGet repository.

<note>

Currently, the NuGet dependency trigger supports only API versions 1 and 2 due to specifics of the Nuget.CommandLine tool.

</note>

## Requirements and limitations

For a TeamCity server running on __Windows__, __.NET 4.0__ is required.

For a TeamCity server running on __Linux__, the NuGet dependency trigger will reportedly work with the following __limitations__:
* filtering by Package Version Spec is not supported
* only HTTP package sources are supported
* NuGet feed version 1.0 is used, so case-sensitivity issues might occur
* the current trigger implementation on Linux might increase the server load
* authentication issues might occur

<anchor name="Configuring+NuGet+Dependency+Trigger"/>

## Triggering Settings

1. Select the NuGet version to use from the __NuGet.exe__ drop-down menu.
   >The recommended approach is to preinstall NuGet via __[Administration | Tools](nuget.md#Installing+NuGet+to+TeamCity+agents)__. In this case, the installed version will automatically appear in this menu. If you want to provide a custom path to a NuGet executable, you need to explicitly allow its usage on the server by specifying the following [internal property](server-startup-properties.md#TeamCity+Internal+Properties):
       ```Plain Text
       teamcity.nuget.server.cli.path.whitelist=<disk>:\\<path-to-executable>\\NuGet.exe
       ```
   > 
   {product="tc"}
2. Specify the NuGet package source, if it is different from `nuget.org`.
3. Specify the credentials to access NuGet feed if required.
4. Enter the package ID to check for updates.
5. Optionally, you can specify [package version range](https://docs.microsoft.com/en-us/nuget/reference/package-versioning#version-ranges-and-wildcards) to check for. If not specified, TeamCity will check for latest version.

You can also opt to trigger build if pre-release package version is detected by selecting corresponding checkbox. Note that this is only supported for NuGet version 1.8 or newer.

## Triggered Build Customization

<include from="configuring-vcs-triggers.md" element-id="triggered-build-customization"/>

 <seealso>
        <category ref="admin-guide">
            <a href="nuget.md">NuGet</a>
        </category>
</seealso>

