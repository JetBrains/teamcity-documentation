# Code-first Workflows

Previous tutorials explained how to start your TeamCity project from the UI. However, many software developers prefer to design CI/CD workflows similarly to how a product is developed: via code stored next to the source files. In TeamCity, you can design code-first workflows using YAML and Kotlin DSL.

* **YAML** — simple markup most suitable for moderately complex workflows. Currently supported only in [pipelines](create-and-edit-pipelines.md). See the [](pipelines-yaml-syntax.md) article for the complete YAML schema.

* [**Kotlin DSL**](kotlin-dsl.md) — a fully fledged programming language with all corresponding benefits: the ability to dynamically generate objects, create reusable templates and libraries shared across projects (or even TeamCity servers), full IDE support along with refactoring tools and auto-completion, and so on. Supported in both pipelines and [build configurations](creating-and-editing-build-configurations.md). You can access kDSL API reference documentation locally, by adding `/app/dsl-documentation/index.html` to your TeamCity server URL, or at our public [teamcity.jetbrains.com](https://teamcity.jetbrains.com/app/dsl-documentation/index.html) server.


## Walkthrough

This walkthrough guides you through the basic steps of adding a code-first pipeline.

1. Add a `.teamcity.yml` file to the root of your repository and design a pipeline. Refer to [](pipelines-yaml-syntax.md) for complete schema.

> For this tutorial, you can also fork our [sample repository](https://github.com/JetBrains/teamcity-demo-pipeline-simple) that includes a YAML file for a three-job pipeline.
>
> <img src="sample-demo-pipeline.png" width="706" alt="Sample pipeline"/>
>
> This pipeline showcases a variety of TeamCity concepts, including:
> 
> * [Input and output parameters](pipeline-settings.md#Parameters)
> * [Job reuse](job-settings.md#Optimizations)
> * [Service messages](service-messages.md) that send commands to TeamCity from build scripts
> * [Shared files and artifacts](job-settings.md#Output+Files)
> * Test reporting and history

2. Create a new pipeline that targets the repository with this YAML file.

    <img src="create-sample-pipeline.png" width="706" alt="Create pipeline"/>

3. TeamCity will detect an existing configuration file and ask whether you want to start from scratch, or import pipeline settings from this file. Choose the import option and click **Confirm**.

   <img src="sample-pipeline-yml-import.png" width="706" alt="Import pipeline YAML"/>

4. TeamCity will automatically sync changes made to this configuration file and in the UI. You can disable this sync in the pipeline **Repository** section by choosing to keep the settings file on the server.



## Project versioned settings and pipelines

In TeamCity, pipelines and configurations only store settings specific to them. Objects like [VCS roots](configuring-vcs-roots.md) and project-wide [connections](configuring-connections.md) are store in project settings instead. These settings are available on the [**Project settings | Versioned settings**](storing-project-settings-in-version-control.md) page. Enabling them allows you to store settings the entire project (with all of its child entities) in the same repository.

Note that since YAML is only supported for pipelines, you cannot use TeamCity UI to add a pipeline to a project that stores its settings remotely. If versioned settings are enabled for a project that already has a pipeline, and that pipeline already stores its settings in a VCS .yml file (see [pipeline **Repository** settings](pipeline-settings.md#Repository)), project kDSL will not include settings of this pipeline.

<img src="pipeline-yaml-not-included-warning.png" width="706" alt="YAML not included in kDSL warning"/>

See also: [](pipelines-dsl.md#DSL+compatibility+mode).