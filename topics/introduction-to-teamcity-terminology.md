[//]: # (title: Introduction to TeamCity Terminology)
[//]: # (auxiliary-id: Introduction to TeamCity Terminology)

The following guide explains main TeamCity terms and concepts in comparison to their analogues widely used in continuous integration software.   
This guide gives a general idea on the most common TeamCity concepts and serves as a starting point for users migrating from different CI/CI platforms. You can learn more about mentioned TeamCity terms and other notions in the [Concepts](concepts.md) chapter. For a quick reference, see [Basic TeamCity Concepts](continuous-integration-with-teamcity.md#Basic+TeamCity+concepts).

## TeamCity Object Hierarchy

_Projects_ are the topmost objects of the TeamCity hierarchy, and _Root project_ is the main default object that contains all your custom projects and their subprojects. Each project serves for storing logically related _build configurations_ and determining their common settings and parameters. Most often, the TeamCity project corresponds to a single software product or its specific version.

Projects can be configured manually or automatically – based on an external repository in any of the supported version control systems. To store the connection and checkout settings of a single external repository, TeamCity uses a preset called _VCS root_. All VCS roots, required by build configurations of a project, are stored in the project's settings along with the other shared settings like cloud profile connections, common parameters, clean-up rules, and many others.

Example of an object hierarchy in TeamCity:

<img src="ex-hierarchy.png" alt="Example object hierarchy" width="250"/>

A build configuration serves as a blueprint for running a certain job, or a _build_, based on the project's source code: either it is a compilation, tests, deployment – or all these stages one by one. It could store settings used to run, for example, a nightly build or integration tests. Sometimes, you need to create multiple similar build configurations that differ only in few aspects: for this purpose, you can first compose a _build configuration template_ – and then use it to generate as many cognate build configurations as needed; this is most helpful for testing and deploying software on different operating systems.

<tip>

In TeamCity terms, it is considered that a build configuration _belongs_ to its parent project and that each running build that uses this configuration as a blueprint _belongs_ to this build configuration. In certain contexts, you will notice that the terms _build_ and _build configuration_ are used interchangeably in the documentation: for example, if we describe an interrelation between two builds from different configurations, we can consider each _build_ both as a single running job and as a carrier of its build configuration settings.

</tip>

Each build configuration relies on _build features_ and _build steps_. A build feature is a piece of extra functionality available to a build: it could be a performance monitor, a reporting tool, or a support for pull requests. Some features are built in TeamCity and some can be obtained by installing external plugins. A build step is a logical stage of a build that is performed by a certain _build runner_; TeamCity offers a wide ranger of different runners – from testing tools to Docker integration.

Another important preset included into a build configuration is a _trigger_. There are multiple types of triggers in TeamCity but their common purpose is to run a build automatically when a preconfigured condition is satisfied thus establishing automatic continuous integration and delivery of a software product.

TeamCity offers all these granular objects so you can configure as flexible build environment as possible. However, they would not make much sense without means to interconnect them with each other. For this purpose, TeamCity provides two types of dependencies between build configurations (that is between their builds):
* _artifact dependency_ – for sending _artifacts_ produces by one build to another build; example of an artifact is a `.jar` application that is compiled by one build configuration and deployed by another one.
* _snapshot dependency_ – for assigning multiple builds to one version snapshot (such as revision) so the exact source files are used on all the building stages.

By interconnecting builds from different configurations or even projects with dependencies, you can create a complex and flexible _build chain_. The TeamCity product itself is built with a ramified chain of builds used for compiling and testing different software modules and for deploying the resulting distributives to web.

Example of a build chain in TeamCity:

<img src="ex-build-chain.png" alt="Example build chain" width="700"/>

TeamCity can detect changes and run builds on different _branches_ which involves another wide set of functionality like _tags_, _personal builds_, and _status commits to VCS_.

## TeamCity Build Environment

The TeamCity server itself stores all the objects' settings, manages _build queue_, monitors the state of running builds, and performs many other tasks. You can install as many additional _secondary servers_ as you need to ensure high availability of your setup.

However, a different piece of software is used for actually running builds – a _build agent_. By default, you get three build agents with your TeamCity server but you can get more if required. A build agent software can be installed on a different machine or alongside the server.

<img src="arch-workflow.png" alt="Basic TeamCity workflow" width="700"/>

Check out the [basic CI workflow in TeamCity](continuous-integration-with-teamcity.md#Basic+CI+Workflow+in+TeamCity) description to get an idea of how TeamCity agents run builds.

## Terminology Mapping

If you are migrating to TeamCity from another CI/CD solution, you might need some time to get used to the terms and concepts of the TeamCity environment. The following sections provide you an approximate mapping of general terms between TeamCity and other popular CI/CD platforms.

### Mapping to GitLab CI Terminology

<table>

<tr><td>

GitLab CI term

</td><td>

TeamCity equivalent

</td></tr>

<tr><td>

`.gitlab-ci.yml`

</td><td>

[Build steps](configuring-build-steps.md) and [build triggers](configuring-retry-build-trigger.md)
 
See also: [deployment build configuration type](deployment-build-configuration.md)

</td></tr>

<tr><td>

GitLab Code Quality

</td><td>

[Code Quality Tools](code-quality-tools.md) and [Code Coverage](code-coverage.md)

</td></tr>

<tr><td>

GitLab server

</td><td>

[TeamCity server](installing-and-configuring-the-teamcity-server.md)

</td></tr>

<tr><td>

GitLab Runner

</td><td>

[Build agent](build-agent.md)

</td></tr>

<tr><td>

Job

</td><td>

[Build configuration](build-configuration.md)

</td></tr>

<tr><td>

Pipeline

</td><td>

[Build chain](build-chain.md) (via snapshot dependencies in the UI), or [sequential chain](kotlin-dsl.md#Build+Chain+DSL+Extension) (via DSL)

</td></tr>

<tr><td>

Stage

</td><td>

[Build step](configuring-build-steps.md)

</td></tr>

<tr><td>

Tag

</td><td>

[Agent requirements](agent-requirements.md)

</td></tr>

</table>

### Mapping to CircleCI Terminology

<table>

<tr><td>

CircleCI term

</td><td>

TeamCity equivalent

</td></tr>

<tr><td>

Image (`.circleci/config.yml`)

</td><td>

[Build configuration](creating-and-editing-build-configurations.md) with [build steps](configuring-build-steps.md)

</td></tr>

<tr><td>

Build Agent

</td><td>

[Build agent](build-agent.md)

</td></tr>

<tr><td>

Cache

</td><td>

[Artifact dependency](artifact-dependencies.md)

</td></tr>

<tr><td>

Job

</td><td>

[Build configuration](build-configuration.md)

</td></tr>

<tr><td>

Nomad Server

</td><td>

[TeamCity server](installing-and-configuring-the-teamcity-server.md)

</td></tr>

<tr><td>

Nomad Client

</td><td>

[Secondary node](configuring-secondary-node.md)

</td></tr>

<tr><td>

Project

</td><td>

[Project](project.md)

</td></tr>

<tr><td>

Step

</td><td>

[Build step](configuring-build-steps.md)

</td></tr>

<tr><td>

Workflow

</td><td>

[Build Chain](build-chain.md) (via [dependencies](configuring-dependencies.md) in the UI), or [sequential chain](kotlin-dsl.md#Build+Chain+DSL+Extension) (via DSL)

</td></tr>

<tr><td>

Workspace

</td><td>

[Checkout directory](build-checkout-directory.md)

</td></tr>

</table>

### Mapping to Travis CI Terminology

<table>

<tr><td>

Travis CI term

</td><td>

TeamCity equivalent

</td></tr>

<tr><td>

Build configuration (`.travis.yml`)

</td><td>

[Build configuration](build-configuration.md)

</td></tr>

<tr><td>

Build

</td><td>

Build  that belongs to a [build configuration](build-configuration.md)

</td></tr>

<tr><td>

Job

</td><td>

[Build step](configuring-build-steps.md)

</td></tr>

<tr><td>

Stage

</td><td>

Build in a [build chain](build-chain.md)

</td></tr>

</table>


### Mapping to Jenkins Terminology

Please refer to separate [Jenkins to TeamCity Migration Guidelines](jenkins-to-teamcity-migration-guidelines.md).

__ __

<tip>

__Share your insight!__

If you are experienced in working with two or more mentioned CI/CD tools or notice any inaccuracy in the provided mapping, we encourage you to share your [feedback](https://confluence.jetbrains.com/display/TW/Feedback) with us so we can update this page with relevant information.

</tip>