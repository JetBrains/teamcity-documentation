[//]: # (title: Terms and Concepts)
[//]: # (auxiliary-id: Terms and Concepts;Introduction to TeamCity Terminology;Concepts)

The following guide explains the main TeamCity terms and concepts. For a quick reference, see also [Basic TeamCity concepts](continuous-integration-with-teamcity.md#Basic+TeamCity+concepts).

>Use the documentation tree in the sidebar to navigate between the articles of this section — they describe each concept in detail.

## TeamCity Object Hierarchy

_[Projects](project.md)_ are the topmost objects of the TeamCity hierarchy, and _Root project_ is the main default project that contains all your custom projects and their subprojects. Each project serves for storing logically related _[build configurations](managing-builds.md)_ and defining their common settings and parameters. Most often, a TeamCity project corresponds to a single software product or its specific version.

Projects can be configured manually or automatically, from a VCS repository. The settings of each repository used in a project are stored in a dedicated preset called _[VCS root](vcs-root.md)_. Besides VCS roots, a project contains other settings shared between its nested subprojects and build configurations: _[cloud profile connections](teamcity-integration-with-cloud-solutions.md)_, _[common parameters](levels-and-priority-of-build-parameters.md)_, _[clean-up rules](teamcity-data-clean-up.md)_, and [many others](creating-and-editing-projects.md).
{instance="tc"}

Projects can be configured manually or automatically, from a VCS repository. The settings of each repository used in a project are stored in a dedicated preset called _[VCS root](vcs-root.md)_. Besides VCS roots, a project contains other settings shared between its nested subprojects and build configurations: _[common parameters](levels-and-priority-of-build-parameters.md)_, _[clean-up rules](teamcity-data-clean-up.md)_, and [many others](creating-and-editing-projects.md).
{instance="tcc"}

Example of a simple object hierarchy in TeamCity:

<img src="ex-hierarchy.png" alt="Example object hierarchy" width="227"/>

A build configuration serves as a blueprint for running a certain job, or a _build_, based on the project's source code: either it is a compilation, testing, deployment — or all these stages one by one. A single build configuration can define settings that are used to run, for example, a nightly build or integration tests.   
In certain cases, you may need to create multiple build configurations that differ only in few details. For this purpose, you can compose a _[build configuration template](build-configuration-template.md)_ and use it to generate as many similar build configurations as needed. One of the popular use cases for using templates is testing and deploying software on different operating systems.

>In TeamCity terms, it is considered that a build configuration _belongs_ to its parent project and that each running build that uses this configuration as a blueprint _belongs_ to this build configuration. In certain contexts, you will notice that the terms _build_ and _build configuration_ are used interchangeably in the documentation. For example, if we describe two related builds from different configurations, we can perceive each _build_ both as a single running job and as a carrier of its build configuration settings.

Each build configuration can have _[build features](adding-build-features.md)_ and _[build steps](configuring-build-steps.md)_. A build feature is a piece of extra functionality available to a build: it could be a performance monitor, a reporting tool, or a support for pull requests. Some features are built in TeamCity and some can be obtained by installing external plugins. A build step is a logical stage of a build that is performed by a certain _[build runner](build-runner.md)_. TeamCity offers a wide range of different runners: from testing tools to Docker integration.

Another important setting included into a build configuration is a _[trigger](configuring-build-triggers.md)_. There are multiple types of triggers in TeamCity. Their common purpose is to run a build automatically when a preconfigured condition is satisfied, thus establishing automatic continuous integration of a software product.

TeamCity offers all these granular objects so you can configure a build process as flexibly as possible. However, they would make more sense if you have the means to interconnect them with each other. For this purpose, TeamCity provides two types of dependencies between build configurations (that is between their builds):
* _[artifact dependency](artifact-dependencies.md)_ — for sending _artifacts_ produced by one build to another build; an example of an artifact is a `.jar` application that is compiled by one build configuration and deployed by another.
* _[snapshot dependency](snapshot-dependencies.md)_ — for assigning multiple builds to the same source revision (commit) so the same project files are used on all the building stages.

By connecting builds from different configurations or even projects with dependencies, you can create a flexible _[build chain](build-chain.md)_. The TeamCity product itself is built with a complex chain of builds. It allows us to compile and test different software modules and deploy the resulting distribution files to the web.

Example of a build chain in TeamCity:

<img src="ex-build-chain.png" alt="Example build chain" width="700"/>

## TeamCity Build Environment

The [TeamCity server](install-and-start-teamcity-server.md) stores all the objects' settings, manages the _[build queue](working-with-build-queue.md)_, monitors the state of running builds, and performs many other tasks. You can install as many additional _[secondary servers](multinode-setup.md)_ as you need to ensure high availability and scalability of your setup.   
You can host the server on your own machine or get a [TeamCity Cloud](https://www.jetbrains.com/teamcity/cloud/) subscription so your server is automatically launched and maintained in the cloud.
{instance="tc"}

The TeamCity server stores all the objects' settings, manages the _[build queue](working-with-build-queue.md)_, monitors the state of running builds, and performs many other tasks.
{instance="tcc"}

A different piece of software is used for actually running builds — a _[build agent](build-agent.md)_. By default, you get three build agents with your TeamCity server but you can get more if required. A build agent software can be installed on a different machine or alongside the server.
{instance="tc"}

A different piece of software is used for actually running builds — a _[build agent](build-agent.md)_. You automatically get access to cloud build agents with your TeamCity Cloud server, but you can also install a build agent software on your own machines.
{instance="tcc"}

>Check out the [basic CI workflow in TeamCity](continuous-integration-with-teamcity.md#Basic+CI+Workflow+in+TeamCity) to see an example of a simple build run.

## Learn More

To learn more about TeamCity, explore this Help further:
* The current Concepts section lists the main TeamCity objects and terms. Use it as a reference to learn about unfamiliar notions.
* To properly install and configure your setup, refer to [Supported Platforms and Environments](supported-platforms-and-environments.md) and [Installation](install-and-start-teamcity-server.md).
{instance="tc"}
* If you are using TeamCity only for managing and viewing builds, make sure to read [this section](managing-builds.md).
* If you have a Project Administrator or Server Administrator access, use the [TeamCity Configuration and Maintenance](teamcity-configuration-and-maintenance.md) section to learn how to configure projects or how to use the Administration panel and maintain the server. This section also contains information on [licensing](licensing-policy.md) and [configuration as code in TeamCity](kotlin-dsl.md).
{instance="tc"}
* If you have a Project Administrator or Server Administrator access, use the [TeamCity Configuration and Maintenance](teamcity-configuration-and-maintenance.md) section to learn how to configure projects or how to use the Administration panel and maintain the server. This section also contains information on [configuration as code in TeamCity](kotlin-dsl.md).
{instance="tcc"}
* Available TeamCity add-ins are described [here](installing-tools.md).
* Under [Extending TeamCity](extending-teamcity.md), advanced users can learn how to [run build scripts for interacting with TeamCity](build-script-interaction-with-teamcity.md), how to [use TeamCity REST API](https://www.jetbrains.com/help/teamcity/rest/teamcity-rest-api-documentation.html), and more.
* If none of these sections answer your questions, try looking into [Troubleshooting](troubleshooting.md) and [How-to](how-to.md).

Additional information resources:

* [Key features of TeamCity](https://www.jetbrains.com/teamcity/features/)
* [CI/CD Guide](https://www.jetbrains.com/teamcity/ci-cd-guide/)
* [How to write custom TeamCity plugins](https://plugins.jetbrains.com/docs/teamcity/developing-teamcity-plugins.html)
* [TeamCity Roadmap](https://www.jetbrains.com/teamcity/roadmap/)