[//]: # (title: GitHub Checks Trigger)

## Common Information

The **GitHub Checks Trigger** runs a TeamCity build whenever a new change is pushed to a remote GitHub repository. In addition, this trigger posts detailed build result info to GitHub.

<img src="dk-checkstrigger-ghsummary.png" width="706" alt="Build summary info on GitHub"/>

If a processed changeset is a part of a Pull Request, this request shows the same build result info on its "Checks" tab.

<img src="dk-checksTrigger-pullrequest.png" width="706" alt="Build report in the Checks tab"/>

If you do not wish to expose your build server address, tick the **Disable TeamCity links in GitHub check run output** option in trigger settings. In this case build status messages will not publish links to TeamCity builds and build logs.

The trigger leverages [GitHub Checks API](https://docs.github.com/en/rest/checks?apiVersion=2022-11-28#check-runs) and can be used as the only commit verification mechanism, or compliment native [GitHub Actions](https://github.com/features/actions) workflows.

<img src="dk-checkstrigger-with-actions.png" width="706" alt="TC builds with GH actions"/>

## Limitations and Requirements

* The **GitHub checks trigger** handles webhooks received from GitHub. For that reason, it is compatible only with those build configurations that were created via a [GitHub App connection](configuring-connections.md#github-app).

    <img src="dk-githubchecks-from-connection.png" width="706" alt="Create a configuration via a GitHub App connection"/>
    
    The connection must have its **Support Webhooks** option enabled. In addition, the trigger requires the related GitHub App to have the `Checks: Read and write` permission and handle the `Check run` and `Check suite` events. In case you configure a new GitHub App TeamCity connection in "Automatic" mode, all required permissions and event handlers will already be enabled. Otherwise, if you configure a new connection in "Manual" mode or wish to update connections created prior to version 2024.07, modify your App settings on the GitHub side accordingly. You will also need to re-acquire an [auth token](git.md#Authentication+Settings) for the updated permissions to take effect.

* If a related VCS root has the [Use tags as branches](git.md#General+Settings) option enabled, the GitHub checks trigger does not automatically start new builds when a new [tagged release](https://docs.github.com/en/repositories/releasing-projects-on-github/managing-releases-in-a-repository#creating-a-release) is created.

* To prevent excessive build runs, if a configuration has both GitHub Checks and [standard VCS](configuring-vcs-triggers.md) triggers, only one TeamCity build is triggered by incoming changes. However, if no compatible agents are immediately available and a build spends some time in the queue (for example, waiting for a [cloud agent](agent-cloud-profile.md) to start), two separate builds might be triggered for the same changeset. For more details, see this YouTrack ticket: [TW-88928](https://youtrack.jetbrains.com/issue/TW-88928).



## Comparing Checks and VCS Triggers

In a "classic" TeamCity setup, the automatic changes building is configured as follows:

1. TeamCity collects new changes from a remote repository. This can happen during an automatic [polling](configuring-vcs-roots.md) or, if the configuration was created via an existing [GitHub App connection](configuring-connections.md#github-app), when GitHub sends a post-commit webhook to TeamCity.
2. A [VCS trigger](configuring-vcs-triggers.md) checks pending changes according to its own schedule, and produces a new build if one or multiple new commits are available. The trigger schedule protects TeamCity from spawning an excessive number of builds, which may happen in case of very high frequency commits. Instead, it provides a balanced approach where builds can process new commits in batches. For example, all commits that were pushed in the last five minutes will result a single TeamCity build.
3. If a [](commit-status-publisher.md) feature is configured, the build status is communicated back to GitHub.


Compared to this setup, the **Checks trigger** showcases the following major differences:

* It has no internal schedule and guarantees that each new push results in a new TeamCity build. This mechanism is preferable for cases when you need to spawn a build for all changes, no matter how frequently they occur.
* It does not require a [](commit-status-publisher.md) to publish Markdown-formatted status updates.



