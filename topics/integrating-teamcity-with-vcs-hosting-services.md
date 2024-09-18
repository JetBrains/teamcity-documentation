[//]: # (title: Integrating TeamCity with VCS Hosting Services)
[//]: # (auxiliary-id: Integrating TeamCity with VCS Hosting Services)

You can create presets of connections to the following hosting services:
* [GitHub.com](https://github.com/) and [GitHub Enterprise](https://enterprise.github.com/)
* [GitLab.com](https://about.gitlab.com/) and [GitLab CE/EE](https://about.gitlab.com/install/ce-or-ee/)
* [Bitbucket Cloud](https://bitbucket.org/)
* [Bitbucket Server and Data Center](https://www.atlassian.com/software/bitbucket/enterprise)
* [Azure DevOps](https://azure.microsoft.com/services/devops/)
* [JetBrains Space](https://www.jetbrains.com/space/)

Once created, such a connection can serve as a base for different operations: creating projects from URL, creating VCS roots, integrating with issue trackers, and authenticating users in TeamCity using their external profiles.

>In TeamCity Cloud, connections to GitHub.com, GitLab.com, and Bitbucket Cloud are already predefined in the Root project's settings, which makes them available in all the other projects.
>
{product="tcc"}

Connections are configured on the __Project Administration | Connections__ page. Each connection is accessible in the current project and all its subprojects. If you use global VCS hosting services for your whole organization, it makes sense to configure a single connection to such a service on the Root project level. This way, the users will be able to quickly access the list of the organization's repositories when creating subprojects or editing projects' settings.

<img src="projectConnections.png" width="700" alt="Adding a GitHub connection"/>

<anchor name="Connecting+to+GitHub"/>

## Integrating TeamCity with GitHub

Integration with GitHub allows you to:
* create a [project from GitHub URL](creating-and-editing-projects.md#Creating+project+pointing+to+repository+URL)
* create a [VCS root from URL](guess-settings-from-repository-url.md)
* create a [Git VCS root](git.md)
* integrate with a [GitHub issue tracker](github.md)
* enable [GitHub authentication](configuring-authentication-settings.md#GitHub)

See how to configure a connection to GitHub.com or GitHub Enterprise [here](configuring-connections.md#GitHub).

<video src="https://youtu.be/hbh7sk5sGTc"
title="TeamCity tutorial — How to integrate TeamCity and GitHub"/>

## Integrating TeamCity with GitLab

Integration with GitLab allows you to:
* create a [project from GitLab URL](creating-and-editing-projects.md#Creating+project+pointing+to+repository+URL)
* create a [VCS root from URL](guess-settings-from-repository-url.md)
* integrate with a [GitLab issue tracker](gitlab.md)
* enable [GitLab authentication](configuring-authentication-settings.md#GitLab.com)

See how to configure a connection to GitLab.com or GitLab CE/EE [here](configuring-connections.md#GitLab).

## Integrating TeamCity with Bitbucket

Integration with hosted (Cloud) and self-hosted (Server and Data Center) Bitbucket products allows you to:

* create a [project and build configuration from Bitbucket URL](creating-and-editing-projects.md#Creating+project+pointing+to+repository+URL)
* create a [VCS root from URL](guess-settings-from-repository-url.md)

With Bitbucket Cloud you can also:

* create a [Mercurial VCS root](mercurial.md)
* integrate with a [Bitbucket Cloud issue tracker](bitbucket-cloud.md)
* enable [Bitbucket Cloud authentication](configuring-authentication-settings.md#Bitbucket+Cloud)

See the following documentation articles to learn how to configure connections to Bitbucket: [Bitbucket Cloud](configuring-connections.md#Bitbucket+Cloud) | [Bitbucket Server and Data Center](configuring-connections.md#Bitbucket+Server+and+Data+Center).

## Integrating TeamCity with Azure DevOps

Integration with Azure DevOps Services allows you to:
* create a [project from a Git repository URL](creating-and-editing-projects.md#Creating+project+pointing+to+repository+URL)
* create a [VCS root from URL](guess-settings-from-repository-url.md)
* enable [user authentication via Azure DevOps](configuring-authentication-settings.md#Azure+DevOps+Services)

See how to configure a connection to Azure DevOps [here](configuring-connections.md#Azure+DevOps).

## Integrating TeamCity with JetBrains Space

Integration with JetBrains Space allows you to:
* create a [project from a Git Space URL](creating-and-editing-projects.md#Creating+project+pointing+to+repository+URL)
* create a [VCS root from URL](guess-settings-from-repository-url.md)
* enable [JetBrains Space authentication](configuring-authentication-settings.md#JetBrains+Space)

See how to configure a connection to JetBrains Space [here](configuring-connections.md#connect-to-jetbrains-space).
