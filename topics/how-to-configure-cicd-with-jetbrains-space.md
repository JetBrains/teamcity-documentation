[//]: # (title: How to Configure CI/CD with JetBrains Space)
[//]: # (auxiliary-id: How to Configure CI/CD with JetBrains Space)

**[JetBrains Space](https://www.jetbrains.com/space/) is a full-cycle collaboration solution for software development teams. This guide explains how to achieve continuous integration and delivery of JetBrains Space projects by integrating them with TeamCity.**

<table><tr><td></td></tr><tr><td>

Integration with TeamCity brings the following advantages to the JetBrains Space users:
* Compiling, testing, and deploying projects within the same environment.
* Building source code of merge requests and merging them automatically after a successful build.
* Extensive build overview: diffs and artifacts, detailed test reports on-the-fly, code coverage, inspections, and various other metrics. Statuses of builds and code reviews are cross-shared between systems for easier monitoring.
* Flexible pipelines where builds depend on one another and share settings and results.
* Ability to configure builds as code, in [Kotlin DSL](kotlin-dsl.md).
* Authentication with a single account in both systems: VCS (JetBrains Space) and CI/CD (TeamCity).

</td></tr><tr><td></td></tr></table>

![pending.png](pending.png) _The [preliminary setup](#Preliminary+Setup) usually takes up to 10 minutes to complete, granting all the steps are performed with no delay. After it is completed, enabling the components of this integration gets quite straightforward and can be done at any pace._

## Prerequisites

To perform all the steps described in this guide, you need to have:
* Administrative access to either the [TeamCity Cloud instance](https://www.jetbrains.com/teamcity/signup/) or [TeamCity On-Premises server](https://www.jetbrains.com/teamcity/download/) of your organization.
* Administrative access to the JetBrains Space instance of your organization.

If you only want to integrate JetBrains Space repositories with TeamCity projects, having project administration rights is enough.

## Preliminary Setup

Connecting TeamCity to JetBrains Space involves two steps: (1) creating a service application for TeamCity authentication in your Space instance and (2) creating a preset of connection to Space.

### Step 1: Create Application in JetBrains Space

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
      <anchor name="redirect-uri"/>
   3. To be able to use authentication via Space in TeamCity or/and to create projects/configurations from Space repositories, enable _Authorization Code Flow_ as well. Enter your TeamCity server's URL (`https://<server>:<port>/oauth/space/accessToken.html`) as the redirect URI.  
      To ensure that your TeamCity server can always connect to JetBrains Space, it is important to specify all the other possible endpoint addresses of the server. In most cases, it would be enough to specify the _Server URL_ set in __Global Settings__ in TeamCity. However, if you use a [proxy](configuring-proxy-server.md) for your TeamCity server but access this server directly, the authentication might not work unless the server's IP address is also specified here.
      {product="tc"}
   4. Copy the app's _Client ID_ and _Client secret_. You will need them for configuration on the TeamCity side.

Now you can return to TeamCity and add a connection to JetBrains Space.

### Step 2: Establish Connection to JetBrains Space

>TeamCity allows you to configure all settings of your _connection_ to a service in one place and then reuse these settings in different projects and build configurations. If you add such a _connection_ on the <emphasis tooltip="root-project">Root project</emphasis> level, this will allow using its settings to any other project on the server. To make a connection available only in a certain project, you need to add it in this project.  
>You can configure as many connections as you want.

To create a connection to your JetBrains Space instance:
1. Go to __Project Settings | Connections__ and click __Add Сonnection__.
2. Choose the _JetBrains Space_ connection type.  
   <img src="connection-to-space.png" width="460" alt="Create a connection to Space"/>
4. Enter the settings as follows:
   * _Space URL_: the URL of your Space instance (`<company_name>.jetbrains.space`).
   * _Client ID_: the client ID value copied from the Space app's __Authentication__ tab.
   * _Client secret_: the client secret value copied from the Space app's __Authentication__ tab.
5. Save the connection.

At this stage, you are free to connect the current project or any of its subprojects to your JetBrains Space instance whenever necessary.

## Connecting TeamCity Projects with JetBrains Space Projects

There are three ways to integrate a JetBrains Space repository with TeamCity:
* Create a TeamCity _project_ based on a repository.
* Create a _build configuration_ based on a repository in an existing TeamCity project.
* Create a <emphasis tooltip="vcs-root">VCS root</emphasis> based on a repository in an existing TeamCity project.

We will describe the first approach as it's the most popular and self-sufficient. However, you can always add one more Space [root](configuring-vcs-roots.md) or [build configuration](creating-and-editing-build-configurations.md#Creating+Build+Configuration+from+URL) to an existing project — tne procedure is similar.

Let's create a nested project of the project where you've added the JetBrains Space connection. If it is the <emphasis tooltip="root-project">Root project</emphasis>, go to __Administration | Projects__ and click __Create project__ there.

You will notice the new button: __From JetBrains Space__. Its name depends on the _Display name_ you gave to the connection, but you can always distinguish Space connections from others by the Space logo. To create a new project:

1. Click __From JetBrains Space__.
2. As it is the first time you connect this server to your Space instance, you have to authenticate in Space via your user profile. Click __Sign in to Space__ and accept the access request. Next time, you won't have to confirm it again, unless you sign out or change your password.  
  If you get the _OAuth 2.0 Error_, this probably means that the _Redirect URI_ has not been configured properly in Step 1 of the preliminary setup. Make sure to [revise it](#redirect-uri). Note that Space supports only HTTPS connection.  
   <img src="create-project-from-space.png" width="706" alt="Create a project from a Space repository"/>
3. The project creation wizard will display a list of all Space projects your user has access to. Choose a repository and wait until TeamCity verifies the connection settings.
4. Now it's time to configure the main settings of the new project and its [VCS root](vcs-root.md). You can always adjust them later.
   * _Project name_ and the name of its first [build configuration](build-configuration.md).
   * _VCS root_: read-only — it matches the URL of a repository you choose in Step 3.
   * _Default branch_: `refs/heads/main`, or another branch of your choice. Remember to keep `refs/heads` and change only the branch name.
   * _Branch specification_: if you want to run builds on other branches apart from the default one, this setting allows extending the scope of the VCS root and monitor these branches as well. TeamCity uses a [special format or branch specification rules](working-with-feature-branches.md). To monitor the whole repository, use the `refs/heads/*` rule.
     <img src="space-project-basic-settings.png" width="706" alt="Configure project settings"/>
   >Our test project also contains project settings specified in a TeamCity [Kotlin DSL](kotlin-dsl.md) format. If TeamCity detects those in your repository, it can automatically apply all the specifications.
5. Click __Proceed__.

TeamCity will attempt to [autodetect build steps](configuring-build-steps.md#Autodetecting+build+steps) in your project. You are free to confirm or reject the proposed steps and explore the project settings further.

To be able to manually run builds on the Space repository sources, you need to [configure steps](configuring-build-steps.md) of the newly created build configuration. The simplest scenario is to add a [Command Line](command-line.md) step and define the CLI command to build your sources. To initiate a build based on the freshest sources of the default branch, click __Run__ in the upper right corner. TeamCity will launch a one-step build with these CLI commands.  
To monitor changes in the Space repository and automatically start builds on new commits, configure a [VCS trigger](configuring-vcs-triggers.md) in this build configuration.

After this basic setup, you can advance the Space integration by following the instructions below, or learn how to [create more sophisticated build configurations](configuring-general-settings.md) and utilize the power of TeamCity to the fullest.

## Building Sources of Merge Requests
{product="tcc"}

A build's checkout scope usually consists of the following items: the default branch of [VCS root(s)](vcs-root.md) + the project's [branch specification](working-with-feature-branches.md) + [checkout rules](vcs-checkout-rules.md) of the build configuration. By adding the [Pull Request](pull-requests.md) build feature to this build configuration, you can add one more item to this formula — branches of merge requests. This will allow TeamCity to monitor changes in merge requests and run builds on them. The most common use case is prebuilding sources of feature-branches before they are merged into the default branch.



## Reporting Build Statuses to JetBrains Space

## Displaying JetBrains Space Code Review Status in TeamCity

## Authenticating in TeamCity with JetBrains Space Account

Users of your JetBrains Space instance can authenticate in TeamCity with their Space accounts.

This functionality requires that the [JetBrains Space connection](#Step+2%3A+Establish+Connection+to+JetBrains+Space) is added on the Root project level.

To configure it:
1. Go to __Administration | Authentication__.
2. Click __Add module__ and choose the _JetBrains Space_ type.  
   <img src="add-space-auth-module.png" width="460" alt="Add a JetBrains Space authentication module"/>
3. Choose if you want to allow creating new users on the first login, if their email is not recognized by TeamCity. Disabling this option could be helpful if you use a publicly available TeamCity server and want to limit access to it.
4. Save the module.

To sign in to TeamCity, click the JetBrains Space icon above the TeamCity login form and, after the redirect, approve the TeamCity application.

<img src="spaceauth.png" width="296" alt="Sign in to TeamCity with JetBrains Space account"/>