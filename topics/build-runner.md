[//]: # (title: Build Runner)
[//]: # (auxiliary-id: Build Runner)
_Build runner_ is a part of TeamCity that allows integration with a specific build tool (Ant, MSBuild, Command Line, and so on). In a build configuration, a build runner defines how to run a build and report its results.

Each build runner has two parts:
* the server\-side settings that are configured through the web UI
* the agent\-side part that executes a build on an agent

TeamCity comes bundled with the following runners:
* [.NET CLI (dotnet)](net.md)
* [.NET Process Runner](net-process-runner.md)
* [Ant](ant.md)
* [Command Line](command-line.md) – run an arbitrary command line
* [Deployers](deployers.md) 
  * [Container Deployer](container-deployer.md)
  * [FTP Upload](ftp-upload.md)
  * [SMB Upload](smb-upload.md)
  * [SSH Upload](ssh-upload.md)
  * [SSH Exec](ssh-upload.md)
* [Duplicates Finder (ReSharper)](duplicates-finder-resharper.md)
* [Duplicates Finder (Java)](duplicates-finder-java.md)
* [FxCop](fxcop.md)
* [Gradle](gradle.md)
* [Inspections (IntelliJ IDEA)](inspections.md) – a set of IntelliJ IDEA inspections
* [Inspections (ReSharper)](inspections-resharper.md) – a set of ReSharper inspections
* [IntelliJ IDEA Project](intellij-idea-project.md), and its earlier version: [Ipr (deprecated)](ipr-deprecated.md)
* [Maven](maven.md)
* [MSBuild](msbuild.md)
* [MSpec](mspec.md)
* [NAnt](nant.md)
* [NuGet-related runners](nuget.md)
* [NUnit](nunit.md)
* [PowerShell](powershell.md)
* [Rake](rake.md)
* [Simple Build Tool (Scala)](https://confluence.jetbrains.com/pages/viewpage.action?pageId=74844978)
* [Visual Studio (sln)](visual-studio-sln.md) – Microsoft Visual Studio 2005/2008/2010/2012/2013/2015 solutions
* [Visual Studio 2003](visual-studio-2003.md) – Microsoft Visual Studio 2003 solutions
* [Visual Studio Tests](visual-studio-tests.md) (with integrated MSTest)
* [Xcode Project](xcode-project.md)

Technically, build runners are implemented as plugins.

Build runners are configurable in the [Configuring Build Steps](configuring-build-steps.md) section of the [Create/Edit Build Configuration](creating-and-editing-build-configurations.md) page.

 <seealso>
        <category ref="admin-guide">
            <a href="configuring-build-steps.md">Configuring Build Steps</a>
        </category>
</seealso>