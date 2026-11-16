[//]: # (title: Support and Troubleshooting)
[//]: # (help-id: Troubleshooting,Feedback)


When a problem occurs with TeamCity, we recommend first checking the [Common Problems](common-problems.md) and [Known Issues](known-issues.md) pages to see if there is existing guidance for the issue you are experiencing.

If you need additional help, we offer several convenient options:

* [Issue tracker](https://youtrack.jetbrains.com/issues/TW) (public, with an option for private): Log bugs or feature requests.

* [Support ticket](ticket-based-support.md) (private): Available only for customers with active Enterprise server [licenses](licensing-policy.md) to receive direct assistance from our support team.
  {instance="tc"}

* [Support ticket](ticket-based-support.md) (private)
  {instance="tcc"}

* [Community forum](https://jb.gg/teamcity-forum) (public): Engage in discussions with the wider user community.

These options are described in more detail below.


### Issue Tracker

Our [public issue tracker](https://youtrack.jetbrains.com/issues/TW) allows you to check if somebody has already reported your problem. If the same issue exists, please vote for it.

Although issues in our issue tracker are public by default, you do have an option to create a private issue, or add private comments/attachments to public issues. You can do this by setting the visibility only to the `teamcity-developers` group.

If you are certain you have encountered a bug, collect the relevant data about the problem and post a new issue in the issue tracker.

Feature requests can also be logged through our issue tracker.

### Support Ticket

Customers who own a TeamCity Enterprise server license with active maintenance can submit a ticket directly to our support team. We use Zendesk for our ticketing system, and tickets can be submitted through the [online support form](https://teamcity-support.jetbrains.com/hc/en-us/requests/new?ticket_form_id=66621) or by [email](mailto:teamcity-support@jetbrains.com).
{instance="tc"}

We use Zendesk for our ticketing system, and tickets can be submitted through the [online support form](https://teamcity-support.jetbrains.com/hc/en-us/requests/new?ticket_form_id=66621) or by [email](mailto:teamcity-support@jetbrains.com).
{instance="tcc"}

For additional details, such as our support team’s availability, optional support plans with defined SLAs, and handling of production outage situations, please refer to the [Ticket-Based Support](ticket-based-support.md) page.

#### Submit a Request with Diagnostic Data

You can start a support request directly from the TeamCity UI. TeamCity collects diagnostic data from the TeamCity server, uploads it to the [JetBrains Upload Service](reporting-issues.md#Uploading+Large+Data+Archives), and opens the Zendesk support form with your request details and an Upload ID that links the ticket to the uploaded data. You do not need to collect logs and thread dumps manually, and our support team can start investigating the issue sooner.

__Required permissions__

The **Submit support request** icon appears in the navigation sidebar only for users who have the global _Submit support requests with diagnostic data to JetBrains Support_ permission. The System Administrator role includes this permission by default. To allow other users to submit requests, add the permission to one of their roles as described in [Managing Roles](managing-roles-and-permissions.md#Managing+Roles).

When you submit a request from a project, build configuration, or build page, TeamCity also checks that you can view the data of that page. Users with the System Administrator role have all permissions in every project and need nothing else. Other users need the following permissions in the related project:

* _View project_ and _View build configuration settings_ — to submit a request from a project or build configuration page.
* _View build runtime data_ — additionally, to submit a request from a build results page.

If any of these permissions is missing, TeamCity rejects the request with an error before collecting any data. In this case, ask an administrator to grant the missing permissions, or submit the request from a page that is not related to a specific project, for example, the **Agents** page.

__Collected data__

The data TeamCity collects depends on the page you open the form from. When the **Collect session logs** checkbox is enabled, the following data is always collected:

* `teamcity-*` server log files. See [General Logging Description](teamcity-server-logs.md#General+Logging+Description) for details.
  {instance="tc"}
* `teamcity-*` server log files.
  {instance="tcc"}
* A series of server [thread dumps](reporting-issues.md#Server+Thread+Dump).
* Server environment details: operating system and architecture, Java/JVM details, database type and driver, licensing, and [multinode setup](multinode-setup.md) details.
  {instance="tc"}
* Server environment details: operating system and architecture, Java/JVM details, database type and driver, and licensing details.
  {instance="tcc"}

The following data is added based on the current page:

| Page | Additional data |
|------|-----------------|
| Project | Settings of the project |
| Build configuration | Settings of the build configuration and its parent project |
| Build results | Settings of the build configuration and its parent project, the full build log, and the build's [hidden artifacts](build-artifact.md#Hidden+Artifacts) (internal logs, properties, and settings) |

For example, if a specific build fails or behaves unexpectedly, open the results page of this build before submitting the request so that its build log is included. The form lists the data that will be collected from the current page under the **Collect session logs** checkbox.

__Submit the request__

1. If the issue concerns a specific project, build configuration, or build, open its page in TeamCity.
2. Click **Submit support request** in the navigation sidebar.
3. Fill in the **Title** and **Issue description** fields, and specify when the issue started or was first noticed.

   <img src="support-request-form.png" width="460" alt="Submit support request form"/>

4. Keep the **Collect session logs** checkbox enabled to attach diagnostic data to the request. If you clear it, TeamCity does not collect any data and opens the Zendesk support form with your request details right away.
5. Click **Proceed**. TeamCity collects the data and uploads it to JetBrains. This can take a few minutes. Keep the TeamCity page open until the upload completes. To stop the process, click **Cancel data collection** in the notification.

   <img src="support-request-collecting-logs.png" width="378" alt="Collecting session logs notification"/>

6. When the _Upload complete_ notification appears, click **Proceed to Zendesk**. The support form opens with your title and description, and the Upload ID is added to the ticket description. Do not remove the Upload ID: our support team uses it to match your ticket with the uploaded data. You can also copy the ID from the notification.

   <img src="support-request-upload-complete.png" width="378" alt="Upload complete notification"/>

7. Add any other details to the Zendesk form and submit the ticket.

The TeamCity server needs outbound HTTPS access to `uploads.jetbrains.com` to upload the data, and your browser needs access to `teamcity-support.jetbrains.com` to open the support form. If outbound traffic is restricted by a firewall or proxy, allow these hosts before submitting a request.
{instance="tc"}

### Community Forum

You can search the [TeamCity community forum](https://jb.gg/teamcity-forum) to see if anyone else has experienced your problem. Our forum's user base is quite active and is a good place to find support or start threads.

## General Guidance

When reporting an issue through one of the systems mentioned above, make sure to:
{instance="tc"}

* Provide all necessary information by reviewing the [guidelines on reporting issues](reporting-issues.md).
  {instance="tc"}

* Use a [recent TeamCity version](previous-releases-downloads.md). Please note that we generally do not provide regular support for major versions that are more than one year old (refer to the [Release Cycle](teamcity-release-cycle.md) page for more details).
  {instance="tc"}


When reporting an issue through one of the systems mentioned above, make sure to provide all necessary information by reviewing the [guidelines on reporting issues](reporting-issues.md).
{instance="tcc"}



## Data Security

We encourage making your posts publicly available so that other users can contribute based on their experience and benefit from the answers.

All data in the [JetBrains community forum](https://jb.gg/teamcity-forum) is public. Issues in the [JetBrains issue tracker](https://youtrack.jetbrains.com/issues/TW) are public by default; however, there is an option to create issues visible only to the JetBrains team. Additionally, private comments and attachments can be added to public issues.

If you need to share information that is not intended for public access, you can use the following methods:

* Post a comment or attach files to the issue in the [JetBrains issue tracker](https://youtrack.jetbrains.com/issues/TW), making them visible only to the `teamcity-developers` group.

* Attach the data to a support ticket. View the [Ticket-Based Support](ticket-based-support.md) page for contact details.

* For files exceeding 10MB, upload them to the [JetBrains Upload Service](reporting-issues.md#Uploading+Large+Data+Archives) and share the Upload ID in the related support case.

All data marked as non-public is used solely for investigating the related issue and is discarded afterward.

During transfer and storage, the data may reside on third-party servers, such as mail servers, Amazon AWS servers, Zendesk, and others.

For more details, please refer to the [JetBrains Privacy Policy](https://www.jetbrains.com/company/privacy.html).







<!--



When a problem with TeamCity occurs, there is a number of places to look for information:
* Check [Known Issues](known-issues.md) and [Common Problems](common-problems.md).
* Visit the [TeamCity forum](https://jb.gg/teamcity-forum) — you can search the forum to see if anyone else has experienced your problem. Our forum's user base is quite active and is a good place to find support or start threads.
* [TeamCity issue tracker](https://youtrack.jetbrains.com/issues/TW) — check if somebody has already reported your problem. If the same issue exists, please vote for the issue. If you are sure you have faced a bug, please [collect](reporting-issues.md) the relevant data about the problem and post a new issue to the tracker. Be sure to include the TeamCity version, describe where exactly you see the problem, and what actions were preceding it. If relevant, please describe your environment (OS, web server, TeamCity distribution used, how TeamCity is set up, and so on).
* [Contact us](troubleshooting.md) to report an issue or ask a question using the general guidelines described.
* If you own Enterprise TeamCity license and need to submit information that is not meant to be public, you can also contact the development team via [this online form](https://teamcity-support.jetbrains.com/hc/en-us/requests/new?ticket_form_id=66621){nullable="true"} or [feedback email](mailto:teamcity-support@jetbrains.com?subject=(build: )).

To speed up the resolution of your problem, make sure to:
* include the affected TeamCity version;
* include detailed exact error messages, logs, screenshots;
* mention all related postings on the topic.

-->

 <seealso>
        <category ref="troubleshooting">
            <a href="known-issues.md">Known Issues</a>
            <a href="reporting-issues.md">Reporting Issues</a>
            <a href="ticket-based-support.md">Ticket-Based Support</a>
        </category>
</seealso>