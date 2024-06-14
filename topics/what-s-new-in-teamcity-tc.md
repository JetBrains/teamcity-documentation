[//]: # (title: What's New in TeamCity On-Premises 2024.07)

<chunk include-id="2024-07-tc">

## New Licensing Mechanism
{product="tc"}

Experience seamless license management with our newest [JetBrains Account](https://account.jetbrains.com/licenses) integration. Log into the JetBrains Account from TeamCity and choose a server license to activate. Once connected, TeamCity will automatically receive updates for all server and agent licenses, meaning you will no longer need to manually enter your license codes for any additional agents or server subscription updates.

The new licensing mechanism benefits free-tier TeamCity Professional users as well, since linking your server instance to the JetBrains account allows your server administrators to receive timely email notifications when new critical security updates are available.

Learn more: [](manage-teamcity-license.md).

## Custom Location for Versioned Settings
{product="tc"}

When you enable synchronization on the **Versioned Settings** page in your project settings, you can now specify a custom location for your remotely stored Kotlin DSL scripts.

<img src="dk-vcs-settings-custompath.png" width="706" alt="Custom settings path"/>

This flexibility is especially beneficial for teams working with monorepos. It allows multiple standalone TeamCity projects to target the same repositories without conflicts, by storing project settings in different custom directories instead of the default `.teamcity` folder.

Additionally, this feature enables you to organize the versioned settings of all your projects in a single repository. This streamlined approach simplifies DSL script maintenance and enhances security by keeping configuration files separate from your main codebase.

Learn more: [Storing Project Settings in Version Control Systems](storing-project-settings-in-version-control.md#Custom+Settings+Path)



## Upload SSH Keys from the Create Project Page
{product="tc"}

When you create a new project [from an SSH URL](creating-and-editing-projects.md#Creating+project+pointing+to+repository+URL), TeamCity shows a list of available SSH keys. Starting from this version, you can upload new keys directly from this page.

<img src="dk-ssh-on-new-project.png" width="706" alt="Upload SSH key on new project page"/>

This minor enhancement streamlines the workflow by eliminating the need to upload keys in advance from the [SSH Keys](ssh-keys-management.md#Upload+New+SSH+Keys+to+a+Project) page.

Learn more: [](ssh-keys-management.md#Upload+SSH+Keys+to+TeamCity+Server)




## Feature Three
{product="tc"}

DESCRIPTION

Learn more: LINK


## Miscellaneous Changes
{product="tc"}

* The list of [statistic values](custom-chart.md#Default+Statistics+Values+Provided+by+TeamCity) reported by TeamCity now includes the **AllTestsDuration** value.
* Working with the [interactive agent terminal](install-and-start-teamcity-agents.md#Debug+Agents+Remotely) now counts as a valid agent activity that prolongs the agent lifetime and prevents it from automatic inactivity shutdown.


</chunk>

