[//]: # (title: What's New in TeamCity On-Premises 2024.11)

<snippet id="2024-11-tc">

## Upload Custom Kotlin Libraries
{instance="tc"}

Starting with this version, you can upload custom Kotlin libraries as `.jar` to you TeamCity server.

<img src="custom-dsl-library-upload.png" width="706" alt="Upload custom Kotlin library"/>

To start using these libraries in your project's [.kts files](kotlin-dsl.md), add Maven dependencies to required `pom.xml`s.

Learn more: [](kotlin-dsl.md#Add+Custom+Kotlin+Libraries)


## Miscellaneous Changes
{instance="tc"}

* The [](artifacts-migration-tool.md) now supports migration to and from Microsoft Azure storages. This tool allows you to easily transfer build artifacts from one storage to another. Note that you need to install an unbundled plugin to set up Azure storages: [Azure Artifact Storage](https://plugins.jetbrains.com/plugin/9617-azure-artifact-storage).
* All messages written to `teamcity-server.log` during a server startup are now duplicated to the [teamcity-startup.log](teamcity-server-logs.md). This log ensures major boot events are logged to a separate file, which may assist in troubleshooting server startup issues.

</snippet>

