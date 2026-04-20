# Pipelines Kotlin DSL

In TeamCity 2026.1, pipelines owned by projects who store their settings in [](kotlin-dsl.md) automatically convert their YAML settings to Kotlin. The familiar **Visual**/**YAML** switch starts showing the additional **Kotlin DSL** tab.

<img src="pipelines-dsl.png" width="706" alt="DSL in pipelines"/>

This allows you to have a complete project DSL without any pipeline "gaps".

## DSL compatibility mode

Enabling versioned settings for a project that includes a pipeline imposes a few restrictions, depending on where your pipeline stores its settings.

* If the [main pipeline repository](pipeline-settings.md#Repository) stored its YAML on a server when you enabled project versioned settings, it switches to the **in VCS repository** mode and becomes partly editable via TeamCity UI. Edits made in the **Visual** or **YAML** tab are automatically converted to DSL, but cannot be saved or committed automatically. Copy the generated DSL and manually commit it to the project `.kts` file. Until you do so, a pipeline cannot run because its current configuration mismatches the one stored in a remote file.

* If the main repository already stores its configuration file as a remote YAML file, the pipeline remains fully editable via TeamCity UI. However, the project `.kts` file will not include this pipeline's Kotlin DSL, so the project configuration file will remain incomplete.


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

