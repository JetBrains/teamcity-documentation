[//]: # (title: Node.js)
[//]: # (auxiliary-id: Node.js)

The Node.js build runner, provided in terms of TeamCity EAP 2021.1, allows running [`npm`](https://www.npmjs.com/) and [`yarn`](https://yarnpkg.com/) commands.

Refer to [Configuring Build Steps](configuring-build-steps.md) for a description of common build steps' settings. Refer to [Docker Wrapper](docker-wrapper.md) to learn how you can run this step inside a Docker container.

## Prerequisites

In terms of EAP, Node.js steps can only be run inside a Docker container. `node:lts` is used by default. If there is an `.npmrc` file inside your JS project, TeamCity will automatically use the node version specified in it.

## Autodetecting JS Steps

If your repository contains a `package.json` file, TeamCity will [automatically detect](configuring-build-steps.md#Autodetecting+build+steps) used frameworks and propose adding respective build steps.

Currently supported frameworks are [ESlint](https://eslint.org/), [Jest](https://jestjs.io/), [Mocha](https://mochajs.org/), and [Hermione](https://www.npmjs.com/package/hermione).

## Accessing Private Registries

To access a private npm registry during a build (for example, to download some package), you need to:
1. In __Project Settings | Connections__, configure an _NPM Registry_ connection: specify the registry URL, automation access token, and, optionally, a scope.
2. In __Build Configuration Settings | Build Features__, add an _NPM Registry Connection_ build feature and select the new connection, so it can be used in this configuration.

As a result, a TeamCity agent will authenticate in this registry during the build and sign out after its finish.

>Note that TeamCity will only be able to access registries where automation tokens are allowed. If your connection test fails in TeamCity, revise the registry settings.
> 
{type="note"}

Alternatively to this procedure, you can let TeamCity parse a token from the `.npmrc` file inside your JS project. To achieve this, declare a token variable in this file as [specified here](https://docs.npmjs.com/using-private-packages-in-a-ci-cd-workflow#create-and-check-in-a-project-specific-npmrc-file) and then create an [environment variable](configuring-build-parameters.md) `NPM_TOKEN` in TeamCity with the value of the access token and the Password type.

>If the `.npmrc` file is present in your repository __and__ you configure an NPM connection in TeamCity, TeamCity might use the token specified in the `.npmrc` file instead of that in the connection settings. See the workarounds to this issue in [our tracker](https://youtrack.jetbrains.com/issue/TW-71200#focus=Comments-27-4854154.0-0).
> 
{type="warning"}