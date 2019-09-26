[//]: # (title: Storing Project Settings in Version Control)
[//]: # (auxiliary-id: Storing Project Settings in Version Control)

On this page:

<tag-list of="chapter" mode="tree" depth="4"/>

## Overview

TeamCity allows the two-way synchronization of the project settings with the version control repository. Supported VCSs are Git, Mercurial, Perforce, Subversion, and Azure DevOps Server (formerly TFS).

You can store settings in the XML format and in the [Kotlin language](https://kotlinlang.org/) and define settings programmatically using the [kotlin-based DSL](kotlin-dsl.md). 

When you enable two-way settings synchronization:  
* Each administrative change made to the project settings in the TeamCity web UI is committed to the version control; the changes are made noting the TeamCity user as the committer.
* If the settings change is committed to the version control, the TeamCity server will detect the modifications and apply them to the project on the fly.    
Before applying the newly checked-in settings, certain constraints are applied. If the constraints are not met (that is, the settings are invalid), the current settings are left intact and an error is shown in the UI. Invalid settings are those that cannot be loaded because of constraints, for instance, a build configuration referencing a non\-existing VCS root, of having a duplicate ID or a duplicate name, and so on.

The settings in the VCS are stored in the `.teamcity` folder in the root of the repository the same format as in the [TeamCity Data Directory](teamcity-data-directory.md).

## Synchronizing Settings with VCS

By default, the synchronization of the project settings with the version control is disabled.

To enable it, go to __Project Settings | Versioned Settings__.

The "_Enable/disable versioned settings_" permission is required (default for the [System Administrator](role-and-permission.md#Per-Project+Authorization+Mode) role).

The __Configuration__ tab is used to define
* whether the synchronization settings are the same as in the parent project;
* whether the synchronization is enabled;
   * when synchronization is enabled, you can define which settings to use when build starts. See details [below](#Defining+Settings+to+Apply+to+Builds).
* which VCS Root is used to store the project settings: you can store the settings either in the same repository as the source code, or in a dedicated VCS root.

Enabling synchronization for a project also enables it for all its subprojects. TeamCity synchronizes all changes to the project settings (including modifications of [build configurations](build-configuration.md), [templates](build-configuration-template.md), [VCS roots](vcs-root.md), and so on) with the exception of [SSH keys](ssh-keys-management.md).

You can override the synchronization settings inherited from a project for a subproject.

As soon as synchronization is enabled in a project, TeamCity will make an initial commit in the selected repository for the whole project tree (the project with all its subprojects) to store the current settings from the server. If the settings for the given project are found in the specified VCS root (the VCS root for the parent project settings or the user\-selected VCS root), a warning will be displayed asking if TeamCity should
* overwrite the settings in the VCS with the current project settings on the TeamCity server; or
* import the settings from the VCS replacing the current project settings on the TeamCity server with those from version control.

### Defining Settings to Apply to Builds

There are two sources of build settings: (1) the current settings on the TeamCity server, that is the latest settings changes applied to the server (either made via the UI, or via a commit into the `.teamcity` directory in the VCS root) and (2) the settings in the VCS on the revision selected for a build.  
Therefore, it is possible to start builds with settings different from those currently defined in the build configuration. For projects where versioned settings are enabled, you can tell TeamCity which settings to take __when build starts__.    
This gives you several possibilities:
* If you are using TeamCity [feature branches](working-with-feature-branches.md), you can define a branch specification in the VCS root used for versioned settings, and TeamCity will run a build in a branch using the settings from this branch.
* You can start a [personal build](personal-build.md) with changes made in the `.teamcity` directory, and these changes will affect the build behavior.
* When running a [history build](history-build.md), TeamCity will attempt to use the settings corresponding to the moment of the selected change. Otherwise, the current project settings will be used.

Before starting a build, TeamCity stores configuration for this build in build internal artifacts under `.teamcity/settings` directory. These configuration files can be examined later to understand what settings were actually used by the build.

To define which settings to take __when build starts__, open the __Project Settings | Versioned Settings__ page click _Show advanced options_ and select one of the following options:
* __always use current settings__: all builds use current project settings from the TeamCity server. Settings changes in branches, history, and personal builds are ignored. Users cannot run a build with custom project settings.
* __use current settings by default__: a build uses the latest project settings from the TeamCity server. Users can run a build with older project settings via the [custom build dialog](triggering-a-custom-build.md#Changes).
* __use settings from VCS__: builds in branches and history builds, which use settings from VCS, load settings from the versioned settings revision calculated for the build. Users can change configuration settings in [personal builds from IDE](remote-run.md) or can run a build with project settings current on the TeamCity server via the [custom build dialog](triggering-a-custom-build.md#Changes).
   * changes in the following settings will __affect__ the build:
     * build number pattern
     * build steps (build steps parameters and their order)
     * build configuration parameters
     * system properties and environment variables
     * agent requirements
     * build features (except Automatic merge and VCS labeling)
     * failure conditions
     * artifact publishing rules
     * artifact dependencies
  * changes in the following settings will be ignored and __won't affect__ the build:
     * VCS roots and checkout rules
     * snapshot dependencies
     * build triggers
     * build configuration level options, like hanging builds detection, enabling/disabling of triggering of personal builds, or build configuration type

## Storing Secure Settings 

It is recommended to store security data outside the VCS. The __Project Settings | Versioned Settings | Configuration__ page has an option to __store passwords, API tokens, and other secure settings outside of VCS__. This option is enabled by default if versioned settings are enabled for a project for the first time, and not enabled for projects already storing their settings in the VCS.

If this option is enabled, TeamCity stores a random generated IDs in XML configuration files instead of the scrambled passwords. Actual passwords are stored on the disk under the TeamCity Data Directory and are not checked into the version control system.

<warning>

If this option is disabled, the [security implications](#Implications+of+Storing+Security+Data+in+VCS) listed below should be taken into account before committing security data to the VCS.
</warning>

<anchor name="tokensGen"/>

### Generating Tokens
[//]: # (AltHead: tokensGen)

If you need to add a password into the versioned settings not via the TeamCity UI (for example, adding settings with Kotlin\-based `DSL`), add the password to the project in TeamCity and get the corresponding token to use in the settings (select _Generate token for a secure value_ in the __Actions__ drop-down menu). 

At this time passwords are not inheritable by projects hierarchy. If a setting in a project (a VCS root, OAuth connection, cloud profile) requires a password, the token generated for this password can be used in this project only. For instance, it is not possible to take a generated token and use it in a similar setting in a subproject. A new token should be generated in this case.   
If you need to use a secure value in the nested projects, consider adding a [password parameter](typed-parameters.md#Adding+Parameter+Specification) with the secure value and using a [reference](configuring-build-parameters.md#Using+Build+Parameters+in+Build+Configuration+Settings) to the parameter in the nested projects.


### Implications of Storing Security Data in VCS 

If you are using a version __prior to TeamCity 2017.1__, it is recommended to carefully consider the implications of storing security settings in a VCS.
* If the projects or build configurations with settings in a VCS have __password fields defined__, the values appear in the settings committed into the VCS (though, in scrambled form).
   * If the project settings are stored in __the same repository as the source code__, anyone with access to the repository will be able to see these scrambled passwords.
   * If the project settings are stored __separately from the source code in a dedicated repository__ and the _Show settings changes in builds_ option is enabled, any user having the "View VCS file content" permission can see all the changes in the TeamCity UI using the [changes difference viewer](difference-viewer.md).
* Being able to change the settings in an arbitrary manner via a VCS, it is possible to trigger builds of any build configurations and obtain settings of any build configurations irrespective of the build configurations permissions configured in TeamCity.
* by committing wrong or malicious settings, a user can affect the entire server performance or server presentation to other users.

It is recommended to store passwords, API tokens, and other secure settings outside of VCS using the corresponding option [described above](#Storing+Secure+Settings).

Note that SSH keys will not be stored in the VCS repository.

<anchor name="settingsFormat"/>

## Settings Format
[//]: # (AltHead: settingsFormat)

You can select the settings format: on the __Versioned Settings | Configuration__ page for your project, click _Show advanced options_. 

TeamCity stores project settings:
* in the XML format
* in the Kotlin-based DSL format (see a [dedicated page](kotlin-dsl.md)). 

 

<note>

To test kotlin\-based settings in a sandbox project, you can download the settings as a zip archive without disabling the UI: on the project settings page, click the __Actions__ menu and select __Download settings in kotlin format.__ A zip archive will be downloaded.
</note>

<anchor name="ForcingSynch"/>

## Committing Current Project Settings to VCS
[//]: # (AltHead: settingsFormat)

<warning>

Before committing settings to the VCS, consider the recommended approach to storing security settings [described above](#Storing+Secure+Settings).
</warning>

If you want to commit the current configuration to the VCS (for example, earlier you committed misconfigured settings to the repository and TeamCity was unable to load it displaying errors and warnings), you can use the __Commit current project settings...__ option on the __Versioned Settings | Configuration__ page.

When TeamCity commits settings into a VCS, it uses a  standard commit message noting the TeamCity user as the committer and the project whose settings changed. It is possible to add a fixed custom prefix to each settings change committed by TeamCity via the `teamcity.versionedSettings.commitMessagePrefix` [internal property](configuring-teamcity-server-startup-properties.md), for example, `teamcity.versionedSettings.commitMessagePrefix=TC Change\n\n`

## Displaying Changes

TeamCity will not only synchronize the settings, but will also automatically display changes to the project settings the same way it is done for regular changes in the version control. You can configure the changes to be displayed for the affected build configurations: on the __Project Settings | Versioned Settings__ page, __Configuration__ tab, click __Show advanced options__ and check the _Show settings changes in builds_ box.

By default, the VCS trigger will ignore such changes. To enable build triggering on a settings commit, add a trigger rule in the following format: `+:root=Settings_root_id;:*`.

All changes in the VCS root where project settings are stored are listed on the __Versioned Settings | Change log__ tab of the __Versioned Settings__ page.

<anchor name="enableAfterUpgrade"/>

## Enabling Versioned Settings after TeamCity Upgrade
[//]: # (AltHead: enableAfterUpgrade)

The format of the XML settings files changes from one TeamCity version to another to accommodate the new features and improvements. Generally, the format is not changed within bugfix releases and is changed in minor/major releases. When a TeamCity server is upgraded, the current settings on the TeamCity server are changed from the earlier to the current format.

It is a common practice to upgrade a TeamCity test server with production data before upgrading the production server. In order to avoid accidentally changing the format of the settings which are used on a production server of an older version, versioned settings are disabled after a TeamCity upgrade and the corresponding health item is displayed. System administrators have permissions to enable versioned settings. When enabled, the converted settings in the format of the current TeamCity version will checked in the version control. Note that the new settings will be committed to the default branch of the VCS root; the settings stored in other branches will have to be updated manually.

## FAQs

Q. Can I apply the settings from a TeamCity server of a different version?   
A. No, because just like with the TeamCity Data Directory, the format of the settings differs from one TeamCity version to another.

Q. Where are the settings stored?   
A. The settings are stored in the `.teamcity` folder in the root of the VCS root\-configured repository path. For Git and Mercurial this is always the root of the repository. The default branch configured in the VCS root is used with [Git](git.md), [Mercurial](mercurial.md). You can create a dedicated VCS root to change the branch (or a repository path in case of Perforce, Subversion, or Azure DevOps – formerly TFS).

Q. Why is there a delay before a build is run after I changed to the settings in the UI?   
A. When the settings are changed via the UI, TeamCity will wait for the changes to be completed with a commit to the VCS before running a build with the latest changes.

Q. Who are the changes authored by?   
A. If the settings are changed via the user interface, in Git and Mercurial a commit in the VCS will be performed on behalf of the user who actually made the change via the UI. For Perforce as well as Azure DevOps Server (formerly TFS), the name of the user  specified in the VCS root  is used, and in Subversion the commit message will also contain the username of the TeamCity user who actually made the change via the UI.



__  __

__See also:__

__Administrator's Guide__: [Kotlin DSL](kotlin-dsl.md) 

__ __