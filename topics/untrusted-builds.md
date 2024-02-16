[//]: # (title: Untrusted Builds)

The **Untrusted Builds** section allows you to set up the changes verification process that aims to prevent TeamCity from running malicious code authored by external users.

<chunk include-id="untrusted-builds-and-build-approval">

> Do not confuse the [](untrusted-builds.md) and [](build-approval.md) features.
> * **Untrusted Builds** provide an additional security layer that blocks potentially harmful changes authored by external users.
> * The **Build Approval** feature implements a confirmation mechanism that avoids mindless/excessive builds for certain configurations (configurations that deploy build results to public resources, configurations that require significant resources to run, and so on).
>
{type="tip"}

</chunk>

> Untrusted Builds currently support the following version control systems:
> * GitHub
> * GitLab
> 
{type="note"}

## Common Information

TeamCity can run malicious code when your [](vcs-root.md) targets a public repository whose settings allow external users to commit changes via pull requests, and either of the following is true:

* The [](pull-requests.md) feature is enabled and its settings allow building changes from all users (the pull requests filtering is set to "Everybody").
* The branch specification of the project's [](vcs-root.md) includes the pull request branches (for example, `refs/pull/*` for GitHub) and the [VCS Trigger](configuring-vcs-triggers.md) automatically starts new builds when new changes are detected.

In this case external users can fork your public repository, introduce malicious changes and send a pull (merge) request. TeamCity will detect this request and trigger a new build with these changes.

> See this section for more information about potential damage caused by users who can modify repository code: [](security-notes.md#manage-permissions).
>
{type="warning"}

Depending on the exact build configuration setup and the desired behavior, you may want to make the following adjustments:

* To prevent TeamCity from building any requests, remove the [](pull-requests.md) feature or branch specification rules that target pull request branches.
* To ignore requests sent by external users, set up filtering by authors in the **Pull Requests** feature's settings.
* To postpone building external users' requests until these changes are verified by your trusted reviewers, use **Untrusted Builds** settings.


## Configure Untrusted Build Settings


## Kotlin DSL

The following [](kotlin-dsl.md) snippet illustrates how to configure Untrusted Builds in code.

```Kotlin

```