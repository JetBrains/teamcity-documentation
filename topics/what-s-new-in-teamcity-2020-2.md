[//]: # (title: What's New in TeamCity 2020.2)
[//]: # (auxiliary-id: What's New in TeamCity 2020.2)

## New header

TeamCity 2020.2 comes with a more modern header in both classic and experimental UI.

<img src="header.png" alt="New header 2020.2"/>

## Python support out of the box

TeamCity 2020.2 comes with a new bundled [Python](python.md) runner. It automatically detects Python on build agents and allows running Python scripts on all supported platforms.

Comparing to the now obsolete [Python Runner](https://plugins.jetbrains.com/plugin/9042-python-runner) plugin, the new runner tightly integrates with TeamCity and introduces extra possibilities, such as:
* Automatic test reports (via unittest and pytest) and inspections (via flake8 and pylint)
* Using virtual environments
* Launching a build step inside a Docker container

<img src="python.png" alt="Bundled Python build runner"/>

If you were using the old plugin to run your Python projects, we highly recommend switching the respective build steps to the bundled runner. As the new runner offers extra settings, these steps have to be reconfigured manually.

Read [how to configure the new runner](python.md).

## Agentless build steps

Since this release, last steps of a build can finish without a build agent, in an external software.

Normally, a running build occupies a build agent until all its steps finish. In some cases, a build needs to wait for a process executed by some external system, outside of the build agent. For example, when the last step is responsible for deploying via a third-party software, an agent would simply do nothing until this step finishes.

Now, a build can be detached from its current agent. This agent becomes available to other builds, while the build continues running in the "agentless" mode. 

To detach an agent, a build should send the `##teamcity[buildDetachedFromAgent]` [service message](service-messages.md).     
There are dedicated REST API endpoints which can be used to report the progress of the agentless build and end it once the external process finishes. You can read more about them [in the documentation](detaching-build-from-agent.md).

There is a limit for a number of agentless builds running simultaneously. It is equal to the maximum number of authorized agents in your installation. This way, if you have 10 agent licenses, you can run in parallel up to 10 agents _plus_ up to 10 agentless builds.  
If the limit is exceeded, a build won't be able to detach from its agent until some of the running agentless builds finish.

## Authentication with GitHub.com, GitHub Enterprise, GitLab.com, GitLab CE/EE, Bitbucket Cloud

Now, users can authenticate in TeamCity with their external accounts in:
* [GitHub.com](http://github.com/) / [GitHub Enterprise](https://github.com/enterprise)
* [GitLab.com](https://about.gitlab.com/) / [GitLab CE/EE](https://about.gitlab.com/install/ce-or-ee/)
* [Bitbucket Cloud](https://bitbucket.org/product/)

This integration requires creating a dedicated application on the VCS hosting provider's side and a _connection_ in TeamCity. You can find detailed instructions on how to configure a connection [here](integrating-teamcity-with-vcs-hosting-services.md).   
To enable authentication with each of the listed providers, a system administrator needs to activate a respective [authentication module](authentication-modules.md). If any of these modules is enabled on your server, users will see its icon above the login form:

<img src="oauth.png" alt="Authentication with VCS hosting provider"/>

To sign in as an external user, you need to click this icon and, after the redirect, approve the TeamCity application in your external account. If successful, you will be signed in back to TeamCity.   
If a user with your external account's email is registered in TeamCity, you will be authenticated as this user. Otherwise, TeamCity will create a new user profile (unless this option is manually disabled).   
To be automatically recognized by TeamCity, users can also connect their profiles to external accounts in __My Settings & Tools__.

Admins can map TeamCity users with external accounts and limit providers' groups/organizations/workspaces whose members can access the server.

Learn [how to enable and manage authentication modules](configuring-authentication-settings.md).

## Support for Bitbucket Cloud pull requests

TeamCity already supports monitoring pull requests in GitHub, GitLab, Azure DevOps, and Bitbucket Server. And now — in Bitbucket Cloud.

<img src="bb-cloud-pr.png" alt="Bitbucket Cloud pull requests"/>

When you send a pull request to a Bitbucket Cloud repo, TeamCity will detect it, run a new build, and then display the pull request results. For this to work, you need to configure the [Pull Requests](pull-requests.md) build feature and a [VCS trigger](configuring-vcs-triggers.md).   
Note that this feature works a bit differently with Bitbucket Cloud than with other VCSs. Since Bitbucket Cloud does not create a dedicated branch per pull request, TeamCity runs builds on the source branches of pull requests and monitors only the source repository (forks are not supported).

See [more details](pull-requests.md#Bitbucket+Cloud+Pull+Requests).

## JetBrains Space support in Commit Status Publisher

The [Commit Status Publisher](commit-status-publisher.md) build feature can integrate with [JetBrains Space](https://www.jetbrains.com/space/). With this feature, TeamCity builds can automatically publish their statuses to a JetBrains Space project in real-time.

<img src="space-csp.png" alt="JetBrains Space support in Commit Status Publisher"/>

Read [how to configure this integration](commit-status-publisher.md#JetBrains+Space).

## Experimental UI updates

Our [experimental UI](teamcity-experimental-ui.md) is a work in progress: we introduce the changes in the early stages of development so you can already benefit from the new features. In each release, we polish already existing experimental pages and reproduce all the important classic features in the new UI. Our roadmap strongly depends on our users' feedback.   
In version 2020.2, we added the __Test History__ and __Build Queue__ pages, implemented the full-text search in the build log, and improved the __Dependencies__ tab of the build results.

If any of the familiar features are missing, you can switch an experimental page to the classic UI with the ![mammoth.png](mammoth.png) button, or turn off the experimental UI on the server completely in __My Settings & Tools__.

>__You can leave feedback about your experience with the experimental UI via [this survey](https://surveys.jetbrains.com/s3/feedback-form-for-teamcity?tcv=2020.2)!__

### Experimental Test History page

The highly demanded __Test History__ page gets a fresh look in this release. Now, you can see the detailed test statistics without switching to the classic mode.

<img src="exp-test-history.png" alt="Experimental Test History page"/>

### Full-text search in build log

You can now search through a build log regardless of how much of it is already loaded in the UI.

<img src="search-log.png" alt="Search in experimental build log"/>

## Updated Dependencies display

We received lots of requests to show queued builds in the experimental UI representation of a build chain timeline. In this release, we've updated the build chain display to make it more informative:
* The __Dependencies | Timeline__ view shows all dependencies of the current build, including queued ones.
* The __Dependencies | Chain__ view displays a full build chain. You can see all builds that depend on the current one and promote the current build to them, as in the classic UI. If a build is [composite](composite-build-configuration.md), you can also group it right from this view.

<img src="exp-buildchain.png" alt="Experimental build chain"/>

### Plugin support in Experimental UI

Now, you can write plugins for the experimental UI using modern web technologies and different frameworks. See [this blog post](https://blog.jetbrains.com/teamcity/2020/09/teamcity-2020-2-updated-plugin-development/) for more details.

### Experimental Build Queue page

We are actively working on the new representation of the build queue. __Since TeamCity 2020.2.2, the new queue is displayed by default.__ In earlier versions, you can switch to it by clicking the test-tube icon in the upper right corner of the screen.

The new sidebar is of great help to our users on the __Projects__ and __Agents__ pages. Now, it is available for the __Queue page__ as well, which is most helpful for big installations with many agent pools.   

You can also click any build in the queue to see its details:

<img src="exp-queue.png" alt="Experimental build queue"/>

## Customizable clean-up schedule

TeamCity server clean-up becomes more flexible with the support of [cron-like expressions](cron-expressions-in-teamcity.md). You can customize the schedule so the clean-up starts with any necessary regularity: for example, on weekends or twice a day.

<img src="clean-up-cron.png" alt="Cron expressions in clean-up"/>

Remember that too frequent clean-ups might extensively load the CPU, and too rare clean-ups take more time and might lead to garbage accumulation. We recommend keeping the clean-up schedule balanced. In most cases, a daily clean-up would be enough, but highly-loaded installations might require running clean-up more frequently.

## Time-limited access tokens

TeamCity can generate time-limited [access tokens](managing-your-user-account.md#Managing+Access+Tokens). You can use these tokens in scripts or other REST API requests to grant temporary access to the TeamCity server. After the token’s time limit expires, TeamCity will automatically revoke its access.

To add a new token, go to __My Settings & Tools | Access tokens__:

<img src="tmp-access-token.png" alt="Temporary access tokens"/>

## Editing project settings on secondary nodes
{product="tc"}

TeamCity 2020.2 adds a [new responsibility](multinode-setup.md#Processing+User+Requests+to+Modify+Data+on+Secondary+Node) for secondary nodes. If granted to a node, it allows UI actions: running, stopping, tagging, commenting builds, and much more. The [list of supported UI actions](multinode-setup.md#User-level+Actions+on+Secondary+Node) now also includes editing projects and build configurations (with a few limitations, such as editing cloud profiles).   
Disabling this responsibility will switch a secondary node to a read-only mode.

See also [upgrade notes](upgrade-notes.md#Changes+from+2020.1.x+to+2020.2).

## Monitoring disk usage in external storage
{product="tc"}

An increasing number of our users prefer storing build artifacts in cloud — for example, in Amazon S3. However, it was not previously possible to see what amount of data is stored there.

The [Disk Usage](disk-usage.md) report now supports external [artifact storage](configuring-artifacts-storage.md). It shows the amount of data stored in each configured storage or in all of them at once. Besides, if you had several artifact directories configured for the local storage, you can now also see this report separately for each directory.

On the __Administration | Disk Usage__ page, you can switch between the detected storages and see detailed reports:

<img src="disk-usage.png" alt="External storage in Disk Usage report"/>

## Identifying builds with custom settings

It is now easier to distinguish "custom" builds from regular ones. If a build was run with custom parameters or artifact dependencies, or not on the latest commit, TeamCity will visually mark such build. It will show the respective icon and hint opposite this build in the build list:

<img src="custom-build-hint.png" alt="Custom build hint"/>

and in the build results:

<img src="customized-params-build-results.png" alt="Custom build hint"/>

Click the link in the hint to open the related tab of build results.

>This functionality works both in the classic and experimental UI.

## Muting failed tests after successful retry

Some builds can retry tests if they fail. This is most convenient for _flaky tests_. Such tests can alternately fail and succeed when applied to the same source revision, and you might want to exclude their influence on the build status.   
Now, if _test retry_ is enabled in your build, TeamCity will mute a failed test if it eventually succeeds during the same build run. This test will not affect the build status, and the build will finish successfully given it has no other problems.

<img src="test-retry.png" alt="Muted by test retry"/>

See more details in the [documentation](build-failure-conditions.md#Common+build+failure+conditions).

## Other improvements

* __Source branch filter for pull requests__   
   TeamCity can filter pull requests not only by the target but by the source branch. It allows you to easily eliminate certain draft branches from the scope of monitoring.   
   Note that this filter is not applicable to Bitbucket Cloud as TeamCity monitors pull requests directly on their source branches.
* __Passing NuGet packages between build steps__  
If you need to publish NuGet packages and then use their contents within one build, you want to guarantee they are published and indexed on time — and not at the build finish. Previously, it required using a NuGet Publish step. Since this release, a build step can send the `##teamcity[publishNuGetPackage]` [service message](service-messages.md#Passing+NuGet+Packages+between+Steps) instead. This ensures the NuGet packages are published in all configured NuGet feeds right at the end of the current build step and are available in the following build steps.
* __Faster cold start of agent Docker containers__   
  Since this version, TeamCity agent Docker images are based on the full agent distribution. The full agent contains all bundled plugins. This significantly speeds up the agent cold start because the full agent doesn't need to synchronize all plugins with the server. When launched, it will only download external plugins or tools installed on your server, if any.   
  Please note that this improvement is effective only if the Docker image matches the server version.
* __Easier Perforce SSH root configuration__   
   If a VCS root of your project connects to Perforce via SSH, TeamCity will automatically establish a trusted connection with it. The `p4 trust` command is now sent every time you test a Perforce connection or a build agent checks out from Perforce.
* __Limiting the number of artifacts published per build__   
   This helps prevent memory consumption and performance problems if multiple builds publish many artifacts in parallel.   
   This number is set to 1000 in all new installations. On upgrade of an existing TeamCity installation, this number will stay unlimited. You can limit the value in __Administration | Server Administration | Global Settings__. Note that this limit does not consider hidden artifacts.
* __Execution timeout for composite builds__   
   When a [composite build](composite-build-configuration.md) comprises a build that cannot start (for example, it has no compatible agents), this composite build might run forever until it is terminated manually. Now, you can set an execution timeout for such build so it fails automatically after being unable to start for a long time.
* __Support for an S3 path prefix__   
   You can now set a path prefix when configuring an Amazon S3 artifact storage. This allows using the same S3 bucket for all TeamCity projects and configure prefix-based permissions.
* __Updated pull request icons__   
   Icons representing pull requests in build results are now larger and change color depending on the pull request state, helping you to identify its status quicker.
* __Health report about insecure Tomcat connection configuration__   
   If the server is installed behind a reverse proxy providing HTTPS access to the TeamCity server, TeamCity now checks if the `secure="true"` and `scheme="https"` attributes are present in the Tomcat connection. If these attributes are missing, TeamCity will display the respective health report.
* __No default Gradle build file value__   
   Previously, the _Build file_ field of the [Gradle](gradle.md) runner was set to `build.gradle` by default. We have removed this default value as some users rely on custom names of build files and prefer to let Gradle decide what file to choose.   
   If you use `build.gradle` as your build file, all will continue to work as before this update.
* __REST API updates__:
   * [VCS labels](https://www.jetbrains.com/help/teamcity/rest/manage-builds.html#VCS+Labels)
   * [Test statistics for personal builds](https://www.jetbrains.com/help/teamcity/rest/manage-tests-and-build-problems.html)
* The [.NET](net.md) build runner now supports earlier versions of Visual Studio and MSBuild. Currently supported versions are: Visual Studio 2010 or later, MSBuild 4 / 12 or later.
* To see all bundled tool updates, read our [upgrade notes](upgrade-notes.md#Changes+from+2020.1.x+to+2020.2).
{product="tc"}
* Version 2020.2 comes with ~30 performance fixes in various pieces of functionality (for example, in the Custom Run dialog).

## Fixed issues

See [TeamCity 2020.2 release notes](teamcity-2020-2-release-notes.md).

## Upgrade notes
{product="tc"}

Before upgrading, we highly recommend reading about important changes in [version 2020.2 comparing to 2020.1.x](upgrade-notes.md#Changes+from+2020.1.x+to+2020.2).

## Previous releases
* [What's New in TeamCity 2020.1](https://www.jetbrains.com/help/teamcity/2020.1/what-s-new-in-teamcity.html)
* [What's New in TeamCity 2019.2](https://www.jetbrains.com/help/teamcity/2019.2/what-s-new-in-teamcity-2019-2.html)
* [What's New in TeamCity 2019.1](https://www.jetbrains.com/help/teamcity/2019.1/what-s-new-in-teamcity-2019-1.html)

## Roadmap

See the [TeamCity roadmap](https://www.jetbrains.com/teamcity/roadmap/#teamcity-roadmap) to learn about future updates.


