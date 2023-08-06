[//]: # (title: What's New in TeamCity 2023.09)
[//]: # (auxiliary-id: What's New in TeamCity 2023.09;What's New in TeamCity)


## Agents with Bundled JDKs

Starting with version 2023.09, you can build distributions of TeamCity agents bundled with custom JDKs. These distributions allow you to install both an agent and a JDK it requires to operate in one go.

<img src="dk-fullAgentDistributions.png" width="706" alt="Full agent distributions page"/>

To create a custom agent distribution, navigate to **Administration | Agent JDKs** and add a new JDK option (you will need to specify the platform, the architecture, and a link for TeamCity to download this specific JDK).

<img src="dk-addAgentJDK.png" alt="Add Agent JDK" width="706"/>

When a new option is added, TeamCity will start building your custom agent distribution. You can download custom agent+JDK bundles by clicking **Install agent | Full distributions** on the **Agents | Overview** page.

Learn more: [](install-teamcity-agent.md).

## Seamless GitHub App Registration

In version 2023.05, we introduced the [new type](configuring-connections.md#GitHub) of connections to GitHub and GitHub Enterprise. These connections utilize [GitHub Apps](https://docs.github.com/en/apps/using-github-apps/about-using-github-apps), instead of the traditional OAuth-based access to repositories.

Starting with version 2023.09, you will be able to create these connections much faster, without manually setting up and registering new apps in GitHub. Click the corresponding button in the connection description, and TeamCity will set everything up for you.

<img src="dk-GhAppManifestButton.png" width="706" alt="GitHub Manifest App Button"/>

Learn more: [Configuring Connections](configuring-connections.md#GitHub).


## Enhanced Integration with Perforce Helix Swarm

In version 2023.09, we have overhauled the "Perforce Helix Swarm" publisher of the [](commit-status-publisher.md) build feature. TeamCity can now utilize workflows and tests that already exist in your Swarm setup (instead of creating its own tests). In addition, the Publisher no longer requires credentials of a user with administrator access.

<img src="dk-swarm-personalbuild.png" width="706" alt="Personal build in TeamCity"/>

Learn more: [](integrating-with-helix-swarm.md).

## Miscellaneous

* Dependent builds whose ["On failed dependency" condition](snapshot-dependencies.md) is set to "Make build failed to start" no longer wait for an available agent when their dependencies fail or are cancelled. Instead, the dependent build's status changes to "Failed to start" as soon as possible, and TeamCity proceeds to the next build in chain.

## Upgrade Notes
{product="tc"}

Upgrade notes are not published for TCC.

## Fixed Issues
{product="tc"}

Release notes are not published for TCC.



## Roadmap

See the [TeamCity roadmap](https://www.jetbrains.com/teamcity/roadmap/#teamcity-roadmap) to learn about future updates.

## We'd Love to Hear From You


We place a high value on your feedback and encourage you to share your thoughts and suggestions. See this link for more information: [Feedback](feedback.md).
