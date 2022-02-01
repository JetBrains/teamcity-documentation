[//]: # (title: Build Runner)
[//]: # (auxiliary-id: Build Runner)

_Build runner_ is a part of TeamCity that allows integration with a specific build tool (Ant, MSBuild, Command Line, and so on). In a build configuration, a build runner defines how to run a build and report its results.

Technically, build runners are implemented as plugins. Each runner has two parts:
* the server-side settings that are configured through the web UI
* the agent-side part that executes a build on an agent

Instructions on configuring bundled runners are listed under the [Configuring Build Steps](configuring-build-steps.md) section.

Build runners are configurable in the __[Build Steps](configuring-build-steps.md)__ section of the __[Create/Edit Build Configuration](creating-and-editing-build-configurations.md)__ page.

<video href="wLmLgh5OK5o"
title="TeamCity - How to use specific runners to supercharge your builds"/>

 <seealso>
        <category ref="admin-guide">
            <a href="configuring-build-steps.md">Configuring Build Steps</a>
        </category>
</seealso>