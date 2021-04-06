[//]: # (title: Detaching Build from Agent)
[//]: # (auxiliary-id: Detaching Build from Agent)

If a final step of a build triggers some external service and the build does not require a [build agent](build-agent.md) anymore, the respective runner can detach the build from the agent. This makes this agent available to other builds. The build then continues running on the TeamCity server, and the external service reports its progress directly to the server. Such build steps are called [_agentless steps_](agentless-build-step.md).

## Releasing build agent

To release its current build agent, a runner needs to send the `##teamcity[buildDetachedFromAgent]` [service message](service-messages.md). After receiving this message, the agent skips all the following steps of the build, unless they have the "[_Always, even if build stop command was issued_](configuring-build-steps.md#Execution+policy)" execution policy enabled. If necessary, you can enable it for mandatory final steps — the agent will be released only after completing them.

If the [limit of agentless builds](#Agentless+builds%27+licensing) is not exceeded, the server releases the agent and it instantly becomes available to other builds. Otherwise, the agent stays attached to the build until some of the running agentless builds finish. 

>We highly recommend releasing the agent only during the last build step. Make sure the tasks performed outside TeamCity do not require the build agent.

## Logging build data

During agentless steps, the external tool should report all build status information and send any other types of requests directly to the TeamCity server via [REST API](https://www.jetbrains.com/help/teamcity/rest/teamcity-rest-api-documentation.html).

To perform a request, it needs to provide:
* [build-level authentication](artifact-dependencies.md#Build-level+authentication) credentials specified as [build system properties](configuring-build-parameters.md):
   * username: `%system.teamcity.auth.userId%`
   * password: `%system.teamcity.auth.password%`
* [build ID](working-with-build-results.md#Internal+Build+ID): `%teamcity.build.id%`
* TeamCity server URL: `%teamcity.serverUrl%`

An agent should send these parameters to the external software in advance, before being released.

### Logging messages

To log messages, use the following call:

```shell script
POST /app/rest/builds/id:<build_id>/log 
(curl -v --basic --user <username>:<password> --request POST http://<teamcity.url>/app/rest/builds/id:<build_id>/log --data <message> --header "Content-Type: text/plain")
```

Here, you can send any [service message](service-messages.md) as `<message>`.

An example request to send a warning:

```shell script
POST /app/rest/builds/id:TestBuild/log 
(curl -v --basic --user TeamCityBuildId=87065:lqmT22NStn4ulqmT22NStn4ulqmT22NStn4u --request POST http://localhost:8111/app/rest/builds/id:TestBuild/log --data "##teamcity[message text='Deployment failed' errorDetails='stack trace' status='ERROR']" --header "Content-Type: text/plain")
```

>To structure service messages in a build log, use [_flow tracking_](service-messages.md#Message+FlowId).

### Finishing build

It is important to ensure that a build, executed externally, delivers a finishing request to the TeamCity server. If the server is temporarily unavailable and cannot receive this request, the external tool should retry until this operation is successful. Without the finishing request, the build will be running on the TeamCity server indefinitely, until reaching its specified timeout, if any.

To finish a build, use the following call:

```shell script
PUT /app/rest/builds/id:<build_id>/finish
(curl -v --basic --user <username>:<password> --request PUT http://<teamcity.url>/app/rest/builds/id:<build_id>/finish)
```

Alternatively, you can finish it by sending the exact finish date as a non-empty string in the `yyyyMMdd'T'HHmmssZ` format:

```shell script
PUT /app/rest/builds/id:<build_id>/finishDate
(curl -v --basic --user <username>:<password> --request PUT http://<teamcity.url>/app/rest/builds/id:<build_id>/finishDate --data "20201231T235959+0000" --header "Content-Type: text/plain")
```

>If you need to end a build as "Failed", log the `buildProblem` message, as described [here](service-messages.md#Reporting+Build+Problems).

<anchor name="DetachingBuildfromAgent-agentless-licensing"/>

## Agentless builds' licensing

The number of builds that can simultaneously run without an agent is limited by the number of your active [agent licenses](licensing-policy.md#Number+of+Agents). For example, if you have 10 agent licenses, you can run in parallel up to 10 regular builds on agents plus up to 10 agentless builds. As soon as you reach the limit of running agentless builds, TeamCity will not detach steps in the following builds until some of the current agentless builds finish.
{product="tc"}

The number of builds that can simultaneously run without an agent is limited by the number of your active agent licenses. For example, if you have 10 agent licenses, you can run in parallel up to 10 regular build on agents plus up to 10 agentless builds. As soon as you reach the limit of running agentless builds, TeamCity will not detach steps in the following builds until some of the current agentless builds finish.
{product="tcc"}

<seealso>
        <category ref="concepts">
            <a href="agentless-build-step.md">Agentless Build Step</a>
        </category>
</seealso>

