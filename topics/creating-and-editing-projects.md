[//]: # (title: Creating and Editing Projects)
[//]: # (help-id: Creating and Editing Projects;Project)

In TeamCity, actual building tasks are carried out by [build configurations](creating-and-editing-build-configurations.md) and [pipelines](create-and-edit-pipelines.md). However, both of them must be placed inside a project.

This topic illustrates different ways to create projects.


## Root Project and Settings Inheritance

Before you begin, note that every TeamCity server includes a built-in, undeletable project called the **Root project**. All new projects are created as its children, but it cannot host build configurations and pipelines directly.

In TeamCity, child projects inherit many settings and entities from their parent, such as [connections](configuring-connections.md) and [cloud agent profiles](teamcity-integration-with-cloud-solutions.md). The Root project lets you take advantage of this concept and define server-wide resources. For example, you can create [AWS cloud profile](setting-up-teamcity-for-amazon-ec2.md) that spawns cloud agents accessible to all projects on the server.

You can navigate to the Root project settings by clicking the **&lt;Root project&gt;** breadcrumbs item in [edit mode](project-administrator-guide.md#Edit+and+View+Modes)...

<img src="dk-navigate-to-root-project.png" width="706" alt="Navigate to Root project"/>

...or by going to the `<your_server_URL>/admin/editProject.html?projectId=_Root` URL directly. Note that since [user permissions](managing-roles-and-permissions.md) are project-based, only Root project administrators can edit its settings.


## Create New Projects in TeamCity UI

New TeamCity projects can be added using corresponding sidebar buttons. The **Create** button next to the **Projects** menu item allows you to add top-level projects owned directly by the [Root project](#Root+Project+and+Settings+Inheritance).

<img src="dk-crete-project-sidebar-1.png" alt="Create new project" width="706"/>

To add a subproject of an existing project, click the identical button next to that project.

<img src="dk-crete-project-sidebar-2.png" alt="Create new subproject" width="706"/>

> When a TeamCity user creates a project, TeamCity automatically adds it to the **Favorites** list for that user. This allows you to quickly locate your own projects. You can click a star icon next to project names to add or remove them from Favorites.
> 
{style="tip"}

For a brand-new TeamCity installations, only **From repository URL** and **Manually** project creation options are available. You can get more options after you configure connections to VCS hosting providers.

<img src="dk-default-add-project-options.png" width="706" alt="Default New Project Options"/>



### From Repository URL

This option allows you to create a new project and a child build configuration in one go using a Git, Subversion, Mercurial, TFS, or Perforce repository (depot) URL. You can use any URL type:

* A regular repository web link: `https://github.com/Johndoe/my-sample-app`
* An HTTPS clone URL: `https://github.com/Johndoe/my-sample-app.git`
* An SSH clone URL: `git@github.com:Johndoe/my-sample-app.git`

To start building a remote repository, follow the steps below.

<procedure type="steps">

<step>

On the new project page, click the **From a repository URL** tile.

</step>

<step>

Choose the authentication type.

   <deflist type="medium">
   
   <def title="Password / Access token">
   
   Enter a username and either a password or personal access token. For public repositories that do not require authentication to clone, you can leave both fields empty. If you later add features that need higher access (for example, `write` permissions for the [](commit-status-publisher.md) feature to display build statuses), you can configure authentication settings at that time.
   
   </def>
   
   <def title="SSH key">
   
   Available if the **Repository URL** is an SSH clone URL. Click **Upload SSH key** to add a private key, which will be saved in the parent project ([**parent project settings**](project-administrator-guide.md#Edit+and+View+Modes) **| SSH keys**) and appear in the drop-down menu when configuring additional projects.
   
   Learn more: [](ssh-keys-management.md)
   
   </def>
   
   </deflist>

</step>


<step>

The next page contains mixed settings of both project and a build configuration owned by this project.

   * **Parent project** — use this menu to change the project's parent.
   * **Project name** and **Build configuration name** — public names visible in TeamCity.
   * **Default branch** — the full name of a branch that will become a default one in TeamCity (for example, `refs/heads/main`). See the following article for more information: [](working-with-feature-branches.md#Default+Branch).
   * **Branch specification** — the set of rules that specify which repository branches TeamCity should track. The default `refs/heads/*` rule adds all repo branches to the watchlist. See the following topic to learn more: [](working-with-feature-branches.md).
   
   > The VCS root whose name is shown on this page stores most of the settings you entered: authentication settings, branch specification, and default branch. To edit them, navigate to [**project settings**](project-administrator-guide.md#Edit+and+View+Modes) **| VCS Roots**.
   > 
   > * [What are VCS Roots](project-administrator-guide.md#VCS+Roots)
   > * [](configuring-vcs-roots.md)
   > 
   {style="tip"}

</step>

<step>

Click **Proceed**. At this stage you have already created a project and its child build configuration, and now edit configuration settings. TeamCity opens the [Add build step](configuring-build-steps.md) so you could add actual functionality to your configuration and start building or testing a remote repository. Refer to this article for more information on build steps.

</step>

</procedure>



### Manually

This option allows you to create a completely blank project.

<procedure type="steps">

<step>

On the new project page, click the **Manually** tile.

</step>


<step>

Specify initial settings for your new TeamCity project:

* **Parent project** — use this menu to change the project's parent.
* **Name** — the public project name.
* **Project ID** — becomes a part of the project URL, and used to access this project in REST API calls, Kotlin DSL settings, and more. You can leave the default value which is assembled from parent project IDs and this project's truncated name.
* **Description** — the optional project description.

</step>


<step>

Click **Create**. You will end up with an empty project. Repeat the steps above to add subprojects to it, or start adding [build configurations](creating-and-editing-build-configurations.md) and [pipelines](create-and-edit-pipelines.md).

</step>


</procedure>


### From a Configured Connection

If a TeamCity project stores a configured [connection](configuring-connections.md) to a VCS provider, it can use this connection to quickly set up all authentication settings and retrieve a list of remote repositories. This is the most convenient way to create multiple projects targeting different repositories stored under the same hosting provider.

First, you will require a connection. In this article, we will add a connection to GitHub and use it to add a project targeting a GitHub-hosted repository. See the [](configuring-connections.md) article for more information on connections to other VCS providers.


<procedure type="steps" title="Add a connection">

<step>

Open [parent project settings](project-administrator-guide.md#Edit+and+View+Modes). If you want a future connection to be available for any project created on this server, modify [Root project](#Root+Project+and+Settings+Inheritance) settings. Otherwise, start with adding a [manually created blank project](#Manually).

</step>

<step>

Navigate to the **Connections** tab and click **Add Connection**.

<img src="dk-add-connection-tab.png" width="706" alt="Add new connection"/>

</step>


</procedure>










<!--

This section details creating projects via the TeamCity web UI. Other options include the [REST API](https://www.jetbrains.com/help/teamcity/rest/create-and-delete-projects.html) and using TeamCity project configuration in [DSL based on the Kotlin language](kotlin-dsl.md).

## Creating Project

To create a project, use the __Administration__ link in the upper right corner and click __Create project__. The __Create project__ page is displayed.

There are several options to create a project from:
* [from a repository URL](#From+Repository+URL)
* [from GitHub.com](#Creating+project+pointing+to+GitHub.com+repository)
* [from Bitbucket (Cloud, Server, Data Center)](#Creating+project+pointing+to+Bitbucket)
* [from GitLab](#Creating+project+pointing+to+GitLab.com)
* [from Azure DevOps](#Creating+project+pointing+to+Azure+DevOps+Services)
* [from JetBrains Space](#Creating+project+pointing+to+JetBrains+Space)
* [manually](#Creating+project+manually)
 
Note that only two options are available by default: _From a repository URL_ and _Manually_. 
If a [connection](integrating-teamcity-with-vcs-hosting-services.md) to some VCS hosting service is configured in the Root project (or a parent project of the project to be created), the corresponding option becomes available, so you can create a project using an existing VCS connection specification.

>After you create a project from a URL or VCS, you can edit the project description. To add a link to your project description, use the Markdown format: \[My Project \](https://www.example.com).

To create a subproject, go to the [project settings](project-administrator-guide.md#Edit+and+View+Modes) page of the parent project and use one of the available options, similarly to creating a project.

### Creating project pointing to repository URL

1. On the __Create project__ page, click the "From a repository URL" tile.

2. Specify the [project settings](project-administrator-guide.md#Edit+and+View+Modes):

    <table>
        <tr>
            <td>Setting</td>
            <td>Description</td>
        </tr>
        <tr>
            <td>Parent Project</td>
            <td>Select the parent project from the drop-down menu.</td>
        </tr>
        <tr>
            <td>Repository URL</td>
            <td>A VCS repository <a href="guess-settings-from-repository-url.md">URL</a>. TeamCity recognizes URLs for Subversion, Git, and Mercurial. TFS and Perforce are partially supported.<br/>Icons next to this field represent VCS hosting services supported by TeamCity. If you click an active (highlighted) icon, you will be able to select an existing connection specification. If you click an inactive icon, you will be redirected to the _Add Connection_ form.<br/>Depending on the URL format, the following authentication settings may vary.</td>
        </tr>
        <tr>
            <td>Authentication</td>
            <td>TeamCity automatically sets this field to either "Password / Access token" or "SSH key" depending on the URL format.</td>
        </tr>
        <tr>
            <td>Username</td>
            <td>Available if the <b>Authentication</b> is set to "Password / Access token". Specifies a username required to access the repository. Can be left empty if you want to access a public repository that allows anonymous access.</td>
        </tr>
        <tr>
            <td>Password</td>
            <td>Available if the <b>Authentication</b> is set to "Password / Access token". Specifies a password or token required to access the repository. Can be left empty if you want to access a public repository that allows anonymous access.</td>
        </tr>
        <tr>
            <td>SSH key</td>
            <td>Available if the <b>Authentication</b> is set to "SSH key". Allows you to upload a new private SSH key or choose a <a href="ssh-keys-management.md#Upload+New+SSH+Keys+to+a+Project">previously uploaded key</a>.</td>
        </tr>
        <tr>
            <td>SSH passphrase</td>
        <td>Available if the <b>Authentication</b> is set to "SSH key". Allows you to specify a passphrase for encrypted SSH keys.</td>
        </tr>
    </table>

3. Click __Proceed__. TeamCity will configure the rest of settings for you.

   * it will determine the type of the VCS repository, autoconfigure VCS repository settings, and suggest the project and build configuration names.   
      For a Git repository, TeamCity will autodetect the default branch, but you have an option to change it and to add other branches to monitor by entering their [specification](working-with-feature-branches.md#Configuring+Branches).
   * the project, build configuration and VCS root will be created automatically.
   * TeamCity will add a VCS build trigger.
   * TeamCity will attempt to autodetect build steps: Ant, NAnt, Gradle, Maven, MSBuild, Visual Studio solution files, PowerShell, Xcode project files, Rake, and IntelliJ IDEA projects.
    
4. On the __Auto-detected Build Steps__ page select the detected step(s) to use in your build configuration. Click __Use selected__. If no steps found, you will have to [configure build steps manually](configuring-build-steps.md).

5. Your project and a build configuration are configured. Click the __Run__ button to start the build.   
Depending on the build configuration settings, TeamCity can suggest some additional configuration options. Review Suggestions at the end of the settings list and configure required ones.

### Creating project pointing to GitHub.com repository

1. On the __Create project__ page, click a "From GitHub" tile to create a project from an [existing connection](configuring-connections.md#GitHub)
2. Select a repository. TeamCity will verify the repository connection. If the connection is verified, the new page opens.
3. TeamCity will display the project and build configuration name. If required, modify the names and click __Proceed__. TeamCity will autodetect the default Git branch, but you have an option to change it and to add other branches to monitor by entering their [specification](working-with-feature-branches.md#Configuring+Branches).
4. TeamCity will add a VCS build trigger and attempt to autodetect build steps: Ant, NAnt, Gradle, Maven, MSBuild, Visual Studio solution files, PowerShell, Xcode project files, Rake, and IntelliJ IDEA projects.   
    On the __Auto-detected Build Steps__ page, select the detected step(s) to use in your build configuration. Click __Use selected__.   
    If no steps found, you will have to [configure build steps manually](configuring-build-steps.md).
5. Your project and a build configuration are configured. Click __Run__ to start the build. Depending on the build configuration settings, TeamCity can suggest some additional configuration options. Review _Suggestions_ at the end of the settings list and configure required ones.

### Creating project pointing to Bitbucket

1. On the __Create project__ page, click a corresponding "From Bitbucket" tile to create a project from an existing connection to Bitbucket Cloud, Server, or Data Center. See this help article to learn how to set up new connections: [Bitbucket Cloud](configuring-connections.md#Bitbucket+Cloud) | [Bitbucket Server and Data Center](configuring-connections.md#Bitbucket+Server+and+Data+Center).
2. Select a repository. TeamCity will verify the repository connection. If the connection is verified, the new page opens.
3. TeamCity will display the project and build configuration name. If required, modify the names and click __Proceed__. For a Git repository, TeamCity will autodetect the default branch, but you have an option to change it and to add other branches to monitor by entering their [specification](working-with-feature-branches.md#Configuring+Branches).
4. TeamCity will add a VCS build trigger and attempt to autodetect build steps: Ant, NAnt, Gradle, Maven, MSBuild, Visual Studio solution files, PowerShell, Xcode project files, Rake, and IntelliJ IDEA projects.  
On the __Auto-detected Build Steps__ page, select the detected step(s) to use in your build configuration. Click __Use selected__.  
If no steps found, you will have to [configure build steps manually](configuring-build-steps.md).
5. Your project and a build configuration are configured. Click __Run__ to start the build. Depending on the build configuration settings, TeamCity can suggest some additional configuration options. Review _Suggestions_ at the end of the settings list and configure required ones.

### Creating project pointing to GitLab.com
1. On the __Create project__ page, click a "From GitLab" tile to create a project from an [existing connection](configuring-connections.md#GitLab).
2. Select a repository. TeamCity will verify the repository connection. If the connection is verified, the new page opens.
3. TeamCity will display the project and build configuration name. If required, modify the names and click __Proceed__. TeamCity will autodetect the default Git branch, but you have an option to change it and to add other branches to monitor by entering their [specification](working-with-feature-branches.md#Configuring+Branches).
4. TeamCity will add a VCS build trigger and attempt to autodetect build steps: Ant, NAnt, Gradle, Maven, MSBuild, Visual Studio solution files, PowerShell, Xcode project files, Rake, and IntelliJ IDEA projects.   
On the __Auto-detected Build Steps__ page, select the detected step(s) to use in your build configuration. Click __Use selected__.   
If no steps found, you will have to [configure build steps manually](configuring-build-steps.md).
5. Your project and a build configuration are configured. Click __Run__ to start the build. Depending on the build configuration settings, TeamCity can suggest some additional configuration options. Review _Suggestions_ at the end of the settings list and configure required ones.

### Creating project pointing to Azure DevOps Services

1. On the __Create project__ page, click a "From Azure DevOps" tile to create a project from an [existing connection](configuring-connections.md#Azure+DevOps). The recommended approach for Git repositories is to use the [connection based on OAuth 2.0 protocol](configuring-connections.md#Connecting+to+Azure+DevOps). If you need to connect to a TFVC repository, use the obsolete [PAT-based connection](configuring-connections.md#Azure+DevOps+PAT+Connection).
2. Select a repository. TeamCity will verify the repository connection. If the connection is verified, the new page opens.
3. TeamCity will display the project and build configuration name. If required, modify the names and click __Proceed__. For a Git repository, TeamCity will autodetect the default branch, but you have an option to change it and to add other branches to monitor by entering their [specification](working-with-feature-branches.md#Configuring+Branches).
4. TeamCity will add a VCS build trigger and attempt to autodetect build steps.   
On the __Auto\-detected Build Steps page__ select the detected step(s) to use in your build configuration. Click __Use selected__.   
If no steps found, you will have to [configure build steps manually](configuring-build-steps.md).
5. Your project and a build configuration are configured. Click __Run__ to start the build. Depending on the build configuration settings, TeamCity can suggest some additional configuration options. Review _Suggestions_ at the end of the settings list and configure required ones.

### Creating project pointing to JetBrains Space

>If you are looking for how to integrate your JetBrains Space instance with TeamCity, check out this **[full integration guide](how-to-configure-cicd-for-jetbrains-space.md)**!

Before creating a project from a JetBrains Space, you need to configure a [dedicated connection to your Space instance](configuring-connections.md#connect-to-jetbrains-space).

1. On the __Create project__ page, click a "From JetBrains Space" tile to create a project from an [existing connection](configuring-connections.md#connect-to-jetbrains-space). The first time, you will be prompted to sign in to Space and grant TeamCity access to view your user profile and projects. To be able to do this, TeamCity will create a service token for authenticating in your Space instance.
2. Select a repository. TeamCity will verify the repository connection. If the connection is verified, the new page opens.
3. TeamCity will display the project and build configuration names. If required, modify them and click __Proceed__. For a Git repository, TeamCity will autodetect the default branch, but you have an option to change it and to add other branches to monitor by entering their [specification](working-with-feature-branches.md#Configuring+Branches).
4. TeamCity will add a VCS build trigger and attempt to autodetect build steps.   
   On the __Auto\-detected Build Steps page__ select the detected step(s) to use in your build configuration. Click __Use selected__.   
   If no steps found, you will have to [configure build steps manually](configuring-build-steps.md).
5. Your project and a build configuration are configured. Click __Run__ to start the build. Depending on the build configuration settings, TeamCity can suggest some additional configuration options. Review _Suggestions_ at the end of the settings list and configure required ones.

### Creating project manually
1. Click the __Create project__ button and select __Manually__.

2. On the __Create New Project__ page, specify the [project settings](project-administrator-guide.md#Edit+and+View+Modes):

<table><tr>

<td>

Setting


</td>

<td>

Description


</td></tr><tr>

<td>

Parent Project


</td>

<td>

Select the parent project from the drop-down menu.


</td></tr><tr>

<td>

Name


</td>

<td>

The project name.


</td></tr><tr>

<td>

Project ID


</td>

<td>

[ID](identifier.md) of the project


</td></tr><tr>

<td>

Description


</td>

<td>

Optional description for the project. You can add a link in the Markdown format to the description:

```Text
[My Project](https://www.example.com)
```

</td></tr></table>

3. Click __Create__. An empty project is created.

<tip>

To configure an existing project, select the desired project in the list and [open its settings](project-administrator-guide.md#Edit+and+View+Modes).
</tip>

4. [Create build configurations](creating-and-editing-build-configurations.md) (select build settings, [configure VCS settings](configuring-vcs-settings.md), and choose [build steps](configuring-build-steps.md)) for the project.

5. [Assign build configurations to specific build agents](assigning-build-configurations-to-specific-build-agents.md).

-->

## Managing Project

You can view all available projects and subprojects on the __Projects__ page listed in the alphabetical order by default. Administrators can [customize the default order](ordering-projects-and-build-configurations.md).

When you select a project from the list, TeamCity displays the __Project Home__ page where you can preview its nested build configurations and recent build results. To access the project's settings, click the corresponding toggle in the top right corner to switch to the [edit mode](project-administrator-guide.md#Edit+and+View+Modes).

To copy, move, delete or [archive](archiving-projects.md) a project, use the __Actions__ menu in the upper right corner of the [project settings](project-administrator-guide.md#Edit+and+View+Modes) page or the _More_ button ![moreButton.PNG](moreButton.PNG) next to the project on the parent [project settings](project-administrator-guide.md#Edit+and+View+Modes) page. These options are not available for the Root project.

### Copying Project

Use the corresponding item from the __Actions__ menu in the upper right corner of the [project settings](project-administrator-guide.md#Edit+and+View+Modes) page or the _More_ button ![moreButton.PNG](moreButton.PNG) next to the project on the parent [project settings](project-administrator-guide.md#Edit+and+View+Modes) page.

Projects can be copied and moved to another project by project administrators.

A copy duplicates all the settings, [subprojects](project-administrator-guide.md#Steps%2C+Configurations+and+Projects), [build configurations](managing-builds.md), and [templates](build-configuration-template.md) of the original project, but no data related to builds is preserved. The copy is created with the empty [build history](build-results-page.md#Build+History+in+Classic+UI) and no [statistics](statistic-charts.md).

You can copy a project into the same or another parent.

On copying, TeamCity automatically assigns a new name and [ID](identifier.md) to the copy. It is also possible to change the name and ID manually.   
Selecting the __Copy project-associated user, agent and other settings__ option makes sure that all the settings like notification rules or agent's compatibility are exactly the same for the copied and original projects for all the users and agents affected.

You can also opt to copy build configurations build numbers.

<note instance="tc">

When running TeamCity in the [Professional mode](licensing-policy.md), the __Copy__ option will not be displayed for a project if the number of build configurations on the server after copying will exceed the limit (100 build configurations, unless you purchased additional build agent licenses).
</note>


<anchor name="CreatingandEditingProjects-MovingProject"/>

### Moving Project

<warning>

Before moving the project, consider the following:
* TeamCity assigns user roles on a [per-project](managing-roles-and-permissions.md) basis, which means that moving a project may result in __changing the scope of user permissions__ in the new project (new permissions may be added or the existing permissions can be dropped)
* Connection to Git VCS Roots containing SSH keys may get unavailable after a project move.
</warning>


To move a project, use the corresponding item from the __Actions__ menu in the upper right corner of the [project settings](project-administrator-guide.md#Edit+and+View+Modes) page or the _More_ button ![moreButton.PNG](moreButton.PNG) next to the project on the parent [project settings](project-administrator-guide.md#Edit+and+View+Modes) page.

When moving a project, TeamCity preserves all its settings, [subprojects](project-administrator-guide.md#Steps%2C+Configurations+and+Projects), [build configurations](managing-builds.md)/[templates](build-configuration-template.md), and associated data, as well as the [build history](build-results-page.md#Build+History+in+Classic+UI).

### Archiving Project

Use the corresponding item from the __Actions__ menu in the upper right corner of the [project settings](project-administrator-guide.md#Edit+and+View+Modes) page or the _More_ button ![moreButton.PNG](moreButton.PNG) next to the project on the parent [project settings](project-administrator-guide.md#Edit+and+View+Modes) page. Refer to the dedicated [page](archiving-projects.md).

### Bulk Editing IDs

<warning>

Care must be taken when performing this action. Modifying the ID will change all the URLs related to the project. It is highly recommended to update the ID in any of the URLs bookmarked or hard\-coded in the scripts. The corresponding configuration and artifacts directory names on the disk will change too and it can take time.
</warning>

1. Use the corresponding item from the __Actions__ menu in the upper right corner of the [project settings](project-administrator-guide.md#Edit+and+View+Modes) page or the _More_ button ![moreButton.PNG](moreButton.PNG) next to the project on the parent [project settings](project-administrator-guide.md#Edit+and+View+Modes) page.
2. The current project and build configuration [IDs](identifier.md) are displayed. You can modify or reset the IDs for all subproject, VCS roots, build configurations and templates. Click __Regenerate__ to get new Ids automatically or edit them manually.
3. Click __Submit__.

### Pausing / Activating Triggers

You can [pause triggers](changing-build-configuration-status.md#Pausing+Several+Build+Configurations+in+Project) for all or selected build configurations of a project. Use the corresponding item from the __Actions__ menu in the upper right corner of the [project settings](project-administrator-guide.md#Edit+and+View+Modes) page or the _More_ button ![moreButton.PNG](moreButton.PNG) next to the project on the parent [project settings](project-administrator-guide.md#Edit+and+View+Modes) page.

### Exporting Project 

You can [export configuration files](project-export.md) of a project with its children to move it to a different TeamCity server. Use the corresponding item from the __Actions__ menu in the upper right corner of the [project settings](project-administrator-guide.md#Edit+and+View+Modes) page or the _More_ button ![moreButton.PNG](moreButton.PNG) next to the project on the parent [project settings](project-administrator-guide.md#Edit+and+View+Modes) page.

### Deleting Project

Use the corresponding item from the __Actions__ menu in the upper right corner of the [project settings](project-administrator-guide.md#Edit+and+View+Modes) page or the _More_ button ![moreButton.PNG](moreButton.PNG) next to the project on the parent [project settings](project-administrator-guide.md#Edit+and+View+Modes) page.

When you delete a project, TeamCity will remove its `.xml` configuration files. After the deletion, the project is moved to the \<[TeamCity Data Directory](teamcity-data-directory.md)\>/config/_trash/.ProjectID.projectN directory. There is a [configurable](teamcity-data-clean-up.md#Deleted+Build+Configurations+Clean-up) timeout (5 days by default) before all project-related data stored in the database (build history, artifacts, and so on) of the deleted project is completely removed during the next build history clean-up.

>You can [restore](how-to.md#Restore+Just+Deleted+Project) a deleted project before the clean-up is run.
>
{instance="tc"}

The [TeamCity Data Directory](teamcity-data-directory.md)/config/_trash/ directory is not cleaned automatically and can be emptied manually if you are sure you do not need the deleted projects. 

<tip>

If you attempt to delete a project with [dependent build configurations](configuring-dependencies.md) from other projects, TeamCity will warn you about it. If you proceed with the deletion, the dependencies will no longer function.

</tip>

 <seealso>
        <category ref="admin-guide">
            <a href="creating-and-editing-build-configurations.md">Creating and Editing Build Configurations</a>
        </category>
        <category ref="examples">
            <a href="how-to-configure-cicd-for-jetbrains-space.md">How to Configure CI/CD for JetBrains Space</a>
        </category>
</seealso>