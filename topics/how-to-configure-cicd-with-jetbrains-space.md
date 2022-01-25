[//]: # (title: How to Configure CI/CD with JetBrains Space)
[//]: # (auxiliary-id: How to Configure CI/CD with JetBrains Space)

[JetBrains Space](https://www.jetbrains.com/space/) is a full-cycle collaboration solution for software development teams. This guide explains how to achieve continuous integration and delivery of JetBrains Space projects by integrating them with TeamCity.

<table><tr><td></td></tr><tr><td>

**Integration with TeamCity brings the following advantages to the JetBrains Space users**:
* Compiling, testing, and deploying projects within the same environment.
* Building source code of pull requests and merging them automatically after a successful build.
* Extensive build overview: diffs and artifacts, detailed test reports on-the-fly, code coverage, inspections, and various other metrics. Statuses of builds and code reviews are cross-shared between systems for easier monitoring.
* Flexible pipelines where builds depend on one another and share settings and results.
* Ability to configure builds as code, in [Kotlin DSL](kotlin-dsl.md).
* Authentication with a single account in both systems: VCS (JetBrains Space) and CI/CD (TeamCity).

</td></tr><tr><td></td></tr></table>

## Prerequisites

To perform all the steps described in this guide, you need to have:
* Administrative access to either the [TeamCity Cloud instance](https://www.jetbrains.com/teamcity/signup/) or [TeamCity On-Premises server](https://www.jetbrains.com/teamcity/download/) of your organization.
* Administrative access to the JetBrains Space instance of your organization.

If you only want to integrate JetBrains Space repositories with TeamCity projects, having project administration rights is enough.

## Building Sources of JetBrains Space Repository

There are three ways to integrate a JetBrains Space repository with TeamCity:
* Create a TeamCity _project_ based on a repository.
* Create a _build configuration_ based on a repository in an existing TeamCity project.
* Create a <emphasis tooltip="vcs-root">VCS root</emphasis> based on a repository in an existing TeamCity project.

We will describe the first approach as it's the most popular and self-sufficient. However, you can always add one more Space [root](configuring-vcs-roots.md) or [build configuration](creating-and-editing-build-configurations.md#Creating+Build+Configuration+from+URL) to an existing project — tne procedure is similar.

This approach involves the following steps: (1) creating a service application for TeamCity authentication in your Space instance, (2) creating a preset of connection to Space, and (3) creating a TeamCity project from a Space repository URL.

### Step 1. Create Application in JetBrains Space

In your JetBrains Space instance:
1. Go to __Administration | Applications__ and click __New application__.
2. Enter a convenient name (for example, `Space-to-TeamCity`), save the application, and click __Go to application settings__.
3. __Configure In-context Authorization__.
   1. On the __Authorization__ tab, click __Authorize in new context__ under _In-context Authorization_.
   2. Enter the name of the Space project you are about to access from TeamCity and click __Authorize__.  
   >When you create a project in JetBrains Space, it does not automatically add you to this project as a member — this needs to be done manually. TeamCity will be able to see only those projects where you (or the user who created the application) are listed as a member.
   3. (_Optional_) If you want TeamCity to be able to publish commit statuses to Space, you will need to add a respective permission.  
     Click __Configure__ and enable _Git Repositories | Report external check status_. If you are the selected project's administrator, you can approve this permission right in this __Authorization__ tab. Otherwise, wait until it is granted by the project administrator.
4. __Configure Global Authorization__.
   1. On the __Authorization__ tab, click __Configure__ under _Global Authorization_.
   2. To establish general access from TeamCity to Space, enable the _Members | View member profile_ permission and click __Save__. This request has to be accepted by the server administrator.
5. __Configure Authentication Mode__.
   1. Go back to the app's __Overview__ and open the __Authentication__ tab.
   2. Enable _Client Credentials Flow_.
   3. To be able to use authentication via Space in TeamCity or/and to create projects/configurations from Space repositories, enable _Authorization Code Flow_ as well. Enter your TeamCity server's URL as the redirect URI.  
      To ensure that your TeamCity server can always connect to JetBrains Space, it is important to specify all the other possible endpoint addresses of the server. In most cases, it would be enough to specify the _Server URL_ set in __Global Settings__ in TeamCity. However, if you use a [proxy](configuring-proxy-server.md) for your TeamCity server but access this server directly, the authentication might not work unless the server's IP address is also specified here.
   {product="tc"}
   4. Copy the app's _Client ID_ and _Client secret_. You will need them for configuration on the TeamCity side.

Now you can return to TeamCity and add a connection to JetBrains Space.

### Step 2. Establish Connection to JetBrains Space

>TeamCity allows you to configure all settings of your _connection_ to a service in one place and then reuse these settings in different projects and build configurations. If you add such a _connection_ on the <emphasis tooltip="root-project">Root project</emphasis> level, this will allow using its settings to any other project on the server. To make a connection available only in a certain project, you need to add it in this project.  
>You can configure as many connections as you want.

To create a connection to your JetBrains Space instance:
1. Go to __Project Settings | Connections__ and click __Add Сonnection__.
2. Choose the _JetBrains Space_ connection type.
3. Enter the settings as follows:
   * _Space URL_: the URL of your Space instance (`<company_name>.jetbrains.space`).
   * _Client ID_: the client ID value copied from the Space app's __Authentication__ tab.
   * _Client secret_: the client secret value copied from the Space app's __Authentication__ tab.
4. Save the connection.

At this stage, you are free to connect the current project or any of its subprojects to your JetBrains Space instance whenever necessary.

### Step 3. Create Project from JetBrains Space Repository

Let's create a nested project of the project from Step 2. If you have added the connection in the <emphasis tooltip="root-project">Root project</emphasis>, go to __Administration | Projects__ and click __Create project__ there. You will notice that there is now the __From JetBrains Space__ button.

## Building Sources on JetBrains Space Pull Requests

## Reporting Build Statuses to JetBrains Space

## Displaying JetBrains Space Code Review Status in TeamCity

## Authenticating in TeamCity with JetBrains Space Account

