[//]: # (title: What's New in TeamCity On-Premises 2024.07)

<chunk include-id="2024-07-tc">

## New Licensing Mechanism
{product="tc"}

Experience seamless license management with our newest [JetBrains Account](https://account.jetbrains.com/licenses) integration. Log into the JetBrains Account from TeamCity and choose a server license to activate. Once connected, TeamCity will automatically receive updates for all server and agent licenses, meaning you will no longer need to manually enter your license codes for any additional agents or server subscription updates.

The new mechanism does not impact existing licensing or purchase policies and is entirely free, regardless of your server license type (including free-tier Professional servers). It benefits all TeamCity users by linking your server instance to the JetBrains account, enabling server administrators to receive timely email notifications about critical security updates.

Learn more: [](manage-teamcity-license.md).


## New GitHub Checks Webhook Trigger
{product="tc"}

Version 2024.07 extends the collection of [build triggers](configuring-build-triggers.md) with a new trigger designed exclusively for GitHub-facing build configurations set up via [TeamCity GitHub App connections](configuring-connections.md#github-app).

<img src="dk-checkstrigger-ghsummary.png" width="706" alt="Build summary info on GitHub"/>

The new trigger is an all-in-one solution that implements a two-way GitHub-TeamCity integration: TeamCity configuration runs a build for all pushes, no matter how frequently they occur, and communicates the build result status back to GitHub. With this trigger in place, you no longer need to configure the standard [VCS trigger](configuring-vcs-triggers.md) or [](commit-status-publisher.md).

Learn more: [](github-checks-trigger.md)

## Custom Location for Versioned Settings
{product="tc"}

When you enable synchronization on the **Versioned Settings** page in your project settings, you can now specify a custom location for your remotely stored Kotlin DSL scripts and XML settings.

<img src="dk-vcs-settings-custompath.png" width="706" alt="Custom settings path"/>

This flexibility is especially beneficial for teams working with monorepos. It allows multiple standalone TeamCity projects to target the same repositories without conflicts, by storing project settings in different custom directories instead of the default `.teamcity` folder.

Additionally, this feature enables you to organize the versioned settings of all your projects in a single repository. This streamlined approach simplifies DSL script maintenance and enhances security by keeping configuration files separate from your main codebase.

Learn more: [Storing Project Settings in Version Control Systems](storing-project-settings-in-version-control.md#Custom+Settings+Path)


## Bootstrap Steps
{product="tc"}

You can now add [build steps](configuring-build-steps.md) that run right after a build starts, before source files are checked out on an agent.

<img src="dk_bootstrap_step.png" width="706" alt="Bootstrap step"/>

This enhancement allows you to perform preliminary setup, such as preparing a required directory hierarchy or making sure the required files in the checkout directory are not locked by another process.

Learn more: [](configuring-build-steps.md#Bootstrap+Steps)



## Upload SSH Keys from the Create Project Page
{product="tc"}

When you create a new project [from an SSH URL](creating-and-editing-projects.md#Creating+project+pointing+to+repository+URL), TeamCity shows a list of available SSH keys. Starting from this version, you can upload new keys directly from this page.

<img src="dk-ssh-on-new-project.png" width="706" alt="Upload SSH key on new project page"/>

This minor enhancement streamlines the workflow by eliminating the need to upload keys in advance from the [SSH Keys](ssh-keys-management.md#Upload+New+SSH+Keys+to+a+Project) page.

Learn more: [](ssh-keys-management.md#Upload+SSH+Keys+to+TeamCity+Server)


## Security Enhancement for TeamCity Connections
{product="tc"}

Following our commitment to identify and eliminate security vulnerabilities, we have added the new **Enable unique callback/redirect URL** setting to the following [TeamCity connections](configuring-connections.md):

* Azure DevOps OAuth 2.0
* Bitbucket Server/Data Center
* GitHub App (manual)
* GitHub Enterprise
* GitLab CE/EE
* JetBrains Space (manual)

<img src="dk-unique-url.png" width="706" alt="Unique URL in connections"/>

This setting adds a unique string to callback/redirect URLs required to configure OAuth integration with version control systems. Using unique URLs prevents attackers from implementing a malicious authentication server that mimics a real one — the technique used in potential mix-up attacks that trick client applications into leaking VCS authentication codes.

The **Enable unique callback/redirect URL** setting is enabled by default for all newly created OAuth connections. If you wish to enable it for your existing connections, remember to update the modified URL on the VCS side.

Learn more: [](configuring-connections.md)


## Non-Recursive Submodule Checkout
{product="tc"}

The **Submodules** setting of [Git VCS roots](git.md) allows you to choose whether TeamCity should check out submodule repositories referenced by your main repo. The "Checkout" option corresponds to the recursive checkout process, making TeamCity fetch the entire hierarchy of repositories.

The new "Non-recursive checkout" option added in this version allows you to limit the depth of submodule hierarchy by 1. In this mode, TeamCity checks out only those submodules directly referenced by the main repository. Lower-level repositories referenced by submodules are ignored.

<img src="dk-nonrecursive-checkout.png" width="706" alt="Non-recursive checkout"/>

Learn more: [Git General Settings](git.md#General+Settings)

## NUnit and NAnt Runners Update
{product="tc"}

<include src="nunit.md" include-id="2024-07-nunit"/>

Additionally, we are deprecating the [](nant.md) runner. Unlike NUnit, it has no updated counterpart and will only be available via a separate Marketplace plugin once unbundled.



## Miscellaneous Changes
{product="tc"}

* The list of [statistic values](custom-chart.md#Default+Statistics+Values+Provided+by+TeamCity) reported by TeamCity now includes the **AllTestsDuration** value.
* Working with the [interactive agent terminal](install-and-start-teamcity-agents.md#Debug+Agents+Remotely) now counts as a valid agent activity that prolongs the agent lifetime and prevents it from automatic inactivity shutdown.
* The database connection setup wizard that pops up during the first TeamCity server launch now installs the following JDBC drivers depending on the selected database type:
  * MySQL database file: `mysql-connector-j-8.3.0.jar`
  * MS SQL server: `mssql-jdbc-12.6.0.jre8.jar` and `sqlserver12-win-auth.jar` for authentification.
  * PostgreSQL: `postgresql-42.7.1.jar`
* The backslash character (`\`) is now the default escape symbol that allows you to use special characters in [feature branch specifications](working-with-feature-branches.md). This symbol was commonly recommended as an escape symbol in TeamCity documentation and support tickets, and you might already have branch specifications that look like the following:
  ```Plain Text
  #! escape: \
  +:release-\(7.1\)
  ```
  This first line is no longer required and can be safely removed, but your existing specifications will remain valid even if you don't.

</chunk>

