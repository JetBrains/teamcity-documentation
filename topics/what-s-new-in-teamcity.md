[//]: # (title: What's New in TeamCity 2023.11)
[//]: # (auxiliary-id: What's New in TeamCity 2023.11;What's New in TeamCity)


## Agents with Bundled JDKs

Starting with version 2023.11, you can build distributions of TeamCity agents bundled with custom JDKs. These distributions allow you to install both an agent and a JDK it requires to operate in one go.

<img src="dk-fullAgentDistributions.png" width="706" alt="Full agent distributions page"/>

To create a custom agent distribution, navigate to **Administration | Agent JDKs** and add a new JDK option (you will need to specify the platform, the architecture, and a link for TeamCity to download this specific JDK).

<img src="dk-addAgentJDK.png" alt="Add Agent JDK" width="706"/>

When a new option is added, TeamCity will start building your custom agent distribution. You can download custom agent+JDK bundles by clicking **Install agent | Full distributions** on the **Agents | Overview** page.

Learn more: [](install-teamcity-agent.md).



## Seamless GitHub App Registration

In version 2023.05, we introduced the [new type](configuring-connections.md#GitHub) of connections to GitHub and GitHub Enterprise. These connections utilize [GitHub Apps](https://docs.github.com/en/apps/using-github-apps/about-using-github-apps), instead of the traditional OAuth-based access to repositories.

Starting with version 2023.11, you will be able to create these connections much faster, without manually setting up and registering new apps in GitHub. Click the corresponding button in the connection description, and TeamCity will set everything up for you.

<img src="dk-GhAppManifestButton.png" width="706" alt="GitHub Manifest App Button"/>

Learn more: [Configuring Connections](configuring-connections.md#GitHub).


## AWS Connection Improvements

Starting with this version, you can enable or disable the **Available for sub-projects** and **Available for build steps** settings in AWS connection settings. These options allow you to ensure the configured connections are not used by unauthorized TeamCity projects and their [](aws-credentials.md) build features.

<img src="dk-shareAwsConnections.png" width="706" alt="Share AWS connections"/>

In addition, you can now refer to the new section of our "Configuring Connections" documentation article to learn how to configure secure AWS connections that follow Amazon guidelines and do not require permanent user credentials: [](configuring-connections.md#Recommended+Setup).
{product="tc"}

Learn more: [](configuring-connections.md#AmazonWebServices).



## Miscellaneous


* You can now bookmark required agent pools to easily access them from the top of the agents and pools list. Learn more: [](configuring-agent-pools.md#Favorite+Pools).
* <include src="parallel-tests.md" include-id="alternative-dotnet-parallel-filtering-tcc"/>

## Upgrade Notes
{product="tc"}

Before upgrading, we highly recommend reading about important changes in version [2023.11 compared to 2023.05.4](upgrade-notes.md#Changes+from+2023.05.4+to+2023.11).

## Fixed Issues
{product="tc"}

See [TeamCity 2023.11 release notes](teamcity-2023-11-release-notes.md).



## Roadmap

See the [TeamCity roadmap](https://www.jetbrains.com/teamcity/roadmap/#teamcity-roadmap) to learn about future updates.

## We'd Love to Hear From You


We place a high value on your feedback and encourage you to share your thoughts and suggestions. See this link for more information: [Feedback](feedback.md).
