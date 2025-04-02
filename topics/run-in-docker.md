# Run in Docker

The **Run in Docker** build feature allows you to run all steps of a build configuration in the same Docker or Linux container. Below is the list of build steps that can be launched inside a container: 

<include from="container-wrapper.md" element-id="supported-docker-wrapper-steps"/>

Steps that are not included in this list will be run outside of the specified container.

<include from="common-templates.md" element-id="docker-integration-note"><var name="docker-feature-name" value="Run in Docker"/></include>

## Settings

TeamCity provides two options to run build steps inside containers:

* **Run in Docker** build feature — specifies global container settings common for all build steps of this configuration.
* [](container-wrapper.md) — allows you to run one individual step in the required container.

Both "Run in Docker" and "Container Wrapper" expose identical settings.

<var name="first_setting_name" value="Image name"/>
<var name="docker_settings_type" value="build configuration"/>

<include from="container-wrapper.md" element-id="docker-wrapper-and-feature-settings"/>


## Kotlin DSL

The snippet below illustrates how to configure the **Run in Docker** feature in [](kotlin-dsl.md):


```Kotlin
object BuildConf : BuildType({
    name = "BuildConf"
    // ...
    features {
        runInDocker {
            dockerImage = "python:latest"
            dockerImagePlatform = RunInDockerBuildFeature.ImagePlatform.Linux
            dockerPull = true
            dockerRunParameters = "-a stdin -a stdout -i -t ubuntu /bin/bash"
        }
    }
})
```