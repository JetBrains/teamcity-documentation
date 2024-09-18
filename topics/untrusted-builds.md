[//]: # (title: Untrusted Builds)

The **Untrusted Builds** section allows you to set up the changes verification process that aims to prevent TeamCity from running malicious code authored by external users.

<img src="dk-untrustedbuilds-pending.png" width="706" alt="Pending approval"/>

<snippet include-id="untrusted-builds-and-build-approval">

> Do not confuse the [](untrusted-builds.md) and [](build-approval.md) features. Despite certain similarities, these are separate features that serve different purposes.
> 
> * **Untrusted Builds** provide an additional security layer that blocks potentially harmful changes authored by external users.
> * The **Build Approval** feature implements a confirmation mechanism that avoids mindless/excessive builds for certain configurations (configurations that deploy build results to public resources, configurations that require significant resources to run, and so on).
>
{style="tip"}

</snippet>

> Untrusted Builds currently support the following version control systems:
> * GitHub
> * GitLab
> 
{style="note"}

## Common Information

TeamCity can run malicious code when your [](vcs-root.md) targets a public repository whose settings allow external users to commit changes via pull requests, the [VCS Trigger](configuring-vcs-triggers.md) automatically starts new builds when new changes are detected, and either of the following is true:

* The [](pull-requests.md) feature is enabled and its settings allow building changes from all users (the pull requests filtering is set to "Everybody").
* The branch specification of the project's [](vcs-root.md) includes the pull request branches (for example, `refs/pull/*` for GitHub).

In this case external users can fork your public repository, introduce malicious changes and send a pull (merge) request. TeamCity will detect this request and trigger a new build with these changes.

> See this section for more information about potential damage caused by users who can modify repository code: [](security-notes.md#manage-permissions).
>
{style="warning"}

Depending on the exact build configuration setup and the desired behavior, you may want to make the following adjustments:

* To prevent TeamCity from automatically building pull request branches, modify branch specifications of your [VCS Triggers](configuring-vcs-triggers.md). This solution does not completely mitigate security risks since TeamCity users who have permissions to trigger builds can start them manually without a proper code review.

* To ignore requests sent by external users, set up filtering by authors in the **Pull Requests** feature's settings. This option completely cuts off external users' requests, making their branches inaccessible from TeamCity.

* To postpone building external users' requests until they pass a mandatory verification by your trusted reviewers, configure **Untrusted Builds** settings. This configuration does not depend on your trigger settings and requires verification for builds started both automatically (by a trigger) and manually (by a TeamCity user).


## Configure Untrusted Builds Settings

Settings that correspond to untrusted builds are configured on the project level, meaning they affect all build configurations owned by this project and its subprojects.

1. Navigate to project settings (**Administration | &lt;Your_Project&gt;**) and switch to the **Untrusted builds** section in the sidebar.

   <img src="dk-untrusted-builds-common.png" width="706" alt="Untrusted build settings"/>

2. Choose the **Default action**.

   * *Do nothing* — builds that process changes authored by external users do not require additional authorization to start.
   * *Cancel build* — TeamCity cancels builds that process changes authored by external users. This includes both builds initiated by the [](pull-requests.md) feature and manually started builds.
   * *Require approval* — builds that process changes authored by external users are queued, but will not start until the required number of reviewers approve it.

3. Choose whether untrusted builds should be logged. TeamCity warns you the build is untrusted in the [build log](build-log.md)... {product="tc"}

   <img src="dk-untrustedbuilds-log.png" width="706" alt="Warning in build log"/>

   ...as well as writes corresponding messages to the [teamcity-server.log](teamcity-server-logs.md) (regardless of the selected default action).

   ```Plain Text
   [2024-02-21 11:31:05,337]
   WARN — jetbrains.buildServer.SERVER — Build(promotion id: 7004, configuration id: MyProject_Build) detected as untrusted.
   Reasons: {Pull request from a fork in a public repository (target repository url: https://github.com/...)}
   Build URL: http://localhost:8111/buildConfiguration/MyProject_Build/-1
   ```
   
3. Choose whether untrusted builds should be logged. If this option is enabled and the **Default action** is not set to **Cancel build**, TeamCity adds a corresponding note to a [build log](build-log.md) when running an untrusted build. {product="tcc"}
   
   <img src="dk-untrustedbuilds-log.png" width="706" alt="Warning in build log"/>

4. Use the **Approval rules** field to appoint users who should review incoming changes and either approve or block corresponding builds. Use the `user:<username>` syntax to appoint individual users or add all trusted reviewers to a dedicated [user group](creating-and-managing-user-groups.md) and use the `group:<group key>:<count>` syntax. The `count` is a number of votes required to allow a build.

5. Set up the **Timeout** (in minutes) to automatically cancel queued builds that remain unverified longer than this threshold.

6. If the **Approve manually started builds** setting is on, builds initiated by a reviewer (a person added to **Approval rules**) automatically get the approval vote from that person. Note that in case new builds should be validated by a user group, other people should cast their remaining votes to start this build.

## Kotlin DSL

The following [](kotlin-dsl.md) snippet illustrates how to configure Untrusted Builds in code.

```Kotlin
project {
    features {
        untrustedBuildsSettings {
            id = "PROJECT_EXT_42"
            defaultAction = UntrustedBuildsSettings.DefaultAction.APPROVE
            enableLog = true
            approvalRules = "group:CODE_REVIEWERS:2"
            timeoutMinutes = 120
        }
    }
}
```