[//]: # (title: Golang)
[//]: # (auxiliary-id: Golang)

The Golang build feature enables the real-time reporting and history of Go test results in TeamCity.

Before running builds, make sure a Go compiler is installed on an agent.

To enable Go tests reporting in TeamCity, run them with the `-json` flag using one of these two methods:

* Add this flag to the [Command Line](command-line.md) build runner's script: `go test -json`.
* Add the `env.GOFLAGS = -json` parameter to the build configuration.    

<img src="GoEnvParameter.png" width="500" alt="Add a parameter to enable parsing of Golang tests"/>

<note>

Make sure you use only one of the methods to set the `-json` flag. Setting it in both build parameters and the runner's script will make Go fail to build.

</note>
