[//]: # (title: Build Agent Configuration)
[//]: # (auxiliary-id: Build Agent Configuration)

>This page is only relevant for [self-hosted build agents](teamcity-cloud-subscription-and-licensing.md#cloud-self-hosted-agents).
>
{type="note" product="tcc"}

Configuration settings of the build agent are stored in the `<TeamCity Agent Home>/conf/buildAgent.properties` file.

## General Agent Configuration

The configuration file can store properties that will be published on the server as __Agent properties__ and can participate in the [Agent Requirements](agent-requirements.md) expressions. All [system and environment properties](predefined-build-parameters.md#Agent+Properties) defined in the file will be passed to every build run on the agent.

The file is a [Java properties](http://java.sun.com/j2se/1.5.0/docs/api/java/util/Properties.html#load(java.io.InputStream)) file.

A quick guide is:
* use `property_name=value<newline>` syntax
* use `#` in the first position of the line for a comment
* use `/` instead of `\` as the path separator. If you need to include `\`, escape it with another `\`.
* whitespaces within a line matter

Example of the file:

```Shell
## The address of the TeamCity server. The same as is used to open the TeamCity web interface in the browser.
## Note that usage of https:// is recommended
serverUrl=http://localhost:8111/

## The unique name of the agent used to identify this agent on the TeamCity server
## Use blank name to let server generate it.
## By default, this name would be created from the build agent's host name
name=Default agent

## Container directory to create default checkout directories for the build configurations.
workDir=../work

## Container directory for the temporary directories.
## Please note that the directory may be cleaned between the builds.
tempDir=../temp
 
## Container directory for agent state files and caches.
## TeamCity agent assumes ownership of the directory and can delete the content inside.
systemDir=../system

 
######################################
#   Optional Agent Properties        #
######################################
## A token which is used to identify this agent on the TeamCity server for agent authorization purposes.
## It is automatically generated and saved back on the first agent connection to the server.
authorizationToken=1234567890abcdefghijklml

```

<note>

Make sure that the file is writable for the build agent process itself. For example, the file is updated to store its authorization token that is generated on the server-side.
</note>

If the `name` property is not specified, the server will generate a build agent name automatically. By default, this name will be created from the build agent's host name.

The file can be edited while the agent is running: the agent detects the change and (upon finishing a running build, if any) restarts automatically  loading the new settings. 

## Optional Properties

If the default polling protocol is changed in favor of legacy [bidirectional communication](setting-up-and-running-additional-build-agents.md#Bidirectional+Communication) between the server and the agent, the server must be able to open HTTP connections to the agent.
{product="tc"}

The port where the TeamCity build agent starts and where it listens for the incoming data from the server is determined via the `ownPort` property (9090 by default). If the firewall is configured, make sure that the incoming connections for this port are allowed on the agent computer.

```Shell
ownPort=9090

```

>If more than one build agent is hosted on the same machine, different ports must be assigned to them via the `ownPort` property in the `buildAgent.properties` file of every agent.

The IP address used by TeamCity server to connect to the build agent is automatically detected by the server when the agent first connects to TeamCity, unless the ownAddress property is defined. If the machine has several network interfaces, automatic detection may fail and it is recommended to specify the `ownAddress` property:

```Shell
ownAddress=<own IP address or server-accessible domain name>

```

## Set up Agent behind Proxy

It is possible to configure a forward proxy server for agent-to-server connections.

<include src="how-to.md" include-id="agent-proxy-server"/>

<seealso>
        <category ref="concepts">
            <a href="agent-pool.md">Agent Pool</a>
            <a href="build-agent.md">Build Agent</a>
        </category>
        <category ref="admin-guide">
            <a href="predefined-build-parameters.md">Predefined Build Parameters</a>
            <a href="configuring-agent-requirements.md">Configuring Agent Requirements</a>
            <a href="configuring-build-parameters.md">Configuring Build Parameters</a>
        </category>
</seealso>
