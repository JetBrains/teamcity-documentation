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
* NuGet feed version 1.0 is used, so case\-sensitivity issues might occur
* the current trigger implementation on Linux might increase the server load
* authentication issues might occur


## Configuring NuGet Dependency Trigger
1. Select the NuGet version to use from the __NuGet.exe__ drop\-down list (if you have [installed NuGet beforehand](nuget.md#Installing+NuGet+to+TeamCity+agents)), or specify a custom path to `NuGet.exe`;
2. Specify the NuGet package source, if it is different from `nuget.org`;
3. Specify the credentials to access NuGet feed if required
4. Enter the package Id to check for updates.
5. Optionally, you can specify [package version range](https://docs.microsoft.com/en-us/nuget/reference/package-versioning#version-ranges-and-wildcards) to check for. If not specified, TeamCity will check for latest version.

You can also opt to trigger build if pre-release package version is detected by selecting corresponding checkbox. Note that this is only supported for NuGet version 1.8 or newer.

 __  __

__See also:__


__Administrator's Guide__: [NuGet](nuget.md)

__ __
