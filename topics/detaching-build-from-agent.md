[//]: # (title: Detaching Build from Agent)
[//]: # (auxiliary-id: Detaching Build from Agent)

If a build does not require its [agent](build-agent.md) at the final steps, it can release this agent so it becomes available to other builds. During such [_agentless steps_](agentless-build-step.md), the build runs in an external software and reports directly to the TeamCity server.

## Releasing build agent

To release its current build agent, a build needs to send the `##teamcity[detachedFromAgent]` [service message](service-messages.md): for example, by a remote task of the [Command Line](command-line.md) step or via REST API. After receiving this message, the server decides if the agent can be released. If the [limit of agentless builds](#Agentless+builds'+licensing) is not exceeded, it releases the agent and it instantly becomes available to other builds. Otherwise, the agent stays attached to the build until some of the running agentless builds finish. 

After being detached, the agent skips all the following steps of the build, unless they have the "[_Always, even if build stop command was issued_](configuring-build-steps.md#Execution+policy)" execution policy enabled. If necessary, you can enable it for mandatory final steps – the agent will be released only after completing them.

>We highly recommend releasing the agent only during the last build step. Make sure the tasks performed outside TeamCity do not require a build agent.
                        
During agentless steps, the external tool should report all build status information directly to the TeamCity server.

## Logging build data

Without an agent, a build can send its logs and any other requests to TeamCity via [REST API](rest-api.md).

To perform a request, it needs to pass:
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

It is important to ensure that a build, executed externally, delivers a finishing request to the TeamCity server. If the server is temporarily unavailable and cannot receive this request on time, the build should retry until this operation is successful. Without the finishing request, the build will be running on the TeamCity server indefinitely, until reaching its specified timeout, if any.

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

<anchor name="DetachingBuildfromAgent-agentless-licensing"/>

## Agentless builds' licensing

The number of builds that can simultaneously run without an agent is limited by the number of your active [agent licenses](licensing-policy.md#Number+of+Agents). For example, if you have 10 agent licenses, you can run in parallel up to 10 agents plus up to 10 agentless builds. As soon as you rich the limit of running agentless builds, TeamCity will not detach steps in the following builds until some of the current agentless builds finish.

<seealso>
        <category ref="concepts">
            <a href="agentless-build-step.md">Agentless Build Step</a>
        </category>
</seealso>

