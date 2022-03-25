[//]: # (title: What's New in TeamCity 2022.04)
[//]: # (auxiliary-id: What's New in TeamCity 2022.04;What's New in TeamCity)

>This document is a work in progress. Currently, it contains descriptions of features released in TeamCity Cloud versions 2021.12 and 2022.02.
> 
{type="warning"}

## Single sign-on authentication via SAML 2.0
{product="tc"}

TeamCity now supports a new authentication method: Single Sign-On (SSO) via [SAML 2.0](https://en.wikipedia.org/wiki/SAML_2.0). Enabling it in your instance will allow users to authenticate in TeamCity with their account stored in one of the major identity providers: [Okta](https://www.okta.com/), [OneLogin](https://www.onelogin.com/), [AWS SSO](https://aws.amazon.com/single-sign-on/), [AD FS](https://docs.microsoft.com/en-us/windows-server/identity/active-directory-federation-services), and so on.

With a multitude of services an average software developer uses in their daily work, having one account for authentication in all of them can significantly offload the clutter. More importantly, using SSO identity managers is a good security practice, and TeamCity wants to encourage it by providing easy integration.

>The SAML 2.0 support is implemented as a plugin written by a third-party developer and verified by the TeamCity team as safe to use. It was previously available for download and successfully used by some of our customers, but now it's available out of the box.
>
>See the [SAML 2.0 plugin's GitHub page](https://github.com/morincer/teamcity-plugin-saml) for more details about it.
>
>We are eager to see that such quality and high-demanded tools are created by our users. If you are curious to learn how to write custom plugins for TeamCity, visit our dedicated [Help for external plugin writers](https://plugins.jetbrains.com/docs/teamcity/developing-teamcity-plugins.html)!

You can enable SSO in TeamCity as any other authentication module: in **Administration | Authentication**.

<img src="enable-saml.png" alt="Enabling SAML in TeamCity" width="460"/>

After that, the new **SAML Settings** page will appear in the **Administration** section. Use it to customize the module's behavior and to establish the integration on the TeamCity side. On the provider's side, you will need to create a dedicated app for a service connection to TeamCity. Its settings may vary depending on the provider, but the key steps are the same:
1. Enter the TeamCity single sign-on URL, copied from the **SAML Settings** page, in the app's settings.
2. Copy the values of the app's SSO URL, issuer URL, and certificate.
3. Enter them in the **SAML Settings** in TeamCity.

<img src="saml-settings.png" alt="SAML settings in TeamCity" width="706"/>

See more details about this authentication module in [this article](configuring-authentication-settings.md#HTTP+SAML+2.0).

As soon as the connection between TeamCity and your SSO provider is established, users will see a new authentication button on the TeamCity login form. To sign in, they will need to click it and confirm the authentication on the provider's side.

Depending on the module settings, TeamCity can prohibit authentication to accounts whose emails don't match emails of existing TeamCity users, or it can create a new user profile for such an account.

## Running builds on JetBrains Space merge requests
{product="tc"}

TeamCity integration with JetBrains Space now includes the [Pull Requests](pull-requests.md) build feature. If you enable this feature in a build configuration, TeamCity will automatically detect changes in merge requests submitted to your Space repository. For a build run on these changes, it will display the merge request details in the build's overview and send the build statuses back to JetBrains Space.

To enable this functionality in TeamCity:
1. In the project settings, configure a [connection to JetBrains Space](configuring-connections.md#connect-to-jetbrains-space) and create a Git [VCS root](configuring-vcs-roots.md) with your Space repository URL and an empty branch specification.
2. In the build configuration settings, add the [Pull Requests](pull-requests.md) build feature with the _JetBrains Space_ VCS hosting type:  
   <img src="pull-requests-feature.png" alt="Pull Requests build feature" width="706"/>  
   You can specify the [branch filter](branch-filter.md) to monitor merge requests only on target branches that match the specified criteria. For example, if you set the `+:refs/heads/feature-*` filter, TeamCity will monitor merge requests sent only to branches whose names start with `feature-`.
3. To allow publishing build statuses to the commit details in JetBrains Space, you also need to add the [Commit Status Publisher](commit-status-publisher.md) feature to this build configuration.

After the integration is configured, TeamCity will detect merge requests submitted to the JetBrains Space target branches. For builds run on the requested changes, it will display the merge request details in the **Overview** tab:

<img src="pull-request-details.png" alt="Build Overview - Pull Request Details" width="706"/>

TeamCity will also send statuses per build change to timelines of merge requests in JetBrains Space (and to details of commits, if the [Commit Status Publisher](commit-status-publisher.md) feature is enabled in the build configuration):

<img src="space-timeline.png" alt="JetBrains Space - Merge Request Timeline" width="706"/>

To protect a target branch and ensure that only verified requests are merged into it, you can set up [Quality Gates](https://www.jetbrains.com/help/space/branch-and-merge-restrictions.html#quality-gates-for-merge-requests) for your Space repository and use a TeamCity build as an external check. If a build on a merge request fails, Space will notify you that the changes have not passed the required quality check.

<img src="space-quality-gates.png" alt="JetBrains Space - Quality Gates" width="706"/>

## New UI: Editing scope of agent pools
{product="tc"}

It is now possible to edit the scope of [agent pools](configuring-agent-pools.md) and quickly assign agents and projects to them right in the new Sakura UI — without switching to the classic UI mode.

>We keep reproducing the classic TeamCity features in its experimental UI, and a majority of them have already received a new implementation. If you haven't tried the new UI for a while or at all, we encourage you to give it a try this time. Our goal is to make the experimental UI not only as functional as the classic one but a lot more responsive and enhanced with easier navigation and widgets.

To edit an agent pool's scope, click **Assign agents** in its settings. In this dialog, you can choose what agents you want to assign to the pool:

<img src="edit-agent-pool.png" alt="Edit agent pool in new TeamCity UI" width="460"/>

Note that whenever you select a cloud image, you actually assign all its instances to the pool. If this pool has a limit of agent slots, each cloud instance will take a single slot, just like any regular agent.

Similarly, you can also associate projects with this pool: open the **Projects** tab of the pool's settings and click **Assign projects**. This way, agents assigned to this pool will be allowed to run builds only in the selected projects.

## Storing Docker images produced by build to public ECR registry
{product="tc"}

TeamCity can now store Docker images produced by a build to both private and — since this update — public ECR registries.

To be able to use this functionality, you need to add an [Amazon ECR connection](configuring-connections.md#Amazon+ECR) in __Project Settings__ and choose the _ECR Public_ registry type:

<img src="amazon-ecr-public.png" alt="Connecting to public ECR registry" width="750"/>

Remember to also enable [Docker Support](docker-support.md) in your builds.

## Automatic import of user avatars from external systems
{product="tc"}

When a user signs in to TeamCity [via a third-party account](configuring-authentication-settings.md), like GitHub or Bitbucket, for the first time, TeamCity will automatically fetch their avatar from the external system and attach it to their TeamCity user profile. Note that TeamCity will only be able to access avatars of users with verified emails (if you are using GitLab, check that a _public email_ is set in your account).

It is possible to upload a different avatar in the TeamCity user profile settings afterwards.

## Getting project SSH keys via UI
{product="tc"}

You can now copy the public part of an uploaded non-encrypted SSH key from the project settings. To do this, go to **Project Settings | SSH keys** and click **Copy the public key** under the key name.

<img src="copy-public-ssh-keys.png" alt="Copying public SSH keys" width="750"/>

This way, project admins no longer need to ask the system administrator for a public SSH key whenever they need it (for example, to integrate their TeamCity projects with a VCS hosting service) — they can just get it via the TeamCity UI.

## Applying action to multiple builds
{product="tc"}

It is now possible to select multiple builds and apply actions to all of them at once:
* Pin/unpin
* Tag
* Compare with another build
* Add a comment
* Add to favorites
* Remove

On the **Overview** tab of **Build Configuration Home**, you can select the required builds with checkboxes that appear when hovering over builds. To apply an action to them, use the respective command of the pop-up context menu. If you need to select a range of builds, press **Shift** and click build checkboxes at the edges of the range to be selected.

<img src="select-multiple-builds.png" alt="Selecting multiple builds" width="750"/>

## Kotlin DSL update: Import statements include only current DSL version
{product="tc"}

Since this version, all `import` statements in a [Kotlin DSL](kotlin-dsl.md) code no longer include the DSL version specification. Instead, the version is determined automatically based on the current server's version.  
This change affects how a DSL code appears in the _View as Code_ mode in TeamCity and removes irrelevant suggestions when writing `import` statements manually in an IDE. It concerns only newly created projects — the syntax of existing projects is supported for compatibility.

## Perforce integration update: Automatic creation of Helix Swarm test runs and status synchronization with builds
{product="tc"}

[Commit Status Publisher](commit-status-publisher.md) has a new option for sending build status reports to Perforce Helix Swarm — **Create Swarm Test**. If you enable it, TeamCity will [create a test run](https://www.perforce.com/manuals/swarm/Content/Swarm/swarm-apidoc_endpoint_integration_tests.html#Create_a__testrun_for_a_review_version) on the Helix Swarm server and update its status according to the build status in TeamCity.

## Other updates
{product="tc"}

* **Changing the project scope of an investigation or mute**  
  If a build problem occurs in multiple subprojects of the same project, it is convenient to assign its investigation (or mute it) within the whole parent project. However, users often forget to change the project scope in the _Investigation/Mute_ dialog, and TeamCity applies the action only within the current subproject. Now, project administrators can set a parent project as a preferred scope for all its subprojects. [Read how](investigating-and-muting-build-failures.md#Changing+Project+Scope+of+Investigation+or+Mute).
* **Build failure conditions: Creating a build problem per each matching error**  
When configuring a _Fail build on specific text in build log_ [failure condition](build-failure-conditions.md), you can now specify whether to create a build problem only for the first text occurrence found in a build log (default) or for each error that matches the specified pattern.
* **Composite build configurations are no longer displayed as suitable for agents**  
  [Composite build configurations](composite-build-configuration.md) no longer appear on the _Compatible Configurations_ tab of a [build agent's details](viewing-build-agent-details.md).  
  Composite builds are meta-builds designed for aggregating results of builds preceding them in a [chain](build-chain.md). Technically, they do not need a build agent to run and thus cannot be matched as compatible/incompatible with them. This usability update removes composite build configurations from _Compatible_ lists to help avoid any potential confusions.

## Other updates
{product="tcc"}

## Fixed issues
{product="tc"}

## Fixed issues
{product="tcc"}

## Upgrade notes
{product="tc"}

Before upgrading, we highly recommend reading about [important changes in version 2022.04 compared to 2021.2.x](upgrade-notes.md).

## Previous releases
{product="tc"}

* [What's New in TeamCity 2021.2](https://www.jetbrains.com/help/teamcity/2021.2/what-s-new-in-teamcity.html)
* [What's New in TeamCity 2021.1](https://www.jetbrains.com/help/teamcity/2021.1/what-s-new-in-teamcity.html)
* [What's New in TeamCity 2020.2](https://www.jetbrains.com/help/teamcity/2020.2/what-s-new-in-teamcity.html)
* [What's New in TeamCity 2020.1](https://www.jetbrains.com/help/teamcity/2020.1/what-s-new-in-teamcity.html)
* [What's New in TeamCity 2019.2](https://www.jetbrains.com/help/teamcity/2019.2/what-s-new-in-teamcity-2019-2.html)
* [What's New in TeamCity 2019.1](https://www.jetbrains.com/help/teamcity/2019.1/what-s-new-in-teamcity-2019-1.html)

## Roadmap

See the [TeamCity roadmap](https://www.jetbrains.com/teamcity/roadmap/#teamcity-roadmap) to learn about future updates.