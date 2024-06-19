[//]: # (title: GitHub Checks Trigger)

## Common Information

The **GitHub Checks Trigger** runs a TeamCity build whenever a new commit is pushed to a remote GitHub repository. In addition, this trigger posts detailed build result info to GitHub.

<img src="dk-checkstrigger-ghsummary.png" width="706" alt="Build summary info on GitHub"/>

If a processed commit is a part of a Pull Request, this request shows the same build result info on its "Checks" tab.

<img src="dk-checksTrigger-pullrequest.png" width="706" alt="Build report in the Checks tab"/>

The trigger leverages [GitHub Checks API](https://docs.github.com/en/rest/checks?apiVersion=2022-11-28#check-runs) and can be used as the only commit verification mechanism, or compliment native [GitHub Actions](https://github.com/features/actions) workflows.

<img src="dk-checkstrigger-with-actions.png" width="706" alt="TC builds with GH actions"/>

## Limitations and Requirements

The **GitHub checks trigger** handles webhooks received from GitHub. For that reason, it is compatible only with those build configurations that were created via a [GitHub App connection](configuring-connections.md#github-app).

<img src="dk-githubchecks-from-connection.png" width="706" alt="Create a configuration via a GitHub App connection"/>

The connection must have its **Support Webhooks** option enabled. In addition, the trigger requires the related GitHub App to have the "Checks: Read and write" permission and handle the "Check run" and "Check suite" events. In case you configure a new GitHub App TeamCity connection in "Automatic" mode, all required permissions and event handlers will already be enabled. Otherwise, if you configure a new connection in "Manual" mode or wish to update connections created prior to version 2024.07, modify your App settings on the GitHub side accordingly.

## Comparing Checks and VCS Triggers

In a "classic" TeamCity setup, the automatic changes building is configured as follows:

1. TeamCity detects new commits in a remote repository. This can happen during an automatic [polling](configuring-vcs-roots.md) or, if the configuration was created via an existing [GitHub App connection](configuring-connections.md#github-app), when GitHub sends a post-commit webhook to TeamCity.
2. A [VCS trigger](configuring-vcs-triggers.md) checks pending changes according to its schedule, and produces a new build if one or multiple new commits are available. The trigger schedule protects TeamCity from spawning an excessive number of builds, which may happen in case of very high frequency commits. Instead, it provides a balanced approach with new commits handled in batches. For example, all commits that were pushed in the last five minutes will result a single TeamCity build.
3. If a [](commit-status-publisher.md) feature is configured, the build status is communicated back to GitHub.


Compared to this setup, the **Checks trigger** showcases the following major differences:

* It has no internal schedule and guarantees that each new commit results in a new TeamCity build. This mechanism is preferable for cases when you need to spawn a build for all changes, no matter how frequently they occur.
* It does not require a [](commit-status-publisher.md) to publish Markdown-formatted status updates.



