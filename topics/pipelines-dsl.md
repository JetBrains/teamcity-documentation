# Pipelines Kotlin DSL

In TeamCity 2026.1, pipelines owned by projects who store their settings in [](kotlin-dsl.md) automatically convert their YAML settings to Kotlin. The familiar **Visual**/**YAML** switch starts showing the additional **Kotlin DSL** tab.

<img src="pipelines-dsl.png" width="706" alt="DSL in pipelines"/>

This allows you to have a complete project DSL without any pipeline "gaps".

## DSL compatibility mode

Enabling versioned settings for a project that includes a pipeline imposes the following restrictions:

* If the [main pipeline repostirory](pipeline-settings.md#Repository) stored its YAML on a server, it switches to the **in VCS repository** mode.
* Edits made in the **Visual** or **YAML** tab are automatically converted to DSL, but cannot be committed automatically. You need to copy the generated DSL and manually commit it to the project `.kts` file.
* In addition, editing pipelines via TeamCity UI locks out the **Save and Run** button. A pipeline cannot run for as long as its current configuration differs from the remotely stored version. To make the pipeline fully funtional again, commit the changes manually as described above and hit **Cancel**: when TeamCity loads versioned settings from a VCS, a pipeline will apply the latest changes and the **Run** button will be available.

This behavior corresponds to the similar behavior of classic build configurations when the [**Allow editing in the UI**](storing-project-settings-in-version-control.md) checkbox is off. 

## Pipeline DSL entities

Pipeline-specific objects are declared in the `jetbrains.buildServer.configs.kotlin.pipelines` namespace.


<deflist type="medium">

<def title="Pipeline">

Represents a pipeline.

???LINK???

</def>

<def title="Job">

Represents a job.

???LINK???

</def>

<def title="Repository">

Represents a repository that pipeline jobs can process. Each of **Pipeline settings | Repositories** entries correspond to a separate object of this type.

???LINK???

</def>

</deflist>

