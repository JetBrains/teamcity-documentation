[//]: # (title: Installing Agent Tools)
[//]: # (auxiliary-id: Installing Agent Tools)
In TeamCity an _agent tool_ (i.e. a set of files/a binary distribution) is a type of plugin without any classes loaded into the runtime; agent tools are used only to distribute binary files to agents.

TeamCity allows you to install/remove additional tools on the server and distribute them to build agents on-demand.   
In environments with many build agents you can centralize distribution of configuration files (for example, if you want to distribute a custom configuration file/library to all agents that require it) or remove a tool on agents at once.

The __Administration | Tools__ page provides a unified interface to set up tools to be used by appropriate plugins. You can install different versions of a tool and/or change the default one. The tools will be automatically distributed to build agents that request them and used in the related runners.

The following types of tools can be managed up via the __Administration | Tools__ page:
* __IntelliJ Inspections and Duplicates Engine__ with the bundled version of IntelliJ IDEA set as default.
* __JetBrains dotCover Command Line Tools__ with the bundled version set as default. Used to collect code coverage for your .NET project.
* __JetBrains ReSharper Command Line Tools__: by default the tools are bundled with TeamCity and are used by Inspections (.NET), Duplicates Finder (.NET) build runners to run code analysis.
* __Maven__: several bundled versions are displayed, with 3.0.5 set as default.
* __NuGet.exe__ used in NuGet specific build steps and NuGet Dependency Trigger. NuGet packages (`.nupkg` files) with the `tools/NuGet.exe` file inside are supported.
* __NUnit 3__: different versions can be installed and the default version set/changed.
* __Sysinternals handle.exe__ used to determine processes which hold files in the checkout directory on Windows agents.
* __Sysinternals psexec.exe__ required for installing a TeamCity agent from a Windows server to a Windows host using Agent push.
* You can also upload __your own tool as a .zip archive__: the structure of the tool plugin is described on the [Plugins Packaging page](https://plugins.jetbrains.com/docs/teamcity/plugins-packaging.html#Tools). TeamCity will use the name of the zip file as the tool name on all agents. The zip file will be automatically unpacked on the agents to the directory with the same name.   
When the first custom tool is installed, the __Zip Archive__ section appears on the page. In this section, you can see all the tool usages, remove the tool, or install a new one.

TeamCity places installed tools into the \<[TeamCity Data Directory](teamcity-data-directory.md)\>\/plugins/.tools and monitors the content of this folder.

__Since TeamCity 2019.1__, agents download only tools _required_ by builds they run. This way, the agents can run undemanding builds right after the server upgrade, with no need to download all available tools at once, as it was before.   
To set an installed tool as required in a build configuration, reference it via the `%teamcity.tool.<installed_tool_ID>%` property somewhere inside the build configuration settings or [parameters](configuring-build-parameters.md) (in any field that supports the `%parameter%` format).   
When you install a tool on the server, build agents restart and pick up the ID of this tool without downloading it. An agent will download the tool only when starting the first build that requests it. This ensures that agents download only required and compatible tools.   
Once downloaded, the tools are stored on an agent so builds don't spend time on downloading them again.

To check that the tool appears on the agent, look for `teamcity.tool.<installed_tool_ID>` in [configuration parameters reported by the agent](predefined-build-parameters.md#Agent+Properties) in the TeamCity web UI.

If you delete a tool in the TeamCity web UI, each agent that have this tool installed will detect this, delete own copy of the tool, and restart.