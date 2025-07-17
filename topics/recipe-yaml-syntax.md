[//]: # (title: Recipe YAML Syntax)
[//]: # (help-id: Recipe YAML Syntax)

This document describes general [YAML recipe](working-with-meta-runner.md) syntax. To create a custom YAML recipe, write an .yml definition file and upload it to TeamCity ([**Project Settings**](project-administrator-guide.md#Edit+and+View+Modes) | **Recipes** | **Upload private recipe**).

> XML recipes (former meta-runners) are the legacy counterpart of YAML recipes with a different syntax. You can create an XML recipe by [extracting it from a source build configuration](working-with-meta-runner.md#Extract+a+Recipe+From+a+Build+Configuration) and modify the resulting XML file.
> 
{style="tip"}


## Common Recipe Structure

A TeamCity YAML recipe has the following structure:

```yaml
name: myorg/recipe-name
title: The recipe title
version: 1.2.3
description: The recipe description
container: <container properties>
inputs: <input list>
steps: <step list>
```

<deflist type="narrow">

<def title="name">

**Mandatory:** yes<br/>
**Type:** string

The unique recipe name. The value must be in the `namespace/recipe-name` format, where `namespace` is the name of a JetBrains Marketplace owned by a recipe author. All recipes uploaded by the same user should have identical namespace. See the following Marketplace documentation article to learn more about managing your user namespaces: [Uploading TeamCity Recipes](https://plugins.jetbrains.com/docs/marketplace/uploading-a-new-plugin.html#upload-teamcity-recipes).

Recipe and namespace names must be 5–30 characters long, using only alphanumeric characters, dashes, or underscores. They cannot start with a dash or underscore, nor contain consecutive dashes or underscores.

If you do not plan to upload a recipe to JetBrains Marketplace and only intend it to use as a private recipe, you can skip the `namespace` portion. When used alone, `recipe-name` has a limit of 100 characters (alphanumeric, dashes, and underscores).

</def>

<def title="title">

**Mandatory:** yes (only for recipes uploaded to the Marketplace)<br/>
**Type:** string

A public recipe name shown in TeamCity UI (on the [Add Build Step](configuring-build-steps.md) page, in the build log, and so on).

</def>

<def title="version">

**Mandatory:** yes (only for recipes uploaded to the Marketplace)<br/>
**Type:** string

The numerical recipe version in the `major.minor.patch` format. `Minor` and `patch` portions are optional for private recipes and mandatory for public ones.

</def>


<def title="description">

**Mandatory:** yes<br/>
**Type:** string

The recipe public description. Unlimited for private recipes and maximum 1000 characters for public ones.

</def>


<def title="container">

**Mandatory:** no<br/>
**Type:** [ContainerSettings](#Container+Settings) object or string

A set of properties that specify a Docker or Podman container in which this recipe should be executed.

<tip>If you want to run every build step of a build configuration inside the same container, configure the <a href="run-in-docker.md">Run in Docker</a> feature instead.</tip>

</def>


<def title="inputs">

**Mandatory:** no<br/>
**Type:** [List&lt;Input&gt;](#Input)

A set of editors (text fields, checkboxes, combo-boxes, and so on) displayed in TeamCity UI when configuring a recipe.

</def>


<def title="steps">

**Mandatory:** yes<br/>
**Type:** [List&lt;Step&gt;](#Step)

A list of build steps executed by a recipe when a build runs.

</def>

</deflist>




## Container Settings

The recipe `container` field has the following syntax:

```yaml
container:
  image: alpine
  platform: linux
  parameters: -it
```

If `image` is the only required field, you can use a compact format instead:

```yaml
container: alpine
```

Unless you specify a public DockerHub image name, a build configuration that runs a recipe inside a container must have the [](configuring-connections.md#Docker+Registry) connection to access a registry.

<deflist type="narrow">

<def title="image">

**Mandatory:** yes<br/>
**Type:** string

The name of Docker/Podman image from which a container is spawned. 

</def>

<def title="platform">

**Mandatory:** no<br/>
**Type:** string<br/>
**Supported values**: `linux` | `windows`

Specifies the container image platform.

</def>


<def title="parameters">

**Mandatory:** no<br/>
**Type:** string

The list of additional container run parameters.

</def>

</deflist>




## Input

An input is a variable used by recipe steps and customizable by the user. When a standalone recipe is added directly to a build configuration, inputs can be configured through UI editors based on their type (text box for text inputs, combo box for select, and so on).

<img src="dk-recipe-customization.png" width="706" alt="Inputs customization for recipes"/>

Recipes can also reference other recipes via the [uses](#recipe-type-uses) field. In that case, input values are provided in YAML instead of the TeamCity UI.


Each input has the following format:

```yaml
<input-name>:
  type: text
  label: Main label
  description: Input description
  default: value1
  required: true
```

In a recipe .yml file, individual input definitions are placed inside the `inputs` block:

```yaml
inputs:
  - env.input_address:
      ...
  - env.input_subject:
      ...
  - env.input_message:
      ...
```

<deflist type="narrow">

<def title="&lt;input-name&gt;">

**Mandatory:** yes<br/>
**Type:** string

The name of a [build parameter](configuring-build-parameters.md) that stores the input value. Declared as a raw value (without the field name).

You can use both environment variables and regular build parameters as input names.

<procedure title="Environment variables (recommended)">

* Declared with the `env.` prefix. For example, `env.my-input`.
* Steps can retrieve environment variable values without using parameter references. For example,`INPUT_VAR="$my-input"` in Bash script, `System.getenv("my-input")` in Kotlin script, and so on.
* Recommended for any input except for those asking for user credentials or other sensitive data. Sensitive values should not be stored in environment variables as they expose these values to all child processes and system tools, which presents a security concern.

</procedure>


<procedure title="Regular build parameters">

* Declared without any prefix. For example, `my-input`.
* Steps must use parameter references to retrieve their values (for example, `string myVal = "%\my-input%";`). This syntax can be abused to inject malicious code disguised as an input value. For that reason, regular build parameters are recommended for trusted user inputs only.

</procedure>


</def>

<def title="type">


**Mandatory:** yes<br/>
**Type:** string<br/>
**Supported values**: `text` | `boolean` | `select` | `password`

The input type. Specifies what value this input can have, as well as appearance and behavior options for the corresponding editor in TeamCity UI.

* `text` — the default input type. Allows the input to have any value. Displays a text box in recipe settings.
* `boolean` — limits the number of available input values to either **true** or **false**. TeamCity shows checkboxes in recipe settings for inputs of this type.
* `select` — allows you to specify a fixed range of values that users can choose from. In TeamCity UI, rendered as a combo box editor. Requires an additional `options` field that lists available values:
   ```yaml
   inputs:
      - env.retry_timeout:
           type: select
           label: Retry timeout
           required: false
           default: 60
           options:
             - 5
             - 10
             - 30
             - 60
   ```
* `password` — similar to `text`, but masks the input value with asterisks in both TeamCity UI and build logs.


</def>

<def title="label">

**Mandatory:** no<br/>
**Type:** string

The main input label displayed next to the editor in TeamCity UI. The maximum length for public recipes is 100 characters.

</def>


<def title="description">

**Mandatory:** no<br/>
**Type:** string

The input description displayed below the editor in TeamCity UI. The maximum length for public recipes is 250 characters.

</def>


<def title="default">

**Mandatory:** no<br/>
**Type:** string

The initial/default input value. Steps will use this value if a user did not provide a custom one. If not set, set the `required` field to **true**.

</def>


<def title="required">

**Mandatory:** no<br/>
**Type:** Boolean

Returns **true** if a user must specify the input value on the TeamCity recipe settings page; otherwise, **false**. Enable this setting if an input has no `default` value.

</def>

</deflist>

## Step

A single set of actions performed during a build. While a recipe is a generalized build step that uses custom versions of [standard TeamCity build steps](configuring-build-steps.md), YAML recipes are designed to be universal and compatible with any TeamCity agent regardless of the installed tools. For that reason, recipe steps can currently use only [](kotlin-script.md), [](command-line.md) steps, and other YAML recipes.

A typical step looks like the following:

```yaml
name: My step
script/kotlin-script/uses: <value>
<containerSettings>
```

In a recipe .yml file, individual step definitions are placed inside the `steps` block:

```yaml
steps:
   - name: First step
     ...
   - name: Second step
      ...
   - name: Third step
      ...
```

<deflist type="narrow">

<def title="name">

**Mandatory:** no<br/>
**Type:** string

The public step name. Unlimited for private recipes and maximum 100 characters for public ones.

</def>


<def title="script">

**Mandatory:** no<br/>
**Type:** string<br/>

Specifies a step that executes a custom script using the TeamCity [](command-line.md) step. The maximum script length for public recipes is 50000 characters. For example, the following step prints "Hello world" to the build log:

```yaml
name: Script step
script: echo "Hello world"
```

</def>


<def title="kotlin-script">

**Mandatory:** no<br/>
**Type:** string<br/>

Specifies a step that executes a custom Kotlin script using the TeamCity [](kotlin-script.md) step. The maximum script length for public recipes is 50000 characters. For example, the following step prints "Hello world" to the build log:

```yaml
name: Kotlin script step
kotlin-script: print("Hello world")
```

</def>


<def title="uses" id="recipe-type-uses">

**Mandatory:** no<br/>
**Type:** string<br/>

References another recipe installed on this TeamCity server and available for the same project where the current recipe is used. The field value depends on whether a referenced recipe is a public or a private one:

* Public recipe: `uses: namespace/recipe-name@major.minor.patch`.
* Private recipes: `uses: private/recipe-name`. 

If the referenced recipe has required inputs, you can specify their values in the `inputs` block.

The following recipe step executes the `jetbrains/some_recipe` recipe of version "1.2.3", and passes **foo** and **bar** values to `some_recipe` inputs "input1" and "input2" respectively.

```yaml
name: Custom step
uses: jetbrains/some_recipe@1.2.3
inputs:
   input1: foo
   input2: bar
```

</def>


<def title="container">

**Mandatory:** no<br/>
**Type:** [ContainerSettings](#Container+Settings) object or string<br/>

Specifies a Docker or Podman container in which this step should be run. Overrides the recipe-wide `container` field for this step.

The recipe step below runs in the "alpine" container for Linux:

```yaml
name: Container script step
container:
   image: alpine
   platform: linux
script: echo "Hello world"
```

You can use a short format that specifies only image name:

```yaml
name: Container script step
container: alpine
script: echo "Hello world"
```

</def>



</deflist>



## Example

The following YAML markup declares a single-step recipe with two inputs. The recipe step executes a Kotlin script uses the `input_name` and `input_value` input values to craft a [setParameter service message](service-messages.md#set-parameter). When this service message prints to a build log, TeamCity finds the `name` parameter and assigns `value` to it.

This recipe is created by the TeamCity team and is available on the JetBrains Marketplace: [jetbrains/set-environment-variable](https://plugins.jetbrains.com/plugin/26673-jetbrains-set-environment-variable).

```yaml
name: jetbrains/set-environment-variable
version: 1.0.0
description: |
  Sets the environment variable to the required value, creating it if absent.
inputs:
  - env.input_name:
      type: text
      required: true
      label: Name
      description: |
        The name of the environment variable to change, without the “env.” prefix. 
        Examples: “PATH”, “JAVA_HOME”, “DOTNET_ROOT”.
  - env.input_value:
      type: text
      required: true
      label: Value
steps:
  - name: Set environment variable
    kotlin-script: |-
      
      @file:Repository("https://download.jetbrains.com/teamcity-repository/")
      @file:DependsOn("org.jetbrains.teamcity:serviceMessages:2024.12")
      
      import jetbrains.buildServer.messages.serviceMessages.ServiceMessage
      import jetbrains.buildServer.messages.serviceMessages.ServiceMessageTypes.BUILD_SET_PARAMETER
      
      val message = ServiceMessage.asString(
          BUILD_SET_PARAMETER,
          mapOf(
              "name" to "env.${requiredInput("name")}",
              "value" to requiredInput("value")
          )
      )
      println(message)
      
      fun requiredInput(name: String) = System.getenv("input_$name") ?: error("Input '$name' is not set.")
```