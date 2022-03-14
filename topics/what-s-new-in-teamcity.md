[//]: # (title: What's New in TeamCity 2022.02)
[//]: # (auxiliary-id: What's New in TeamCity 2022.02;What's New in TeamCity)

TeamCity 2022.02 is still in production!

## Storing Docker images produced by build to public AWS registry

TeamCity can now store Docker images produced by a build to both private and — since this update — public AWS registries.

To be able to use this functionality, you need to add a new [Amazon ECR connection](configuring-connections.md#Amazon+ECR) in __Project Settings__ and choose the _ECR Public_ registry type:

<img src="amazon-ecr-public.png" alt="Connecting to public ECR registry" width="750"/>

Remember to also enable [Docker Support](docker-support.md) in your builds.

## Automatic import of user avatars from external systems

If a user signs in to TeamCity [via a third-party account](configuring-authentication-settings.md), like GitHub or Bitbucket, for the first time, TeamCity will automatically fetch their avatar from the external system. Note that TeamCity will only be able to access avatars of users with verified emails (if you are using GitLab, ensure that a _public email_ is set in your account).

It is possible to upload a different avatar in the TeamCity user profile settings afterwards.

## Getting project SSH keys via UI

You can now copy the public part of an uploaded non-encrypted SSH key from the project settings. To do this, go to **Project Settings | SSH keys** and click **Copy the public key** under the key name.

This way, you no longer need to ask the system administrator to provide a public SSH key whenever you need it (for example, for integration with a VCS hosting service) — just get it via the UI.

## Using TeamCity build as Bitbucket Merge Check

Thanks to more accurate [reporting of build statuses](commit-status-publisher.md) to Bitbucket, it is now possible to set TeamCity builds as [requirements](https://confluence.atlassian.com/bitbucketserver/checks-for-merging-pull-requests-776640039.html#Checksformergingpullrequests-Requiredbuildsmergecheck) in your Bitbucket repository. This allows you to protect a branch and ensure that only verified pull requests are merged into it.

## Applying actions to multiple builds at once

It is now possible to select multiple builds and apply actions to all of them at once:
* Pin/unpin
* Tag/untag
Add a comment
* Add to favorites
* Remove

On the **Overview** tab of **Build Configuration Home**, you can select the required builds with checkboxes that appear when hovering over builds. To apply an action to them, use the respective command of the pop-up context menu. If you need to select a range of builds, press **Shift** and click build checkboxes at the edges of the range to be selected.


## Other updates

* [Commit Status Publisher](commit-status-publisher.md) has a new option for build status reports to Perforce Helix Swarm — **Create Swarm Test**. If you enable it, TeamCity will [create a test run](https://www.perforce.com/manuals/swarm/Content/Swarm/swarm-apidoc_endpoint_integration_tests.html#Create_a__testrun_for_a_review_version) on the Helix Swarm server and update its status according to the build status in TeamCity.
* [Composite build configurations](composite-build-configuration.md) no longer appear on the _Compatible Configurations_ tab of a [build agent's details](viewing-build-agent-details.md).  
  Composite builds are meta-builds designed for aggregating results of builds preceding them in a [chain](build-chain.md). Technically, they do not need a build agent to run and thus cannot be matched as compatible/incompatible with them. This usability update removes composite build configurations from _Compatible_ lists to help avoid any potential confusions.
* 

## Fixed issues
{product="tcc"}

See [TeamCity Build 107913 release notes](teamcity-release-notes-build-107913.md).

## Upgrade notes

* Versions 2017.1 and 2017.2 of [TeamCity REST API](https://www.jetbrains.com/help/teamcity/rest/teamcity-rest-api-documentation.html) have been unbundled. If you have been using any of these versions in your scripts, consider switching to the latest protocol version as described [here](https://www.jetbrains.com/help/teamcity/rest/teamcity-rest-api-documentation.html#REST+API+Versions). If switching is not an option and this is a breaking change for your setup, please contact us via any convenient [feedback channel](feedback.md).
* Freemarker, used by TeamCity [notification templates](customizing-notification-templates.md), has been updated to version 2.3.31.

## Roadmap

See the [TeamCity roadmap](https://www.jetbrains.com/teamcity/roadmap/#teamcity-roadmap) to learn about future updates.