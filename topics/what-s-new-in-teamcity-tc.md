[//]: # (title: What's New in TeamCity On-Premises 2024.07)

<chunk include-id="2024-07-tc">

## Custom Location for Versioned Settings
{product="tc"}

When you enable synchronization on the **Versioned Settings** page in your project settings, you can now specify a custom location for your remotely stored Kotlin DSL scripts.

<img src="dk-vcs-settings-custompath.png" width="706" alt="Custom settings path"/>

This flexibility is especially beneficial for teams working with monorepos. It allows multiple standalone TeamCity projects to target the same repositories without conflicts, by storing project settings in different custom directories instead of the default `.teamcity` folder.

Additionally, this feature enables you to organize the versioned settings of all your projects in a single repository. This streamlined approach simplifies DSL script maintenance and enhances security by keeping configuration files separate from your main codebase.

Learn more: [Storing Project Settings in Version Control Systems](storing-project-settings-in-version-control.md#Custom+Settings+Path)



## Feature Two
{product="tc"}

DESCRIPTION

Learn more: LINK




## Feature Three
{product="tc"}

DESCRIPTION

Learn more: LINK


</chunk>