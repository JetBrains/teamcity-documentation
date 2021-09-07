[//]: # (title: Node.js)
[//]: # (auxiliary-id: Node.js)

The Node.js build runner allows running Node.js tools like [`npm`](https://www.npmjs.com/), [`yarn`](https://yarnpkg.com/), and [`node`](https://github.com/nodejs/node).

Refer to [Configuring Build Steps](configuring-build-steps.md) for a description of common build steps' settings. Refer to [Docker Wrapper](docker-wrapper.md) to learn how you can run this step inside a Docker container.

## Prerequisites

Currently, Node.js steps can only be run inside a Docker container. TeamCity uses `node:lts` by default, or, if there is an `.nvmrc` file inside your project, it will search for the image specification there.

## Autodetecting JavaScript Steps

If your repository contains a `package.json` file, TeamCity will [automatically detect](configuring-build-steps.md#Autodetecting+build+steps) used frameworks and propose adding respective build steps.

>If TeamCity detects a dependency on a test framework, it will propose choosing between two steps: run tests via this framework or with a general `test` command. To display structured test reports in TeamCity, select the step with the framework.
> 
{type="note"}

Currently supported frameworks are [ESlint](https://eslint.org/), [Jest](https://jestjs.io/), or [Mocha](https://mochajs.org/).

If TeamCity detects an `.nvmrc` file, it will automatically use the node version specified in it.

## Running Node.js Commands

In the _Shell script_ field, enter all Node.js commands to be executed in this step.

## Accessing Private Registries

To access a private npm registry during a build (for example, to download a package), you need to:
1. Configure an _NPM Registry_ connection as described [here](configuring-connections.md#npm-registry-settings).
2. In __Build Configuration Settings | Build Features__, add an _NPM Registry Connection_ build feature and select the new connection, so it can be used in this configuration.

As a result, a TeamCity agent will authenticate in this registry during the build.

>Note that TeamCity will only be able to access registries where automation tokens are allowed. If your connection test fails in TeamCity, revise the registry settings.
> 
{type="note"}

Alternatively to this procedure, you can let TeamCity parse a token from the `.npmrc` file inside your JS project. To achieve this, declare a token variable in this file as specified [here](https://docs.npmjs.com/using-private-packages-in-a-ci-cd-workflow#create-and-check-in-a-project-specific-npmrc-file) and then create an [environment variable](configuring-build-parameters.md) `NPM_TOKEN` in TeamCity with the value of the access token and the "Password" type.

>If a token is configured in your NPM registry connection, TeamCity will use it for connecting to this registry. However, there is known issue when TeamCity might use the token specified in the `.npmrc` file instead of that in the connection settings. See the workarounds to this issue in [our tracker](https://youtrack.jetbrains.com/issue/TW-71200#focus=Comments-27-4854154.0-0).
> 
{type="warning"}