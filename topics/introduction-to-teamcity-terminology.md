[//]: # (title: Introduction to TeamCity Terminology)
[//]: # (auxiliary-id: Introduction to TeamCity Terminology)

The following guide explains main TeamCity terms and concepts in comparison to their analogues widely used in continuous integration software.   
This guide gives a general idea on the most common TeamCity concepts and serves as a starting point for users who migrate from different CI/CI software. You can learn more about mentioned TeamCity terms and other notions in the [Concepts](concepts.md) chapter.

## TeamCity Object Hierarchy

_Projects_ are the topmost objects of the TeamCity hierarchy, and _Root project_ is the main default object that contains all the custom projects and their subprojects. Each project serves for storing logically related _build configurations_ and determining their common settings and parameters. Most often, the TeamCity project corresponds to a single software product or its specific version.

<tip>

In TeamCity terms, it is considered that a build configuration _belongs_ to its parent project and that each running build that uses this configuration as a blueprint _belongs_ to this build configuration. In certain contexts, you will notice that the terms "build" and "build configuration" are used interchangeably in the documentation: for example, if we describe an interrelation between two builds from different configurations and consider each "build" both as a single running job and as a carrier of its build configuration settings.

</tip>

Projects can be configured manually or automatically – based on an external repository in any of the supported version control systems. To store connection and checkout settings of a single external repository, TeamCity uses a preset called _VCS root_. All VCS roots, required by build configurations of a project, are stored in the project's settings along with the other shared settings like cloud profile connections, common parameters, clean-up rules, and many others.

Example of object hierarchy in TeamCity:
<img src="ex-hierarchy.png" alt="Example object hierarchy"/>

A build configuration serves as a blueprint for running a certain job, or a _build_, based on the project's source code: from compilation and tests to deployment. It could store settings used to run, for example, a nightly build or integration tests. Sometimes, you need to create multiple similar build configurations that differ only in few aspects: for this purpose, you can first compose a _build configuration template_ – and then use it to generate as many cognate build configurations as needed; this is most helpful for testing and deploying software on different operating systems.

Each build configuration relies on _build features_ and _build steps_. A build feature is a piece of extra functionality available to a build: it could be a performance monitor, a reporting tool, or a support for pull requests. Some features are built in TeamCity and some can be obtained by installing external plugins. A build step is a logical stage of a build that is performed by a certain _build runner_; TeamCity offers a wide ranger of different runners – from testing tools to Docker integration.

Another important preset included into a build configuration is a _trigger_. There are multiple types of triggers in TeamCity but their common purpose is to run a build automatically when a preconfigured condition is satisfied thus establishing automatic continuous integration and delivery of a software product.

TeamCity offers all these granular objects so you can configure as flexible build environment as possible. However, they would not make much sense without means to interconnect them with each other. For this purpose, TeamCity provides two types of dependencies between build configurations:
* _artifact dependency_ – for sending _artifacts_ produces by one build in another build; example of an artifact is a compiled `.jar` application.
* _snapshot dependency_ – for assigning multiple builds to one version snapshot (such as revision) so the exact source files are used on all the building stages.

By interconnecting builds from different configurations or even projects with dependencies, you can create a complex and flexible _build chain_. The TeamCity product itself is built with a ramified chain of builds used for compiling and testing different software modules and for deploying the resulting distributives to web.

<img src="ex-build-chain.png" alt="Example build chain"/>

## TeamCity Build Environment

## Terminology Mapping

Jenkins
GitLab CI
CircleCI
Travis CI