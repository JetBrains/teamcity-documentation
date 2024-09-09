[//]: # (title: Glossary)
[//]: # (auxiliary-id: Glossary)

## A

Agent cloud profile
: A collection of settings according to which TeamCity starts virtual machines with build agents in the cloud. Configuring a cloud profile is a mandatory step of integrating TeamCity with the cloud providers: Amazon EC2, Kubernetes, VMWare vSphere, and [others](teamcity-integration-with-cloud-solutions.md). 
{instance="tc"}
: A collection of settings according to which TeamCity starts virtual machines with build agents in the cloud. Configuring a cloud profile is a mandatory step of integrating TeamCity with the cloud providers: Amazon EC2, Kubernetes, VMWare vSphere, and others. 
{instance="tcc"}

Agent Home Directory
: A directory where a build agent is installed. Its location can be changed manually. This directory contains the agent configuration file and other important files, such as scripts for starting and stopping the agent.

Agentless build step
: A build step that can run without a build agent, in an external software. Using such steps allows releasing a build agent as soon as it is no longer necessary for completing the build operations. This helps optimize the agents' workload on the server.

Agent pool
: A named set of build agents to which you can assign projects. A build agent can belong to one pool only. A project can use multiple pools for its builds. Using pools allows binding specific agents to specific projects.

Agent requirement
: A rule that specifies if a given build configuration can run on a particular agent. Allows managing what agents are used for running each build configuration.

Agent-side checkout
: A mode when a build agent checks out a build's source files from VCS before starting the build. Could be [set as preferred or forced mode](vcs-checkout-mode.md). Alternative to server-side checkout.

Agent Work Directory
: A directory on a build agent that contains default checkout directories. By default, it is the same as the Agent Home Directory.

Already Fixed In
: The prompt in the overview of a failed build that shows the build where the currently failed test was successful and that run _after_ the build with the initial test failure.

Artifact dependency
: A dependency between build configurations that allows sending artifacts produced by one build to another build.

Authentication module
: A chunk of functionality responsible for implementing a certain authentication technology in TeamCity. TeamCity has two types of authentication modules: Credentials Authentication Modules and HTTP Authentication Modules.

## B

Build
: A process that performs a certain CI/CD job. Most builds comprise multiple sequential steps executing their own granular actions. A build is executed according to the settings specified in its build configuration.

Build agent
: A piece of software which listens for the commands from the TeamCity server and starts the actual build process. Agents can be installed in the customer's environment or run on-demand in the cloud.

Build artifacts
: Files produced by a build. Typically, these include distribution packages, WAR files, reports, log files, and so on.

Build chain
: A pipeline, or sequence, of builds connected by snapshot dependencies. Connecting builds into a chain can give many advantages like running them on the same source revision, optimally queueing and triggering them, and monitoring them on the pipeline graph. Example of a build chain is: compiling, testing, and deploying a single project.

Build checkout directory
: A directory on the TeamCity agent machine where all the sources of all builds are checked out into.

Build configuration
: A collection of settings of a certain type of builds that determines their steps, features, and other common parameters. It serves as a blueprint for all builds that belong to it. A Build Configuration Home page shows a list of all its recent builds. Examples of build configurations are _distribution_, _integration tests_, _prepare release distribution_, _"nightly" build_.

Build configuration template
: A predefined collection of settings that serves as a base for creating similar build configurations.

Build feature
: A piece of extra functionality available to a build (it could be a performance monitor, a reporting tool, or a support for pull requests).

Build grid
: A pool of agents used by TeamCity to simultaneously create builds of multiple projects. A build grid employs currently unused resources from multiple computers, any of which can run multiple builds and/or tests at the same time, for single or multiple projects across your company.

Build history
: A record of the past builds produced by TeamCity.

Build log
: An enhanced console output of a build. It is represented by a structured list of the events which took place during the build. Generally, it includes entries on TeamCity-performed actions and the output of the processes launched during the build. TeamCity captures the processes output and stores it in an internal format that allows for hierarchical display.

Build number
: A string identifier composed according to the pattern specified in the build configuration settings. This number is displayed in the UI and passed into the build as a [predefined parameter](predefined-build-parameters.md). It can be:
* [used to download artifacts](patterns-for-accessing-build-artifacts.md#Obtaining+Artifacts)
* [referenced as a property](predefined-build-parameters.md)
* [shared for builds connected by a dependency](how-to.md#Share+the+Build+number+for+Builds+in+a+Chain+Build)
* [used in artifact dependencies](artifact-dependencies.md)
* [set by a service message](service-messages.md#Reporting+Build+Number)

Build queue
: A list of builds that were [triggered](configuring-build-triggers.md) and are waiting to be started. TeamCity will distribute them to compatible build agents as soon as these agents become idle. A queued build is assigned to an agent at the moment it is started on the agent; no preassignment is made while the build is waiting in the build queue.

Build parameter
: A name-value pair, defined by a user or provided by TeamCity, which can be used in a build. [Build parameters](configuring-build-parameters.md) help flexibly share settings and pass them to build steps.

Build runner
: A TeamCity module that allows integration with a specific tool: Command Line, .NET, Kotlin Script, Gradle, and so on. Each build step defines the runner that will be used to execute it.

Build step
: A task to be executed by a build runner. A single build configuration can include multiple steps.

Build tag
: A label that can be assigned to a build. Labels can be used for organizing the history of builds, easier search and navigation, and so on.

Build trigger
: A rule that initiates a new build on a certain event (for example, on a change in the configured VCS root). The build is put into the build queue and is started when there are agents available to run it.

Build working directory
: The directory set as current for the build process. By default, this is the same directory as the build checkout directory.

## C

Change
: Any modification of the source code. If a change has been committed to the version control system, but not yet included in a build, it is considered pending for a certain build configuration.

Clean checkout
: An operation that ensures that the next build will get a copy of the sources fetched all over from the VCS. All the content of the build checkout directory is deleted, and the sources are refetched from the version control.

Code coverage
: A number of metrics that measure how your code is covered by unit tests. TeamCity supports coverage for various .NET and Java tools out of the box.

Code duplicates
: Repetitive blocks of code. The Duplicates Finder build runners (for [Java](duplicates-finder-java.md) and [C#](duplicates-finder-resharper.md)) search for similar code fragments and provide a comprehensive report on repetitive blocks of code discovered in your code base.

Code inspections
: Code analysis tools capable of inspecting source code on the fly, finding and reporting common problems and anti-patterns.

Composite build configuration
: A special type of build configuration that aggregates results from several other builds combined by snapshot dependencies. A composite build can be viewed as a build which consists of several parts which can be executed in parallel on different agents. All these parts will have a synchronized snapshot of the source code, and the results can be seen in a single place.

Configuration parameter
: In TeamCity, a type of [build parameter](configuring-build-parameters.md) that is meant to share settings within a build configuration (widely used in templates and meta-runners).

Continuous integration
: A software engineering term describing a process that frequently rebuilds and tests an application. Generally, it takes the form of a server process or daemon that
* monitors a file system or version control system for changes;
* runs the build process (for example, launches scripts);
* runs tests (for example, JUnit or NUnit).  
Continuous integration is also associated with _Extreme Programming_ and other _agile_ software development practices.  
Following the principles of _Continuous Integration_, TeamCity allows users to monitor the software development process of the company, while improving communication and facilitating the integration of changes without breaking any established practices.

Custom build run
* A standalone build whose settings have been adjusted compared to its build configuration. Such a build can be initiated from a context menu next to the __Run__ button.

## D

Dependent build
: A build configuration that depends on one or more builds via snapshot or artifact dependencies.

Deployment build configuration
: A special type of build configuration that performs deploying to some environment. These are usually build configurations that have snapshot or artifact dependencies on the builds whose results they deploy.

Difference viewer
: A TeamCity component that allows reviewing the differences between two versions of a file modified in the source control and navigating between these differences.

## E

Environment variable
: In TeamCity, a type of [build parameter](configuring-build-parameters.md) that is passed into a spawned build process as into an environment. Defined by the `env.` prefix.

## F

Favorite build
: A mark assigned to a certain build by a user. Users can quickly access their favorite builds in the UI and choose to subscribe to notifications only about these builds.

First failure
: A link to the build where TeamCity detected the first failure of the current test for the same build configuration. Starting from the current build, TeamCity goes back throughout the build history to find out when this test failed for the first time.

## G

Guest user
: A user accessing a TeamCity server anonymously. The server administrator can enable or disable guest access on demand. By default, guest users have the _Project Viewer_ role for all the projects, but its role and permissions can be adjusted.

## H

History build
: A build that starts after a build with more recent changes. It disrupts a normal build flow according to the [order of the source revisions](revision.md#Revision+order).

## N

Notifier
: A TeamCity module that sends notifications about build statuses to external targets. Examples: Slack Notifier, Email Notifier, Browser Notifier.

## P

Permission
: An authorization to perform particular operations: for example, to run a build or modify build configuration settings.

Personal build
: A build that is executed out of the common build sequence. Typically, it uses the changes not yet committed into the version control. This allows developers to run their newly added functionality in a real environment without modifying the project code in the VCS. Personal builds are usually initiated from an IDE via the Remote Run procedure.

Pinned build
: A build that is prevented from being removed during a scheduled [clean-up](teamcity-data-clean-up.md).

Pre-tested commits
: An approach that prevents committing defective code into a build, so the entire team's process is not affected. [These diagrams](https://www.jetbrains.com/teamcity/features/delayed_commit.html) illustrate the TeamCity approach to pre-tested commits.

Project
: A collection of build configurations. A project can correspond to a software project, a specific version/release of a project or any other logical group of the build configurations. A project defines common settings for all its build configurations.

## R

Remote debug
: A feature allowing to remotely debug tests on the TeamCity agent machine from the IDE on the local developer machine. This feature is useful when the agent environment is unique in some aspect which causes a test to fail and when it is difficult to reproduce the problem locally.

Remote run
: A personal build initiated by a developer from one of the supported IDE plugins to test how the changes will integrate into the project's code base. Unlike pre-tested commits, no code is checked into the VCS regardless of the state of the personal build initiated via Remote Run.

Revision
: A specific state of a version control history, or, in other words, version of the source code. When changes occur, they are usually identified by a number or letter code termed _revision_.

Role
: A set of permissions that can be granted to a user in one or all projects thus controlling their access to the projects and various features in the UI.

Root project
: A default project at the top of the project hierarchy. Its settings are available to all the other projects on the server.

Run configuration policy
: A policy that allows selecting specific build configurations you want a build agent to run. By default, build agents run all compatible build configurations, and this is not always desirable — in this case, this policy lets you limit the allowed set in each agent's details. The run configuration policy settings are located in __[Agents Details](viewing-build-agent-details.md) | Compatible configurations__.

## S

Server-side checkout
: A mode when TeamCity checks out a build's source files from VCS to the server machine and, before starting each new build, exports them to the build agent machine. Could be [set as preferred or forced mode](vcs-checkout-mode.md). Alternative to agent-side checkout.

Snapshot dependency
: A dependency between build configurations that allows assigning multiple builds to the same source revision (commit) so the same project files are used on all the building stages.

Super-user
: The super-user login allows accessing the server UI with the System Administrator permissions. It is useful when the administrator forgot the credentials or needs to fix authentication-related settings. The login is performed using an authentication token that can be found in the server logs.

System property
: In TeamCity, a type of [build parameter](configuring-build-parameters.md) that can be passed into build scripts of certain runners as a variable specific to a build tool. Defined by the `system.` prefix.

## T

TeamCity Data Directory
: The directory on the file system used by the TeamCity server to store configuration, build results, and current operation files. This directory is the primary storage for all the configuration settings and holds the data critical to the TeamCity installation.

TeamCity Home Directory
: The directory where the TeamCity server application files and libraries were unpacked during the TeamCity installation.

## U

User account
: A combination of username and password that allows TeamCity users to sign in to the server and use its features. User accounts can be created manually, or automatically upon sign in depending on used authentication scheme (refer to the [Authentication Modules](authentication-modules.md) section for more details).

User group
: A method of aggregating user accounts with similar roles and permissions for easier management.

## V

VCS root
: A connection to a version control system. It represents a set of parameters (paths to sources, username, password, and other settings) that determine how TeamCity communicates with a VCS to [monitor changes](configuring-vcs-roots.md#Common+VCS+Root+Properties) and get sources for a build.


