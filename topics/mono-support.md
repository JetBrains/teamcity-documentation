[//]: # (title: Mono Support)
[//]: # (auxiliary-id: Mono Support)

Mono framework is an alternative framework for running .NET applications on both Windows and Unix-based platforms.   
For more information refer to the [Mono official site](http://www.mono-project.com).

## Supported Build Runners

TeamCity supports running .NET builds using [NAnt](nant.md) and [MSBuild](msbuild.md) runners under Mono framework as well as under .NET Frameworks. (MSBuild as xbuild in Mono). 

[NuGet](nuget.md) runners support Linux and macOS when [Mono](http://www.mono-project.com/docs/getting-started/install/) is installed on the agent. Note that only NuGet CLI 3.2\+ on Mono 4.4.2\+ is supported. 

[Tests reporting tasks](net-testing-frameworks-support.md) are also supported under Mono.

## Mono Platform Detection

When a build agent starts, it detects a Mono installation automatically.

On each platform, Mono detection is compatible with NAnt one. See `NAnt.exe.config` for frameworks detection on NAnt.

### Agent Properties

When Mono is detected automatically on the agent side, the following properties are set:
* __Mono__ — path to the __mono__ executable (Mono JIT)
* __MonoVersion__ — Mono version
* __MonoX.Z__ — set to `MONO_ROOT/lib/mono/X.Z` if exists
* __MonoX.Z\_x64__ — set to `MONO_ROOT/lib/mono/X.Z` if exists and Mono architecture is x64
* __MonoX.Z\_x86__ — set to `MONO_ROOT/lib/mono/X.Z` if exists and Mono architecture is x86

If the Mono installation cannot be detected automatically (for example, you have installed Mono framework into a custom directory), you can make these properties available to build runners by setting them manually in the [agent configuration file](project-and-agent-level-build-parameters.md#Agent+Level+Build+Parameters).

#### Windows Specifics

Automatic detection of Mono framework under Windows has the following specifics:
1. The Mono version is read from `HKLM\SOFTWARE\Novell\Mono\DefaultCLR`.
2. The Frameworks paths are extracted from `HKLM\SOFTWARE\Novell\Mono\%\MonoVersion%`.
3. The platform architecture is detected by analyzing `mono.exe`.

#### macOS Specifics
1. The framework is detected automatically from `/Library/Frameworks/Mono.framework/Versions`.
2. The highest version is selected.
3. The frameworks path are extracted from `/Library/Frameworks/Mono.framework/Versions/%\MonoVersion%/lib/mono`.
4. The platform architecture is fixed to x86 as Mono official builds support only X86.

#### Custom Linux/Unix Specifics

Automatic detection of Mono framework under Unix has the following specifics:
1. Mono version is read from `pkg-config --modversion mono`.
2. The frameworks paths are extracted from `pkg-config --variable=prefix mono` and `pkg-config --variable=libdir mono`.
3. The platform architecture is detected by analyzing the `PREFIX/bin/mono` executable.
You can force Mono to be detected from a custom location by adding the `PREFIX/bin` directory to the beginning of the `PATH` and updating `PKG_CONFIG_PATH` (described in `pkg-config(1)`) with `PREFIX/lib/pkgconfig`.
 
<seealso>
        <category ref="admin-guide">
            <a href="nant.md">NAnt</a>
            <a href="msbuild.md">MSBuild</a>
            <a href="nuget.md">NuGet</a>
        </category>
</seealso>