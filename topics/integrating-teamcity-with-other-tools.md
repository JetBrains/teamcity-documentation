[//]: # (title: Integrating TeamCity with Other Tools)
[//]: # (auxiliary-id: Integrating TeamCity with Other Tools)

TeamCity strives to support all the modern software technologies and platforms. We want to ensure our users are able to integrate every component of their CI/CD infrastructure with TeamCity. We do this either by providing smart detection and straightforward UI options on the TeamCity side or by exposing specialized [REST API](https://www.jetbrains.com/help/teamcity/rest/teamcity-rest-api-documentation.html) endpoints for easier scripting and integration on a third-party system's side.

There are many places in the TeamCity UI where you can set up or adjust software integrations, depending on their context. [Here](supported-platforms-and-environments.md) you can find a list of platforms and environments (and their versions) currently supported in TeamCity.

This article gives an overview of third-party software and platforms supported in TeamCity out of the box. Remember that you can always extend this scope by [installing additional plugins](installing-additional-plugins.md) or even [writing your own](https://plugins.jetbrains.com/docs/teamcity/developing-teamcity-plugins.html).

The tables below are updated in accordance with the newly introduced integrations and whenever we have extra guides to share.

## Integration with Operating Systems

<table>
<tr><td>Software</td><td>Available Integrations</td><td>Extra Guides and Tutorials</td></tr>

<tr><td>

**Windows**

</td><td>

* [Install TeamCity Server on a Windows machine](install-teamcity-server-on-windows.md)
* [Install TeamCity Agent on a Windows machine](install-teamcity-agent.md#Install+from+Windows+Executable+File) and run builds on Windows

</td><td></td></tr>

<tr><td>

**Linux**

</td><td>

* [Install TeamCity Server on a Linux machine](install-teamcity-server-on-linux-or-macos.md)
* [Install TeamCity Agent on a Linux machine](install-teamcity-agent.md) and run builds on Linux

</td><td></td></tr>

<tr><td>

**macOS**

</td><td>

* [Install TeamCity Server on a macOS machine](install-teamcity-server-on-linux-or-macos.md)
* [Install TeamCity Agent on a macOS machine](install-teamcity-agent.md) and run builds on macOS

</td><td></td></tr>

</table>

## Integration with Software Development Platforms and Build Tools

<table>
<tr><td>Software</td><td>Available Integrations</td><td>Extra Guides and Tutorials</td></tr>

<tr><td>

**Java**, including
* **Maven**
* **Gradle**
* **Ant**

</td><td>

* [Reporting Java code inspection results for a build](inspections.md)
* [Detecting Java code duplicates in a build's source](duplicates-finder-java.md)
* [Detailed test reports for Java frameworks on the fly](java-testing-frameworks-support.md)
* [Detailed code coverage](configuring-java-code-coverage.md)
* [Using Apache Maven for build automation](maven.md)
* [Special build triggers for Maven](configuring-maven-triggers.md)
* [Running builds on Gradle source projects](gradle.md)
* [Running Ant's `build.xml`](ant.md) to execute various build tasks, with support for JUnit and TestNG
* [Running a Java build inside a Docker container](docker-wrapper.md)

</td><td>

* [Building and Testing Java with Maven in TeamCity (2021)](https://www.jetbrains.com/teamcity/tutorials/maven-build-configure-test/)
* [Incremental Building with Maven and TeamCity (2012)](https://blog.jetbrains.com/teamcity/2012/03/incremental-building-with-maven-and-teamcity/)
* [Building and Testing Java with Gradle in TeamCity (2021)](https://www.jetbrains.com/teamcity/tutorials/gradle-build-configure-test/)
* [Configuring Artifact Dependencies Using Ant Build Script](artifact-dependencies.md#Configuring+Artifact+Dependencies+Using+Ant+Build+Script)
* [Get TeamCity Artifacts Using HTTP, Ant, Gradle and Maven (2012)](https://blog.jetbrains.com/teamcity/2012/06/teamcity-ivy-gradle-maven/)

</td></tr>

<tr><td>

**.NET**, including
* **MSBuild**
* **NAnt**
* **Visual Studio Solutions**
* **FxCop**
* **C# Script**
* **C#**
* **VB.NET**
* **NuGet**

</td><td>

* [Autodiscovery of .NET build steps in a source project](configuring-build-steps.md#Autodetecting+Build+Steps)
* [Autodetection of a .NET installation on a build agent](net.md#.NET+Version+Detection+Algorithm)
* Cross-platform .NET builds
* [Running .NET CLI commands](net.md#Build+Runner+Options)
* [Running MSBuild commands](net.md#msbuild)
* [Running vstest commands](net.md#vstest)
* [Running devenv](net.md#devenv-build-action)
* [Running any custom .NET commands](net.md#Custom+Commands)
* [Running a .NET build inside a Docker container](net.md#Docker)
* [Detailed and structured test reports for Java frameworks on the fly](net-testing-frameworks-support.md)
* [Detailed code coverage](configuring-.net-code-coverage.md)
* [Reporting .NET code inspection results for a build](inspections-resharper.md)
* [Detecting .NET code duplicates in a build's source](duplicates-finder-resharper.md)
* [Uploading to and downloading packages from NuGet feeds](nuget.md):
  * Support for public, private NuGet feeds, and an ability to [use TeamCity as a private NuGet feed](using-teamcity-as-nuget-feed.md)
      {product="tc"}
  * Automatically [installing](nuget-installer.md), [packing](nuget-pack.md), and [publishing packages](nuget-publish.md)
  * [Triggering builds on updates in a NuGet feed](nuget-dependency-trigger.md)
* [Using FxCop for inspecting .NET assemblies](fxcop.md)
* [Using NAnt to run build scripts](nant.md)
* [Using C# Scripting to run build scripts](c-script.md)

</td><td>

* [Building .NET Projects Using TeamCity (2021)](https://www.jetbrains.com/teamcity/tutorials/dotnet-build-configure-test/)
* [TeamCity integration with .NET, Part 1: New approach and demo (2020)](https://blog.jetbrains.com/teamcity/2020/12/teamcity-integration-with-net-part-1-new-approach-and-demo/)
* [TeamCity integration with .NET, Part 2: Testing and building projects (2020)](https://blog.jetbrains.com/teamcity/2020/12/teamcity-integration-with-net-part-2-testing-and-building-projects/)
* [TeamCity integration with .NET, Part 3: Deploying projects (2020)](https://blog.jetbrains.com/teamcity/2020/12/teamcity-integration-with-net-part-3-deploying-projects/)
* [How to automate CI/CD tasks with C# Scripting in TeamCity (2021)](https://blog.jetbrains.com/teamcity/2021/11/how-to-automate-ci-cd-tasks-with-c-scripting-in-teamcity/)

</td></tr>

<tr><td>

**Command Line**

</td><td>

* Performing any command-line actions across platforms with the simple [Command Line](command-line.md) runner

</td><td>

* [Video tutorial: How to run command line scripts (2022)](https://www.youtube.com/watch?v=oKNdLRrO3mA)

</td></tr>

<tr><td>

**PowerShell**

</td><td>

* [Running PowerShell scripts across platforms](powershell.md) for various build tasks

</td><td>

</td></tr>

<tr><td>

**Python**

</td><td>

* [Autodiscovery of Python build steps in a source project](configuring-build-steps.md#Autodetecting+Build+Steps)
* [Cross-platform execution of Python scripts](python.md)
* Support for Pipenv, Poetry, Venv, and Virtualenv
* [Running a Python build inside a Docker container](docker-wrapper.md)

</td><td>

* [Building and Testing Python Code with TeamCity (2021)](https://www.jetbrains.com/teamcity/tutorials/python-build-configure-test/)
* [Video tutorial: New in TeamCity 2020.2: Python Build Runner](https://www.youtube.com/watch?v=DOLY1K5e8hQ)

</td></tr>

<tr><td>

**Kotlin**

</td><td>

* [Cross-platform execution of Kotlin scripts](kotlin-script.md)
* [Using Kotlin DSL for configuring TeamCity project and build settings as code](kotlin-dsl.md)

</td><td>

* [Kotlin DSL for Beginners: Recommended Refactorings (2021)](https://blog.jetbrains.com/teamcity/2021/04/kotlin-dsl-for-beginners-recommended-refactorings/)
* [Creating TeamCity project templates with Kotlin DSL context parameters (2020)](https://blog.jetbrains.com/teamcity/2020/09/creating-teamcity-project-templates-with-kotlin-dsl-context-parameters/)
* [Configuration as Code, Part 1: Getting Started with Kotlin DSL (2019)](https://blog.jetbrains.com/teamcity/2019/03/configuration-as-code-part-1-getting-started-with-kotlin-dsl/)
* [Configuration as Code, Part 2: Working with Kotlin Scripts (2019)](https://blog.jetbrains.com/teamcity/2019/03/configuration-as-code-part-2-working-with-kotlin-scripts/)
* [Configuration as Code, Part 3: Creating Build Configurations Dynamically (2019)](https://blog.jetbrains.com/teamcity/2019/04/configuration-as-code-part-3-creating-build-configurations-dynamically/)
* [Configuration as Code, Part 4: Extending the TeamCity DSL (2019)](https://blog.jetbrains.com/teamcity/2019/04/configuration-as-code-part-4-extending-the-teamcity-dsl/)
* [Configuration as Code, Part 5: Using DSL extensions as a library (2019)](https://blog.jetbrains.com/teamcity/2019/04/configuration-as-code-part-5-using-dsl-extensions-as-a-library/)
* [Configuration as Code, Part 6: Testing Configuration Scripts (2019)](https://blog.jetbrains.com/teamcity/2019/05/configuration-as-code-part-6-testing-configuration-scripts/)

</td></tr>

<tr><td>

**Node.js**

</td><td>

* [Autodiscovery of JavaScript build steps in a source project](nodejs.md#Autodetecting+JavaScript+Steps)
* [Running npm, yarn, and node commands within a build](nodejs.md)
* [Running a Node.js script inside a Docker container](docker-wrapper.md)
* Support for ESlint, Jest, and Mocha
* [Accessing private NPM registries](nodejs.md#Accessing+Private+NPM+Registries)

</td><td>

* [Video tutorial: New in TeamCity 2021.1: Node.js Runner](https://www.youtube.com/watch?v=8-m5GFrxUzw)

</td></tr>

<tr><td>

**Rake**

</td><td>

* [Running an Rakefile within a build](rake.md)
* Support for Test::Unit, Test-Spec, Shoulda, RSpec, and Cucumber

</td><td>


</td></tr>

<tr><td>

**SBT (Scala)**

</td><td>

* [Using Simple Build Tool's (Scala) commands to build and test sources](simple-build-tool-scala.md)

</td><td>

</td></tr>

<tr><td>

**Xcode**

</td><td>

* [Running an Xcode scheme](xcode-project.md.md)
* A structured build log output based on Xcode build stages, compilation errors, and test reports
* Automatically added [agent requirements](agent-requirements.md) to a specific version of Xcode or IDE

</td><td>
</td></tr>

</table>

## Integration with Testing Frameworks

<table>
<tr><td>Software</td><td>Available Integrations</td><td>Extra Guides and Tutorials</td></tr>

<tr><td>

**JUnit**

</td><td>

* Detailed on-the-fly test reporting for the [supported Java testing frameworks](java-testing-frameworks-support.md)

</td><td></td></tr>

<tr><td>

**TestNG**

</td><td>

* Detailed on-the-fly test reporting for the [supported Java testing frameworks](java-testing-frameworks-support.md)

</td><td></td></tr>

<tr><td>

**NUnit**

</td><td>

* [Running NUnit tests](nunit.md) and providing detailed test results
* Displaying code coverage with [JetBrains dotCover](jetbrains-dotcover.md)

</td><td></td></tr>

<tr><td>

**MSTest / VSTest**

</td><td>

* [Testing a project with the test engine and automatically importing test results](net.md#vstest)

</td><td></td></tr>

<tr><td>

**MSpec**

</td><td>

* [Running MSpec tests and code coverage](mspec.md)

</td><td></td></tr></table>

## Integration with Version Control Systems

<table>
<tr><td>Software</td><td>Available Integrations</td><td>Extra Guides and Tutorials</td></tr>

<tr><td>

**Git**

</td><td>

</td><td></td></tr>

<tr><td>

**Subversion**

</td><td>

</td><td></td></tr>

<tr><td>

**Perforce**

</td><td>

</td><td></td></tr>

<tr><td>

**TFVC**

</td><td>

</td><td></td></tr>

<tr><td>

**CVS**

</td><td>

</td><td></td></tr>

<tr><td>

**Mercurial**

</td><td>

</td><td></td></tr>

<tr><td>

**StarTeam**

</td><td>

</td><td></td></tr></table>

## Integration with VCS Hosting Services

<table>
<tr><td>Software</td><td>Available Integrations</td><td>Extra Guides and Tutorials</td></tr>

<tr><td>

**GitHub.com / GitHub Enterprise**

</td><td>

</td><td></td></tr>

<tr><td>

**GitLab.com / GitLab CE/EE**

</td><td>

</td><td></td></tr>

<tr><td>

**Bitbucket Cloud / Bitbucket Server**

</td><td>

</td><td></td></tr>

<tr><td>

**Azure DevOps Services**

</td><td>

</td><td></td></tr>

<tr><td>

**JetBrains Space**

</td><td>

</td><td></td></tr></table>

## Integration with Cloud Hosting and Orchestration Solutions

<table>
<tr><td>Software</td><td>Available Integrations</td><td>Extra Guides and Tutorials</td></tr>

<tr><td>

**Amazon EC2**

</td><td>

</td><td></td></tr>

<tr><td>

**VMWare vSphere**

</td><td>

</td><td></td></tr>

<tr><td>

**Kubernetes**

</td><td>

</td><td></td></tr></table>

## Integration with Issue Trackers

<table>
<tr><td>Software</td><td>Available Integrations</td><td>Extra Guides and Tutorials</td></tr>

<tr><td>

**JetBrains YouTrack**

</td><td>

</td><td></td></tr>

<tr><td>

**Atlassian Jira**

</td><td>

</td><td></td></tr>

<tr><td>

**Bugzilla**

</td><td>

</td><td></td></tr></table>

## Integration with IDEs

<table>
<tr><td>Software</td><td>Available Integrations</td><td>Extra Guides and Tutorials</td></tr>

<tr><td>

**IntelliJ IDEA Platform**

</td><td>

</td><td></td></tr>

<tr><td>

**Microsoft Visual Studio**

</td><td>

</td><td></td></tr>

<tr><td>

**Eclipse**

</td><td>

</td><td></td></tr></table>

## Integration with Databases

<table>
<tr><td>Software</td><td>Available Integrations</td><td>Extra Guides and Tutorials</td></tr>

<tr><td>

**MySQL**

</td><td>

</td><td></td></tr>

<tr><td>

**Microsoft SQL Server**

</td><td>

</td><td></td></tr>

<tr><td>

**PostgreSQL**

</td><td>

</td><td></td></tr>

<tr><td>

**Oracle**

</td><td>

</td><td></td></tr>

</table>

## Integration with Notification Services

<table>
<tr><td>Software</td><td>Available Integrations</td><td>Extra Guides and Tutorials</td></tr>

<tr><td>

**Slack**

</td><td>

</td><td></td></tr>

<tr><td>

**Browser Notifications**

</td><td>

</td><td></td></tr>

</table>


