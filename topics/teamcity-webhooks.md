[//]: # (title: TeamCity Webhooks)

Webhooks are automated HTTP-based messages sent from apps or services when a certain event occurs. Using webhooks, you can set up an event-driven communication between two APIs.

TeamCity can send payloads to the target URL when a new build starts, an agent unregisters, the server collects changes from a remote repository, and more.

> This article explains how to configure webhooks that notify third-party services about specific TeamCity events. To read about VCS webhooks that notify TeamCity about new repository changes, refer to this article instead: [](configuring-vcs-post-commit-hooks-for-teamcity.md).
> 
{style="note"}

## Enable Webhooks

1. Navigate to **Administration | &lt;Root project&gt; | Parameters**.

2. Click **Add new parameter** to create two [configuration parameters](configuring-build-parameters.md).

   <img src="dk-webhooks-setup.png" width="706" alt="Setup TeamCity Webhooks"/>

   * `teamcity.internal.webhooks.enable` — specifies whether webhooks are enabled. Set this parameter to `true`.
   * `teamcity.internal.webhooks.url` — stores the URL to which TeamCity should send payloads (via HTTP POST requests). For testing purposes, you can specify the URL provided by any real-time webhook testing service, such as [Webhook.site](https://webhook.site/) or [Beeceptor](https://beeceptor.com).
   
3. Specify the list of events that should trigger sending POST requests. To do this, create the `teamcity.internal.webhooks.events` configuration parameter for those TeamCity projects whose events you need to track.

   The list below enumerates available `teamcity.internal.webhooks.events` parameter values. Use a semicolon (`;`) as a separator for multiple values.

   <dl>
   <dt>AGENT_REGISTRED</dt>
   <dd>
   <b>Tracked Event:</b> A new build agent connected to the TeamCity server and obtained an <a href="configure-agent-installation.md#General+Agent+Configuration">authorization token</a>.<br/>
   <b>Parent Project:</b> &lt;Root project&gt; only<br/>
   <b>REST API Payload Schema:</b> <a href="https://www.jetbrains.com/help/teamcity/rest/agent.html#Schema">#/definitions/agent</a>
   </dd>
   
   <dt>AGENT_UNREGISTERED</dt>
   <dd>
   <b>Tracked Event:</b> A build agent stopped and disconnected from the server. This can happen when an agent software needs an upgrade or when you manually stop the agent service.<br/>
   <b>Parent Project:</b> &lt;Root project&gt; only<br/>
   <b>REST API Payload Schema:</b> <a href="https://www.jetbrains.com/help/teamcity/rest/agent.html#Schema">#/definitions/agent</a>
   </dd>
   
   <dt>AGENT_REMOVED</dt>
   <dd>
   <b>Tracked Event:</b> A build agent was removed.<br/>
   <b>Parent Project:</b> &lt;Root project&gt; only<br/>
   <b>REST API Payload Schema:</b> <a href="https://www.jetbrains.com/help/teamcity/rest/agent.html#Schema">#/definitions/agent</a>
   </dd>
   
   <dt>BUILD_STARTED</dt>
   <dd>
   <b>Tracked Event:</b> A build started. Follows the <code>BUILD_TYPE_ADDED_TO_QUEUE</code> and <code>CHANGES_LOADED</code> events.<br/>
   <b>Parent Project:</b> Any project.<br/>
   <b>REST API Payload Schema:</b> <a href="https://www.jetbrains.com/help/teamcity/rest/build.html#Schema">#/definitions/build</a>
   </dd>
   
   <dt>BUILD_FINISHED</dt>
   <dd>
   <b>Tracked Event:</b> A build finishes, regardless of whether it failed or was successful.<br/>
   <b>Parent Project:</b> Any project.<br/>
   <b>REST API Payload Schema:</b> <a href="https://www.jetbrains.com/help/teamcity/rest/build.html#Schema">#/definitions/build</a>
   </dd>
   
   <dt>BUILD_INTERRUPTED</dt>
   <dd>
   <b>Tracked Event:</b> A running build was canceled. Canceled builds do not trigger the <code>BUILD_FINISHED</code> event.<br/>
   <b>Parent Project:</b> Any project.<br/>
   <b>REST API Payload Schema:</b> <a href="https://www.jetbrains.com/help/teamcity/rest/build.html#Schema">#/definitions/build</a>
   </dd>
   
   <dt>CHANGES_LOADED</dt>
   <dd>
   <b>Tracked Event:</b> TeamCity successfully collected changes from a remote repository (or ensured no new changes are present) and is ready to execute build steps.<br/>
   <b>Parent Project:</b> Any project.<br/>
   <b>REST API Payload Schema:</b> <a href="https://www.jetbrains.com/help/teamcity/rest/build.html#Schema">#/definitions/build</a>
   </dd>
   
   <dt>BUILD_TYPE_ADDED_TO_QUEUE</dt>
   <dd>
   <b>Tracked Event:</b> A build was initiated and placed in the build queue.<br/>
   <b>Parent Project:</b> Any project.<br/>
   <b>REST API Payload Schema:</b> <a href="https://www.jetbrains.com/help/teamcity/rest/build.html#Schema">#/definitions/build</a>
   </dd>
   
   <dt>BUILD_PROBLEMS_CHANGED</dt>
   <dd>
   <b>Tracked Event:</b> The list of build problems changed (compared to the previous run of the same build configuration).<br/>
   <b>Parent Project:</b> Any project.<br/>
   <b>REST API Payload Schema:</b> <a href="https://www.jetbrains.com/help/teamcity/rest/build.html#Schema">#/definitions/build</a>
   </dd>
   </dl>


4. Perform an action that triggers tracked events (for instance, run a new build to trigger the `BUILD_TYPE_ADDED_TO_QUEUE` &gt; `CHANGES_LOADED` &gt; `BUILD_STARTED` &gt; `BUILD_FINISHED` chain) and ensure your target URL receives corresponding POST requests.


## Customize Request Payloads

By default, webhooks send requests with full [Agent](https://www.jetbrains.com/help/teamcity/rest/agent.html#Schema) or [Build](https://www.jetbrains.com/help/teamcity/rest/build.html#Schema) payloads. You can manually specify the fields that should be present in request payloads. To do so, add the `teamcity.internal.webhooks.{event_name}.fields` [configuration parameter](configuring-build-parameters.md) with `fields=field1,field2,object(field3)` as its value.

For example, the `teamcity.internal.webhooks.BUILD_INTERRUPTED.fields = fields=buildTypeId,number,canceledInfo(user(username))` parameter will display only a number of a canceled build, an ID of the corresponding build configuration, and a username of the person who canceled this build.

```JSON
{
   "eventType": "BUILD_INTERRUPTED",
   "payload": {
      "buildTypeId": "GhAppMaven_Build",
      "number": "43",
      "canceledInfo": {
         "user": {
            "username": "JohnDoe"
         }
      }
   }
}
```

## Authorization Settings

If the recipient API does not allow sending POST requests anonymously and requires authorization, specify the following additional parameters: 

* `teamcity.internal.webhooks.username` — the username written to the "php-auth-user" header.
* `teamcity.internal.webhooks.password` — the password written to the "php-auth-pw" header. To store this value securely and hide it from TeamCity UI and REST requests, click **Edit...** in the parameter settings dialog and select the "Password" type.

   <img src="dk-webhooks-password.png" width="706" alt="Password for Basic Auth"/>


## Resend Failed Requests

If a request was not delivered (an exception was thrown while sending a request, or a recipient response code was other than "2**"), TeamCity can try resending this message.  To do this, create the `teamcity.internal.webhooks.retry_count` parameter and set the number of retry attempts as its value. The default value is `0`.

## Parameter Inheritance

TeamCity projects inherit configuration parameters from their parent projects. For instance, if the *&lt;Root project&gt;* has the `teamcity.internal.webhooks.events=BUILD_STARTED;BUILD_FINISHED` parameter, every TeamCity project will send webhook messages when their builds start and finish.

If a parent and a child projects both have a parameter with the same name, the child project overrides the inherited value. For instance:

* *&lt;Root project&gt;* has the `teamcity.internal.webhooks.events=BUILD_STARTED;BUILD_FINISHED` parameter;
* *Project A* has the `teamcity.internal.webhooks.events=BUILD_INTERRUPTED` parameter.

In this case, all TeamCity projects report when their builds start and finish, while *Project A* reports only canceled builds. If you need *Project A* to report all three events, change its parameter value to `BUILD_STARTED;BUILD_FINISHED;BUILD_INTERRUPTED`.