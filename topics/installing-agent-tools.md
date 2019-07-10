[//]: # (title: Installing Agent Tools)
[//]: # (auxiliary-id: Installing Agent Tools)
In TeamCity an _agent tool_ (i.e. a set of files/a binary distribution) is a type of plugin without any classes loaded into the runtime; agent tools are used to only distribute binary files to agents.

TeamCity allows you to install/remove additional tools on all the agents, which is especially useful in the environments with many build agents as you can distribute tools to or remove them from all build agents at once, centralize configuration files distribution (for example, you want to distribute a custom configuration file/library to all agents), and so on.

The __Administration | Tools__ page provides a unified interface to set up tools to be used by appropriate plugins. You can install different versions of any tool and/or change the default one. The tools will be automatically distributed to all build agents to be used in the related runners. 

The following types of tools can be managed up via the __Administration | Tools__ page:
* __IntelliJ Inspections and Duplicates Engine__ with the bundled version of IntelliJ IDEA set as default.
* __JetBrains dotCover Command Line Tools__ with the bundled version set as default. Used to collect code coverage for your .NET project 
* __JetBrains ReSharper Command Line Tools__: by default the tools are bundled with TeamCity and are used by Inspections (.NET), Duplicates Finder (.NET) build runners to run code analysis. 
* __Maven:__ several bundled versions are displayed, with 3.0.5 set as default.
* __NuGet.exe__ used in NuGet specific build steps and NuGet Dependency Trigger. NuGet packages (`.nupkg` files) with the `tools/NuGet.exe` file inside are supported.
* __NUnit 3:__ different versions can be installed and the default version set/changed
* __Sysinternals handle.exe__ used to determine processes which hold files in the checkout directory on Windows agents.
* __Sysinternals psexec.exe__ required for installing a TeamCity agent from a Windows server to a Windows host using Agent push
* You can also upload __your own tool as a .zip archive__: the structure of the tool plugin is described on the [Plugins Packaging page](https://plugins.jetbrains.com/docs/teamcity/plugins-packaging.html#Tools). TeamCity will use the name of the zip file as the tool name on all agents. The zip file will be automatically unpacked on the agents to the directory with the same name.   
When a tool is installed, a separate section appears on the page displaying the installed version(s), the tool usages, the default version or the ability to change the default. You can also remove an installed tool/version.

You can see that the tool appears on the agent in the TeamCity Web UI by checking [configuration parameters reported by the agent](predefined-build-parameters.md#Agent+Properties) in the form `teamcity.tool.<the installed tool id>`. You can use this parameter in your build: reference this parameter in the TeamCity Web UI (anywhere where the `%parameter%` format is supported) or refer to this parameter in your build as an environment or a system [parameter](configuring-build-parameters.md).

To distribute a tool to agents, TeamCity places them into the \<[TeamCity Data Directory](teamcity-data-directory.md)\>\/plugins/.tools and monitors the content of this folder.

<note>

When you install a tool on the server, agents restart and pick it up but do not download it until starting the first build that requests this tool. This ensures that agents download only required and compatible tools.   
Once downloaded, the tools are stored on agents so builds don't spend time on downloading them again.

</note>