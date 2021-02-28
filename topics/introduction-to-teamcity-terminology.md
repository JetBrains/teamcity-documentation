[//]: # (title: Introduction to TeamCity Terminology)
[//]: # (auxiliary-id: Introduction to TeamCity Terminology)

The following guide explains the main TeamCity terms and concepts. For a quick reference, see also [Basic TeamCity concepts](continuous-integration-with-teamcity.md#Basic+TeamCity+concepts).

## TeamCity Object Hierarchy

_Projects_ are the topmost objects of the TeamCity hierarchy, and _Root project_ is the main default object that contains all your custom projects and their subprojects. Each project serves for storing logically related _build configurations_ and defining their common settings and parameters. Most often, a TeamCity project corresponds to a single software product or its specific version.

Projects can be configured manually or automatically, from a VCS repository. The settings of each repository used in a project are stored in a dedicated preset called _VCS root_. Besides VCS roots, a project contains other settings shared between its nested build configurations: _cloud profile connections_, _common parameters_, _clean-up rules_, and many others.

Example of an object hierarchy in TeamCity:

<img src="ex-hierarchy.png" alt="Example object hierarchy" width="227"/>

A build configuration serves as a blueprint for running a certain job, or a _build_, based on the project's source code: either it is a compilation, testing, deployment — or all these stages one by one. A single build configuration can define settings that are used to run, for example, a nightly build or integration tests.   
In certain cases, you may need to create multiple build configurations that differ only in few details. For this purpose, you can compose a _build configuration template_ and use it to generate as many similar build configurations as needed. One of the popular use cases for using templates is testing and deploying software on different operating systems.

>In TeamCity terms, it is considered that a build configuration _belongs_ to its parent project and that each running build that uses this configuration as a blueprint _belongs_ to this build configuration. In certain contexts, you will notice that the terms _build_ and _build configuration_ are used interchangeably in the documentation. For example, if we describe two related builds from different configurations, we can perceive each _build_ both as a single running job and as a carrier of its build configuration settings.

Each build configuration can have _build features_ and _build steps_. A build feature is a piece of extra functionality available to a build: it could be a performance monitor, a reporting tool, or a support for pull requests. Some features are built in TeamCity and some can be obtained by installing external plugins. A build step is a logical stage of a build that is performed by a certain _build runner_; TeamCity offers a wide range of different runners: from testing tools to Docker integration.

Another important setting included into a build configuration is a _trigger_. There are multiple types of triggers in TeamCity. Their common purpose is to run a build automatically when a preconfigured condition is satisfied thus establishing automatic continuous integration of a software product.

TeamCity offers all these granular objects so you can configure a build process as flexibly as possible. However, they would make more sense if you have the means to interconnect them with each other. For this purpose, TeamCity provides two types of dependencies between build configurations (that is between their builds):
* _artifact dependency_ — for sending _artifacts_ produced by one build to another build; an example of an artifact is a `.jar` application that is compiled by one build configuration and deployed by another.
* _snapshot dependency_ — for assigning multiple builds to the same source revision (commit) so the same project files are used on all the building stages.

By connecting builds from different configurations or even projects with dependencies, you can create a flexible _build chain_. The TeamCity product itself is built with a complex chain of builds. It allows us to compile and test different software modules and deploy the resulting distribution files to the web.

Example of a build chain in TeamCity:

<img src="ex-build-chain.png" alt="Example build chain" width="700"/>

## TeamCity Build Environment

The TeamCity server stores all the objects' settings, manages the _build queue_, monitors the state of running builds, and performs many other tasks. You can install as many additional _secondary servers_ as you need to ensure high availability and scalability of your setup.

A different piece of software is used for actually running builds — a _build agent_. By default, you get three build agents with your TeamCity server but you can get more if required. A build agent software can be installed on a different machine or alongside the server.

>Check out the [basic CI workflow in TeamCity](continuous-integration-with-teamcity.md#Basic+CI+Workflow+in+TeamCity) to see an example of a simple build run.