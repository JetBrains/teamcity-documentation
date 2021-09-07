[//]: # (title: Integrating TeamCity with VCS Hosting Services)
[//]: # (auxiliary-id: Integrating TeamCity with VCS Hosting Services)

You can create presets of connections to the following hosting services:
* [GitHub.com](https://github.com/) and [GitHub Enterprise](https://enterprise.github.com/)
* [Bitbucket Cloud](https://bitbucket.org/)
* [GitLab.com](https://about.gitlab.com/) and [GitLab CE/EE](https://about.gitlab.com/install/ce-or-ee/)
* [Azure DevOps](https://azure.microsoft.com/ru-ru/services/devops/)

Once created, such a connection can serve as a base for different operations: creating projects from URL, creating VCS roots, integrating with issue trackers, and authenticating users in TeamCity using their external profiles.

>In TeamCity Cloud, connections to GitHub.com, GitLab.com, and Bitbucket Cloud are already predefined in the Root project's settings, which makes them available in all the other projects.
>
{product="tcc"}

Connections are configured on the __Project Administration | Connections__ page. Each connection is accessible in the current project and all its subprojects. If you use global VCS hosting services for your whole organization, it makes sense to configure a single connection to such a service on the Root project level. This way, the users will be able to quickly access the list of the organization's repositories when creating subprojects or editing projects' settings.

<img src="projectConnections.png" width="700" alt="Adding a GitHub connection"/>

## Connecting to GitHub

<include src="configuring-connections.md" include-id="github"/>

## Connecting to Bitbucket Cloud

<include src="configuring-connections.md" include-id="bb-cloud"/>

## Connecting to GitLab

<include src="configuring-connections.md" include-id="gitlab"/>

## Connecting to Azure DevOps Services

<include src="configuring-connections.md" include-id="azure-devops"/>