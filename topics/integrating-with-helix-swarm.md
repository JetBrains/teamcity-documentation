[//]: # (title: Integration with Perforce Helix Swarm)


[Perforce Helix Swarm](https://www.perforce.com/products/helix-swarm) is a code review tool for Helix Core. When a developer shelves a file and asks for a review, TeamCity can run a build for this change and post the result as a [Swarm tests](https://www.perforce.com/manuals/swarm/Content/Swarm/basics_tests.html)  as well as in the comments section of a Helix Swarm review.


## Prerequisites

An integration between TeamCity and Helix Swarm is based on the [](commit-status-publisher.md) build feature. The Publisher utilizes [Swarm tests](https://www.perforce.com/manuals/swarm/Content/Swarm/basics_tests.html) that solve two tasks:

* track whenever a Helix Swarm review is created or edited, and send TeamCity a request to start a new build when this happens;
* post statuses of TeamCity builds back to Helix Swarm reviews.

Depending on whether your Helix Swarm setup already has tests that launch TeamCity builds when users create or edit reviews, you can choose one of the two options:

* Allow TeamCity to create new Swarm tests. Use this approach when you don't have existing Helix Swarm workflows and tests. This option is available only when you pass admin user credentials to the Commit Status Publisher. If you intend to opt for this option, skip to the [](#Set+Up+a+Commit+Status+Publisher) section as you do not need any additional setup on the Swarm side.

* Force TeamCity to find and utilize existing workflows and tests. This approach does not require admin user credentials since TeamCity does not create any Swarm entities. However, you will need to manually set up these entities on the Swarm side.

## Helix Swarm Setup

Steps described in this section are performed in Helix Swarm UI, and required only if TeamCity utilizes credentials of a non-admin user to post new Swarm comments.

### Create a Test

Helix Swarm [Tests](https://www.perforce.com/manuals/swarm/Content/Swarm/basics_tests.html) can send requests to TeamCity REST API endpoints when a review is created or edited.

<img src="dk-swarm-test.png" width="706" alt="Test in Helix Swarm"/>

1. Click **Tests** in the side navigation pane.
2. Click the **Add Test Definition** button.
3. In a new browser tab, go to TeamCity UI and navigate to **Administration | &lt;Your_Build_Configuration&gt;**.
4. Copy the **Build Configuration ID** value from TeamCity.
5. Go back to Swarm test settings and paste the copied value as the test name.

   > Note that Helix Swarm has a limit of 32 characters for test names. If your build configuration ID exceeds this limit, change it in TeamCity build configuration settings.
   > 
   {style="note"}

6. In the **URL** field, enter the following value:
   ```Plain Text
   <TeamCity_server_URL>/app/perforce/runBuildForShelve
   ```
   This value is a TeamCity REST API endpoint that Helix Swarm will use to communicate with your build configuration.

7. Enter the following string in the **Body** field:
   
   ```Plain Text
   buildTypeId=<X>&vcsRootId=<Y>&shelvedChangelist={change}&swarmUpdateUrl={update}
   ```

   * `X` — same value as the test name. Copy from the **Build Configuration ID** in TeamCity.
   * `Y` — in TeamCity, go to **Administration | &lt;Your_Build_Configuration&gt; | Version Control Settings | &lt;Your_VCS_Root&gt;** and copy the **VCS Root ID** value.
   * `{change}` — the [change number](https://www.perforce.com/manuals/swarm/Content/Swarm/test-add.html).
   * `{update}` — the [update callback URL](https://www.perforce.com/manuals/swarm/Content/Swarm/test-add.html).
     
   This string allows Helix Swarm tests to locate a specific TeamCity configuration and schedule a new build for it. For example:
   
   ```Plain Text
   buildTypeId=P4-Remote_MyBuildConfig&vcsRootId=MainP4Root&shelvedChangelist={change}&swarmUpdateUrl={update}
   ```

8. To authorize Swarm's requests to TeamCity, click **Add header** and specify the following values:

   * **Header** — "Authorization"
   * **Value** — "Bearer ABC", where ABC is a [TeamCity user access token](configuring-your-user-profile.md#Managing+Access+Tokens).

9. Set the **Timeout** setting to 10 seconds.

10. Click **Save** to save your new test.


### Set Up a Workflow

Helix Swarm [Workflows](https://www.perforce.com/manuals/swarm/Content/Swarm/workflow.add.html) run tests when a users create or edit Swarm reviews.

<img src="dk-swarm-workflow.png" width="706" alt="Swarm Workflow Settings"/>

1. Click **Workflows** in the side navigation pane.
2. Create a new workflow or edit the **Global Workflow**.
3. In the workflow settings, click **Add Test** and choose the test you created in the previous step.
4. Choose the **On Update** trigger type.
   
   >To get notified about the events, make sure to [configure Swarm triggers](https://www.perforce.com/manuals/swarm-admin/Content/Swarm/setup.perforce.html).
   >
   {style="tip"}

5. Leave the **Blocks** value as "Nothing".
6. Click **Save** to exit workflow settings.

### Set Up a Project

Helix Swarm [Project](https://www.perforce.com/manuals/swarm/Content/Swarm/chapter.projects.html) is a group of Helix Core Server users who are working together on a specific codebase.

1. Click **Projects** in the side navigation pane.
2. Create a new project or edit an existing one.
3. Set the **Workflow** value to the workflow you created in the previous step. If your test is used in the Global Workflow, choose the "No Workflow" option.

## Set Up a Commit Status Publisher

1. In TeamCity UI, navigate to **Administration | &lt;Your_Build_Configuration&gt;**.
2. Switch to the **Build Features** tab.
3. Click **Add build feature** and choose [](commit-status-publisher.md).
4. Choose **Perforce Helix Swarm** as a publisher.
5. Enter the URL of your Helix Swarm instance.
6. Enter username and [ticket](https://www.perforce.com/manuals/p4sag/Content/P4SAG/superuser.basic.auth.tickets.html). A user whose credentials you specify must have permissions to post comments to Helix Swarm reviews. In addition, make sure that the user's ticket does not expire to prevent the Commit Status Publisher from being unable to access your Swarm instance.

7. If you do not have any existing workflows and tests that trigger new TeamCity builds, and you want TeamCity to automatically create them when needed, check the **Create Swarm Tests** option. This setup requires that you pass administrator username and ticket in step 6.
   
   If you already have a workflow that a Commit Status Publisher can use (or you choose to create one to avoid passing administrator credentials to the build feature), leave this checkbox empty.

8. Commit Status Publisher can notify Swarm users about the current TeamCity build status by posting build results (success/fail) under the review's **Comments** tab and by updating the **Tests** section.

   <img src="dk-swarm-comments.png" width="706" alt="TeamCity comments in Swarm"/>

   <img src="dk-swarm-testInReview.png" width="706" alt="Tests section of a review"/>

   Use the **Code Review Comments** checkbox to specify whether the Commit Status Publisher should post these comments to a corresponding Swarm review tab.

9. Click **Test Connection** to verify the Commit Status Publisher can access your Swarm instance. If you are getting the "Provided credentials lack admin permissions" error, ensure you are not trying to enable the **Create Swarm Tests** option for non-admin user credentials.

10. Click **Save** to add the build feature to your configuration.



## Integration Features

After you have set up the Commit Status Publisher, modify and shelve any depot file. Open this shelved change in Helix Swarm and click **Request Review**. This will trigger a Helix Swarm test that will send TeamCity a request to start a new build. If your integration uses manually configured workflows and tests, a review shows the **Tests** section that indicates the current test/build status.

<img src="dk-swarm-testInReview.png" width="706" alt="Tests section of a review"/>

TeamCity attempts to find a user with the same username as a person who requested a review in Helix Swarm, and starts a new personal build for this user. You can click links in the build's **Swarm Reviews** section to open the shelved change and the related Swarm review.

<img src="dk-swarm-personalbuild.png" width="706" alt="Personal build in TeamCity"/>

You can also view the related change from the build's **Changes** tab:

<img src="dk-swarm-changes-tab.png" width="706" alt="Open Swarm changes from TeamCity"/>

When the TeamCity build finishes, Commit Status Publisher can announce the result as a new comment under the **Comments** section of a Helix Swarm review.

<img src="dk-swarm-comments.png" width="706" alt="TeamCity comments in Swarm"/>

You can uncheck the **Create Swarm Tests** option in the Commit Status Publisher's settings dialog to disable these comments. In this case the build feature will only update the **Tests** section of a Swarm review.
