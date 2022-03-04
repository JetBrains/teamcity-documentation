[//]: # (title: Continuous Integration with TeamCity)
[//]: # (auxiliary-id: Continuous Integration with TeamCity)

## What is Continuous Integration?

Continuous Integration is a software development practice in which developers commit code changes into a shared repository several times a day. Each commit is followed by an automated build to ensure that new changes integrate well into the existing code base and to detect problems early.    

>To learn about the basics of continuous integration, refer to our [CI/CD Guide](https://www.jetbrains.com/teamcity/ci-cd-guide/).

## What is TeamCity?

JetBrains TeamCity is a user-friendly continuous integration (CI) server for developers and build engineers [free of charge with the Professional Server License](http://www.jetbrains.com/teamcity/buy/index.jsp) and easy to set up!
{product="tc"}

JetBrains TeamCity is a user-friendly and easy to set up continuous integration (CI) server for developers and build engineers.
{product="tcc"}

This video gives a general overview of the main TeamCity features and explains the [licensing policy](licensing-policy.md):
{product="tc"}

This video gives a general overview of the main TeamCity features:
{product="tcc"}

<video href="s68u2shSo6o"
       title="General TeamCity overview"/>

### What can you do with TeamCity?

* Run parallel builds simultaneously on different platforms and environments
* Optimize the code integration cycle and be sure you never get broken code in the repository
* Review on-the-fly test results reporting with intelligent tests reordering
* Run code coverage and duplicates finder for Java and .NET
* Customize statistics on build duration, success rate, code quality, and custom metrics
* and much more

To learn more about major TeamCity features, refer to the [official JetBrains website](http://www.jetbrains.com/teamcity/features/index.html).

TeamCity works great not only for admins and build engineers, but also for developers. Watch this video to see some of the signature features it provides:
<video href="icuhBgEFtVM"
       title="TeamCity for developers"/>

The complete list of supported platforms and environments can be found [here](supported-platforms-and-environments.md).

## Basic TeamCity concepts

This section describes the main TeamCity concepts. You can find a more comprehensive explanation in the [Introduction to TeamCity terminology](introduction-to-teamcity-terminology.md).

The __TeamCity build system__ comprises a __server__ and __build agents__.

<table><tr>

<td>

Concept

</td>

<td>

Description

</td></tr><tr>

<td>

__Build agent__

</td>

<td>

A piece of software that actually executes a build process. It is installed and configured separately from the TeamCity server, that is the agent can be installed on a separate machine (physical or virtual, and it can run the same operating system (OS) as the server or a different OS.  
Build agents in TeamCity can have different platforms, operating systems, and preconfigured environments that you may want to test your software on. Different types of tests can be run under different platforms simultaneously so the developers get faster feedback and more reliable testing results.
{product="tc"}

>It is possible for the server and an agent to coexist on the same computer, but for production purposes, we recommend installing them on different machines for a number of reasons, the server performance being the most important.
>
{type="note" product="tc"}

A piece of software that actually executes a build process. It is installed and configured separately from the TeamCity server. You get access to Cloud agents with your TeamCity Cloud subscription, but you can also host agents on a physical machine.  
Build agents in TeamCity can have different platforms, operating systems, and preconfigured environments that you may want to test your software on. Different types of tests can be run under different platforms simultaneously so the developers get faster feedback and more reliable testing results.
{product="tcc"}

</td></tr><tr>

<td>

__TeamCity Server__ 

</td>

<td>

The __server__ itself __does not run either builds or tests:__ the server's job is to monitor all the connected build agents, distribute [queued builds](build-queue.md) to the agents based on compatibility requirements, and report the results. All information on the build results (build history and all the build-associated data except for artifacts and build logs), VCS changes, agents, build queue, user accounts and user permissions, and so on, are stored in a database.

>It is possible for the server and an agent to coexist on the same computer, but for production purposes, we recommend installing them on different machines for a number of reasons, the server performance being the most important.
>
{type="note" product="tc"}


</td></tr><tr>

<td>

__Project__

</td>

<td>

A TeamCity project corresponds to a software project or a specific version/release of a software project. A project is a collection of _[build configurations](managing-builds.md)_.

</td></tr><tr>

<td>

__Build configuration__

</td>

<td>

A combination of settings defining a build procedure. The settings include _[VCS roots](vcs-root.md)_, _[build steps](configuring-build-steps.md)_, _[build triggers](configuring-build-triggers.md)_ described below.

</td></tr><tr>

<td>

__VCS root__

</td>

<td>

A collection of version control settings (paths to sources, username, password, _[checkout mode](vcs-checkout-mode.md)_ and other settings) that defines how TeamCity communicates with a version control (SCM) system to monitor changes and get sources for a build.

</td></tr><tr>

<td>

__Build step__

</td>

<td>

A task to be executed. Each build step is represented by a _[build runner](build-runner.md)_ providing integration with a specific build tool (like Ant, Gradle, MSBuild, and so on), a testing framework (for example, NUnit), or a code analysis engine. Thus, in a single build you can have several steps and sequentially invoke test tools, code coverage, and, for instance, compile your project.

</td></tr><tr>

<td>

__Build trigger__

</td>

<td>

A rule which initiates a new build on certain events. For example, a _[VCS trigger](configuring-vcs-triggers.md)_ will automatically start a new build each time TeamCity detects a _[change](change.md)_ in the configured _VCS roots_. 

</td></tr><tr>

<td>

__Change__

</td>

<td>

Any modification of the source code which you introduce. If a change has been committed to the version control system, but not yet included in a build, it is considered pending for a certain build configuration.

</td></tr><tr>

<td>

__Build__

</td>

<td>

A CI/CD job executed on an agent. Consists of one or more steps that can do any service task: compile, test, deploy, produce reports, and so on.

The term _build_ can refer to both the actual process of building and the result of building. After the build process is triggered, it is put into the _[build queue](build-queue.md)_ and is started when there are agents available to run it. After the build is finished, the build agent sends _[build artifacts](build-artifact.md)_ to the server. 

</td></tr><tr>

<td>

__Build queue__

</td>

<td>

A list of builds that were triggered and are waiting to be started. TeamCity will distribute them to _[compatible](agent-requirements.md)_ build agents as soon as the agents become idle. A queued build is assigned to an agent at the moment when it is started on the agent; no preassignment is made while the build is waiting in the build queue.


</td></tr><tr>

<td>

__Build artifacts__ 

</td>

<td>

Files produced by a build, for example, installers, WAR files, reports, log files, when they become available for download.

</td></tr></table>
 

### Basic CI Workflow in TeamCity

To understand the data flow between the server and the agents, what is passed to the agents, how and when TeamCity gets the results, let's take a look at a simple build lifecycle.

<img src="cicd-flow.png" width="711" alt="Basic CI flow with TeamCity"/>

1. The TeamCity server detects a change in your VCS root (repository).
2. The server stores this change in the database.
3. The trigger, attached to the build configuration, detects the relevant change in the database and initiates the build.
4. The triggered build gets to the build queue.
5. The build is assigned to a free and compatible build agent.
6. The agent executes build steps, described in the build configuration. While executing the steps, the agent reports the build progress to the TeamCity server. It is sending all the log messages, test reports, code coverage results on the fly, so you can monitor the build process in real time.
7. After finishing the build, the agent sends build artifacts to the server.