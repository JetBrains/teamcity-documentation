[//]: # (title: Storing Project Settings in Version Control)
[//]: # (auxiliary-id: Storing Project Settings in Version Control)

TeamCity allows synchronizing project settings with the version control repository (VCS). Supported VCSs are Git, Mercurial, Perforce, Subversion, and Azure DevOps Server (formerly TFS).

You can store project settings in the XML format or in the [Kotlin language](https://kotlinlang.org/) and define settings programmatically using the [Kotlin-based DSL](kotlin-dsl.md).

The versioned settings are stored in the `.teamcity` directory in the root of the VCS repository, in the same format as in the [TeamCity Data Directory](teamcity-data-directory.md).

<anchor name="StoringProjectSettingsinVersionControl-SynchronizingSettingswithVCS"/>

## Synchronizing Settings with VCS
{id="SynchronizingSettingswithVCS" auxiliary-id="SynchronizingSettingswithVCS"}
By default, the synchronization of the project settings with the version control system is disabled.

To enable it, go to __Project Settings | Versioned Settings | Configuration__. The "_Enable/disable versioned settings_" permission is required (default for the [System Administrator](role-and-permission.md#Per-Project+Authorization+Mode) role).

Here you can choose one of the following options:
* Use the same settings as in the parent project (default).
* Disable synchronization.
* Enable synchronization. In this case, you can also define which settings to use when the build starts. See details [below](#Defining+Settings+to+Apply+to+Builds).

There are two modes of settings’ synchronization: _two-way_ and _one-way_ (provided in terms of TeamCity 2021.1 EAP).

<anchor name="two-way-sync"/>

The default mode is a _two-way synchronization_, that is when the _Allow editing project settings via UI_ option is enabled. It works as follows:
* Each administrative change made to the project settings in the TeamCity UI is committed to the version control system; the changes are made noting the TeamCity user as the committer.
* If the settings change is committed to the VCS, the TeamCity server will detect them and apply them to the project on the fly.   
  Before applying the newly checked-in settings, TeamCity validates them. If the validation constraints are not met (that is, the settings are invalid), the current settings are left intact and an error is shown in the UI. Invalid settings are those that cannot be loaded because of constraints: for instance, a build configuration references a non-existing VCS root or has a duplicate ID or name.

If you disable the _Allow editing project settings via UI_ option, the project settings will become read-only in the UI and will only reflect changes made in the VCS. This is convenient if you prefer defining project settings’ [as code](kotlin-dsl.md) or load settings from a read-only VCS branch.

Enabling synchronization for a project also enables it for all its subprojects with the default "_Use settings from a parent project_" option selected. TeamCity synchronizes all changes to the project settings (including modifications of [build configurations](build-configuration.md), [templates](build-configuration-template.md), [VCS roots](vcs-root.md), and so on) except [SSH keys](ssh-keys-management.md).   
However, if for certain subprojects the "_Synchronization disabled_" option is selected, such subprojects will not be synchronized even if this option is enabled for their parent project.

As soon as synchronization is enabled in a project, TeamCity will make an initial commit in the selected repository for the whole project tree (the project with all its subprojects) to store the current settings from the server. If the settings for the given project are found in the specified VCS root (the VCS root for the parent project settings or the user-selected VCS root), a warning will be displayed asking if TeamCity should
* overwrite the settings in the VCS with the current project settings on the TeamCity server (only if the two-way [synchronization](#two-way-sync) is enabled); or
* import the settings from the VCS replacing the current project settings on the TeamCity server with those from version control.

>If you choose to generate [portable DSL scripts](kotlin-dsl.md#Project+Settings+Structure) and the current project comprises less than 20 entities, TeamCity will create one `settings.kts` file to define them all. Individual entities include _projects_, _build configurations_, _templates_, and _VCS roots_.  
>If there are more than 20 entities in the project, TeamCity will create a hierarchy of separate `.kt` files to define and group these entities.

On the __Configuration__ tab, you can also choose which VCS root is used to store the project settings: you can store the settings either in the same repository as the source code or in a dedicated VCS root.

<anchor name="StoringProjectSettingsinVersionControl-DefiningSettingstoApplytoBuilds"/>

### Defining Settings to Apply to Builds

There are two possible sources of build settings: (1) the current settings on the TeamCity server, that is the latest settings' changes applied to the server (either made via the UI, or via a commit to the `.teamcity` directory in the VCS root), and (2) the settings in the VCS on the revision selected for a build.  
Therefore, it is possible to start builds with settings different from those currently defined in the build configuration. For projects with enabled versioned settings, you can instruct TeamCity which settings to take __when build starts__.    
This gives multiple options:
* If you are using TeamCity [feature branches](working-with-feature-branches.md), you can define a branch specification [when creating a project from URL](creating-and-editing-projects.md#Creating+project+pointing+to+repository+URL) (Git only) or in the [VCS root](vcs-root.md) used for versioned settings. TeamCity will run a build in a branch using the settings from this branch.
* You can start a [personal build](personal-build.md) with changes made in the `.teamcity` directory, and these changes will affect the build behavior.
* When running a [history build](history-build.md), TeamCity will attempt to use the settings corresponding to the moment of the selected change. Otherwise, the current project settings will be used.

Before starting a build, TeamCity stores configuration for this build in build internal artifacts under the `.teamcity/settings` directory. These configuration files can be examined later to understand what settings were actually used by the build.

To define which settings to take __when build starts__, open the __Project Settings | Versioned Settings__ page, click __Show advanced options__, and select one of the following options:
* __always use current settings__: all builds use current project settings from the TeamCity server. Settings' changes in branches, history, and personal builds are ignored. Users cannot run a build with custom project settings.
* __use current settings by default__: a build uses the latest project settings from the TeamCity server. Users can run a build with older project settings via the [custom build dialog](triggering-a-custom-build.md#Changes).
* __use settings from VCS__: builds in branches and history builds, which use settings from VCS, load settings from the versioned settings' revision calculated for the build. Users can change configuration settings in [personal builds from IDE](remote-run.md) or can run a build with project settings current on the TeamCity server via the [custom build dialog](triggering-a-custom-build.md#Changes).
   * changes in the following settings will __affect__ the build:
     * build number pattern
     * build steps (build steps' parameters and their order)
     * build configuration parameters
     * system properties and environment variables
     * agent requirements
     * build features (except [automatic merge](automatic-merge.md) and [VCS labeling](vcs-labeling.md))
     * failure conditions
     * artifact publishing rules
     * artifact dependencies
  * changes in the following settings coming from a branch will be ignored and __will not affect__ the build:
     * VCS roots and checkout rules
     * snapshot dependencies
     * build triggers
     * build configuration level options, like hanging builds detection, enabling/disabling of triggering of personal builds, or build configuration type
     * clean-up rules

## Storing Secure Settings 

It is recommended to store security data outside of VCS. The _Store passwords and API tokens outside of VCS_ option is available on the __Project Settings | Versioned Settings | Configuration__ page if the [two-way synchronization](#two-way-sync) is enabled. By default, this option is enabled if versioned settings are enabled for a project for the first time, and disabled for projects already storing their settings in the VCS.

If this option is enabled, TeamCity stores randomly generated IDs in XML configuration files instead of the scrambled passwords. Actual passwords are stored on the disk under the [TeamCity Data Directory](teamcity-data-directory.md) and are not checked into the version control system.

<warning>

If this option is disabled, the [security implications](#Implications+of+Storing+Security+Data+in+VCS) listed below should be taken into account before committing security data to the VCS.
</warning>

<anchor name="tokensGen"/>

### Managing Tokens
[//]: # (AltHead: tokensGen)

If you need to add a password (or other secure value) to the versioned settings not via the TeamCity UI (for example, via Kotlin DSL), you can generate a token to be used in the settings instead of this password.   
In the __Project Settings__, select _Generate token for a secure value_ in the __Actions__ drop-down menu. Enter the password and click __Generate Token__. The generated token will be stored on the server. You can copy and use it in the project configuration files instead of the password.

You can also generate new secure tokens on the __Tokens__ tab of the project __Versioned Settings__ section. The tab is available for projects with the enabled "_[Store secure values outside of VCS](#Storing+Secure+Settings)_" option.

The __Tokens__ tab allows viewing all the project tokens, including unused tokens.   
When secure data of a project is stored outside of the version control system, it could potentially be detached from the project: for example, if a project with tokens is moved to another place in the hierarchy or is created from DSL on the new TeamCity server. In such case, you can specify the values for the project tokens on this tab, so the project can continue using them.

Moreover, if the required tokens are available in other projects you are permitted to edit, TeamCity will automatically find them. You can copy the secure values used in any of these projects to your current project.

<img src="tokens-tab.png" alt="Versioned Settings| Tokens" width="800"/>

When there is one or more secure values available for a token, the ![magic-wand.png](magic-wand.png) button appears opposite this token. Click it to review available projects and choose a project to copy a value from, and then click __Copy__ to confirm your choice.

Secure values can be inherited by project hierarchy. If a setting in a project (VCS root, OAuth connection, cloud profile) requires a password, the token generated for this password can be used in this project and in any of its subprojects. To be able to use the inherited password, the subproject must have versioned settings enabled and store settings in the same VCS, as its parent project.   
Alternatively, you can add a [password parameter](typed-parameters.md#Adding+Parameter+Specification) with the secure value and use a [reference](configuring-build-parameters.md#Using+Build+Parameters+in+Build+Configuration+Settings) to the parameter in the nested projects.

### Implications of Storing Security Data in VCS 

If you decide to store secure settings in VCS, it is recommended to carefully consider the following implications:
* If the projects or build configurations with settings in a VCS have __password fields defined__, the values appear in the settings committed into the VCS (though, in scrambled form).
   * If the project settings are stored in __the same repository as the source code__, anyone with access to the repository will be able to see these scrambled passwords.
   * If the project settings are stored __separately from the source code in a dedicated repository__ and the "_Show settings changes in builds_" option is enabled, any user with the "_View VCS file content_" permission will be able to see all the changes in the TeamCity UI using the [changes difference viewer](difference-viewer.md).
* Being able to change the settings in an arbitrary manner via a VCS, it is possible to trigger builds of any build configurations and obtain settings of any build configurations irrespective of the build configurations permissions configured in TeamCity.
* By committing wrong or malicious settings, a user can affect the entire server performance or server presentation to other users.

It is recommended to store passwords, API tokens, and other secure settings outside of VCS using the corresponding option [described above](#Storing+Secure+Settings).

Note that SSH keys will not be stored in the VCS repository.

<anchor name="settingsFormat"/>

## Settings Format
[//]: # (AltHead: settingsFormat)

To be able to select the settings format, click __Show advanced options__ in __Project Settings | Versioned Settings | Configuration__. 

TeamCity stores project settings:
* in the XML format
* in the Kotlin-based DSL format (see a [dedicated page](kotlin-dsl.md))

<note>

To test Kotlin-based settings in a sandbox project, you can download the settings as a ZIP archive without disabling the UI: on the __Project Settings__ page, click the __Actions__ menu and select __Download settings in Kotlin format.__ A ZIP archive will be downloaded.
</note>

<anchor name="ForcingSynch"/>

## Committing Current Project Settings to VCS
[//]: # (AltHead: settingsFormat)

<warning>

Before committing settings to the VCS, consider the recommended approach to storing security settings [described above](#Storing+Secure+Settings).
</warning>

If you want to commit the current configuration to the VCS (for example, earlier you committed misconfigured settings to the repository and TeamCity was unable to load it displaying errors and warnings), you can use the _Commit current project settings_ option on the __Versioned Settings | Configuration__ page.

When TeamCity commits settings into a VCS, it uses the standard commit message noting the TeamCity user as the committer and the project whose settings have changed. It is possible to add a fixed custom prefix to each settings' change committed by TeamCity via the `teamcity.versionedSettings.commitMessagePrefix` [internal property](configuring-teamcity-server-startup-properties.md), for example, `teamcity.versionedSettings.commitMessagePrefix=TC Change\n\n`.
{product="tc"}

When TeamCity commits settings into a VCS, it uses the standard commit message noting the TeamCity user as the committer and the project whose settings have changed.
{product="tcc"}

## Displaying Changes

TeamCity will not only synchronize the settings, but will also automatically display changes to the project settings the same way it is done for regular changes in the version control. You can configure the changes to be displayed for the affected build configurations: on the __Project Settings | Versioned Settings | Configuration__ tab, click __Show advanced options__ and check the _Show settings changes in builds_ box. This option is only effective if you store versioned settings in a VCS root that is not directly attached to the current build configuration. If a VCS root is attached to this configuration, its changes will be displayed by default, unless specific [build triggering rules](configuring-build-triggers.md) are configured.

If you store versioned settings in a separate VCS root, not the one which is directly attached to your build configurations, then when turned ON this checkbox starts showing changes made to these VCS roots. If VCS root is already attached to your configuration, then it does nothing.

By default, the VCS trigger will ignore such changes. To enable build triggering on a settings' commit, add a trigger rule in the following format: `+:root=Settings_root_id;:*`.

All changes in the VCS root, where project settings are stored, are listed on the __Versioned Settings | Change log__ tab.

<anchor name="enableAfterUpgrade"/>
<anchor name="StoringProjectSettingsinVersionControl-enableAfterUpgrade"/>

## Enabling Versioned Settings after TeamCity Upgrade
[//]: # (AltHead: enableAfterUpgrade)

The format of the XML settings files changes from one TeamCity version to another to accommodate the new features and improvements. Generally, the format is not changed within bugfix releases and is changed in major releases. When a TeamCity server is upgraded, the current settings on the TeamCity server are changed from the earlier to the current format.

It is a common practice to upgrade a TeamCity test server with production data before upgrading the production server. In order to avoid accidentally changing the format of the settings which are used on a production server of an older version, versioned settings are disabled after a TeamCity upgrade and the corresponding health item is displayed. System administrators have permissions to enable versioned settings (__Administration | Server Health | Versioned settings disabled__, click __Enable__). When enabled, the converted settings in the format of the current TeamCity version will be checked in the version control. Note that the new settings will be committed to the default branch of the VCS root; the settings stored in other branches will have to be updated manually.

## FAQs

Q. Can I apply the settings from a TeamCity server of a different version?   
A. No, because just like with the TeamCity Data Directory, the format of the settings differs from one TeamCity version to another.

Q. Where are the settings stored?   
A. The settings are stored in the `.teamcity` directory in the root of the VCS root-configured repository path. For Git and Mercurial this is always the root of the repository. The default branch configured in the VCS root is used with [Git](git.md) and [Mercurial](mercurial.md). You can create a dedicated VCS root to change the branch (or a repository path in case of Perforce, Subversion, or Azure DevOps — formerly TFS).

Q. Why is there a delay before a build is run after I changed to the settings in the UI?   
A. When the settings are changed via the UI, TeamCity waits for the changes to be completed with a commit to the VCS before running a build with the latest changes.

Q. Who are the changes authored by?   
A. If the settings are changed via the user interface, in Git and Mercurial a commit in the VCS will be performed on behalf of the user who actually made the change via the UI. For Perforce as well as Azure DevOps Server (formerly TFS), the name of the user specified in the VCS root is used, and in Subversion the commit message will also contain the username of the TeamCity user who actually made the change via the UI.

<seealso>
        <category ref="admin-guide">
            <a href="kotlin-dsl.md">Kotlin DSL</a>
        </category>
</seealso>
