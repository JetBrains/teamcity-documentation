[//]: # (title: Create and Setup Custom Parameters)
[//]: # (auxiliary-id: Changing Build Parameter Type and UI Appearance;Typed Parameters)

This topic explains how to create custom TeamCity parameters and configure their appearance and behavior.

## Name Restrictions

The names of configuration parameters must contain only the `[a-zA-Z0-9._-*]` characters and start with an ASCII letter.

## How to Create a New Parameter

### In TeamCity UI

1. Go to the **Project Settings** or **Build Configuration Settings** page and switch to the **Parameters** tab. See this article for the information on parameters priority and inheritance rules: [](levels-and-priority-of-build-parameters.md).

2. Click the **Add new parameter** button.

3. Specify the parameter kind and enter the parameter name. See this article for more information on the difference between different parameter types: [](configuring-build-parameters.md).

    <img src="dk-params-createNewParam.png" width="706" alt="Create New Parameter"/>

4. Optional: If your custom parameter should have a default value, enter it in the corresponding field. You can also leave this field empty if the final parameter value should be set in child projects or configurations, calculated during a build, or if you need different agents to report different values for this parameter. See the following article to learn more about available value sources: [](levels-and-priority-of-build-parameters.md).

5. Optional: Set up the required parameter specification. Using parameter specs you can force users to enter parameter value each time they start a build, hide parameter values from TeamCity UI and REST API requests, and more. Refer to corresponding sections of this document to learn more.


### In Kotlin DSL

To define a custom parameter in [](kotlin-dsl.md), add the `param("prefix.name", "value")` line to the `params` section of a project or build configuration.

```Kotlin
import jetbrains.buildServer.configs.kotlin.*

// Project-level Parameters
project {
   params {
      param("env.ProjectLevelParam", "/System/DriverKit")
      param("ProjectLevelParam", "true")
   }
}

// Build Configuration-level Parameters
object MyBuildConf : BuildType({
   params {
       param("ConfigLevelParam", "24")
       param("env.ConfigLevelParam", "CTP")
   }
})
```


### Via REST API

To create a parameter via [](teamcity-rest-api.md), send the POST request to the required endpoint and pass a [Property](https://www.jetbrains.com/help/teamcity/rest/property.html) as a request body.

```Shell
POST <SERVER_URL>/app/rest/projects/MyProject/parameters
# or
POST <SERVER_URL>/app/rest/buildTypes/MyProject_MyConfig/parameters
```

Request body:

<tabs>

<tab title="XML">

```XML
<property name="parameter.from.rest" value="custom_value"/>
```

</tab>

<tab title="JSON">

```JSON
{
  "name" : "parameter.from.rest",
  "value" : "custom_value"
}
```

</tab>

</tabs>


You can also send requests to the `/app/rest/buildQueue` endpoint to create one-time parameters for a single build run only. The following request starts a new build and adds a [password parameter](typed-parameters.md#Password) to it.

```Shell
/app/rest/buildQueue
```
{prompt="POST"}

<tabs>

<tab title="XML">

```XML
<build>
   <buildType id="MyBuildConfID"/>
   <properties>
      <property name="env.password" value="mySecret">
         <type rawValue="password"/>
      </property>
   </properties>
</build>
```

</tab>

<tab title="JSON">

```JSON
{
   "buildType": {
      "id": "MyBuildConfID"
   },
   "properties": {
      "property": [{
         "name": "env.password",
         "value": "mySecret",
         "type": {
            "rawValue": "password"
         }
      }]
   }
}
```

</tab>

</tabs>


See this article to learn more about managing parameters via REST API: [Manage Typed Parameters](https://www.jetbrains.com/help/teamcity/rest/manage-typed-parameters.html).


### In the Default Properties File

This method allows you to declare parameters available only for those build configurations that share the same VCS root. Parameters defined this way are not visible in the TeamCity UI and passed directly to the build process.

1. Create a text file with the `teamcity.default.properties` name.
2. Populate it with parameters in the `system.<name>=<value>` or `env.<name>=<value>` format. For example, `env.CATALINA_HOME=C:\tomcat_6.0.13`.
3. Check in this file to the root directory of the target repository.
4. Set up required [checkout settings](configuring-vcs-settings.md#Configure+Checkout+Rules) and ensure the file is checked out to the [](build-working-directory.md).

If needed, you can modify the name and the path to the properties file via the `teamcity.default.properties` parameter of a build configuration.


## Parameter Specifications
{id="Adding+Parameter+Specification"}

Setting up a parameter specification allows you to alter the parameter behavior and appearance.

### Label and Description

These fields allow you to set up custom strings for the **Run Custom Build** dialog.

<img src="dk-params-LabelAndDescription.png" width="706" alt="Custom parameter label and description"/>

If these values are not specified, TeamCity leaves the parameter description empty and uses the regular parameter name as its label.

### Display Modes

The **Display** setting controls whether the parameter should be visible in the [Run Custom Build](running-custom-build.md) dialog. The default parameter display mode is **Normal**.

#### Hidden

Parameters with this display mode are not shown in the **Run Custom Build** dialog when a user triggers a custom build. Choose this mode to hide parameters you do not want users to see.

If you want to prevent users from editing a parameter value but still leave the parameter visible, leave the display mode as "Normal" and tick the **Read-only** checkbox instead.

#### Prompt

If a build configuration has a parameter of the "Prompt" type, every time a user triggers a new build the **Run Custom Build** dialog appears. This ensures a user sets a valid value for all "Prompt" parameters before the build process starts.

For example, the following parameter adds a run confirmation to your build configuration: users cannot start new builds unless they type "deploy" in the corresponding field.

<img src="dk-params-startPrompt.png" width="706" alt="Starting Prompt"/>

```Kotlin
object MyBuildConf : BuildType({
params {
    text("launch.confirmation", "",
            label = "Deployment build confirmation",
            description = "This configuration triggers the deployment chain, which uploads updated NuGet packages and Docker images to public sources. Do you want to continue?",
            display = ParameterDisplay.PROMPT,
            regex = "deploy",
            validationMessage = """Type "deploy" to run this build""")
    }
})
```

### Editor Types

The **Type** setting allows you to choose the editor displayed next to the parameter label in the **Run Custom Build** dialog.

#### Text

This is the default type. Parameters of this type display a regular text box. Choose this type if you want to apply an input mask for user values.

For example, the following parameter uses a regular expression to accept only e-mail addresses:

<img src="dk-params-TextParameterRegex.png" width="706" alt="Text parameter with regex"/>

```Kotlin
object PromptTest : BuildType({
   params {
       text("a.text.mail.param", "", label = "Email Address",
               regex = """^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}${'$'}""",
               validationMessage = "Invalid e-mail address")
   }
})
```

#### Checkbox

Choose this type to display a checkbox next to the parameter label.

<img src="dk-params-CheckboxParam.png" width="706" alt="Checkbox parameter"/>

By default, if the editor is checked the parameter value is `true`; otherwise, the parameter has no value. Use the **Checked value** and **Unchecked value** to provide your own values.

The initial checkbox state depends on the default parameter value.

> The editor supports only checked and unchecked states, the indeterminate state is not allowed. However, if you set the default parameter value to neither "Checked value" nor "Unchecked value", the parameter can report three potential values.
>
> ```Kotlin
> object PromptTest : BuildType({
>    params {
>       checkbox("checkbox.param", "value-not-set",
>       label = "Checkbox parameter", description = "",
>       checked = "checked", unchecked = "unchecked")
>    }
> })
> ```
> The default value applies during normal builds, checked and unchecked values used in custom build runs.
>
{type="tip"}

<anchor id="Password+Type"/>

#### Password

Choose this editor type to hide the parameter value.

<img src="dk-params-password.png" width="706" alt="Password parameter"/>

Password parameter values hidden not only from the TeamCity UI, but also from DSL code (visible via the **View as code** button and saved to a remote repository when you enable [versioned settings](storing-project-settings-in-version-control.md)).

```Kotlin
object PromptTest : BuildType({
   params {
       password("a.password.param", "******", label = "Registry password")
   }
})
```

Requesting parameters via REST API also returns a payload without parameter values.

```Shell
<SERVER_URL/app/rest/buildTypes/MyBuildConfig/parameters?locator=name:password.param
```
{prompt="GET"}

Response payload:

<tabs>

<tab title="XML">

```XML
<properties count="1">
    <property name="password.param">
        <type rawValue="password display='normal' label='Registry password'"/>
    </property>
</properties>
```

</tab>

<tab title="JSON">

```JSON
{
    "count": 1,
    "property": [
        {
            "name": "password.param",
            "type": {
                "rawValue": "password display='normal' label='Registry password'"
            }
        }
    ]
}
```

</tab>

</tabs>


If you need to create a password parameter not in TeamCity UI, you can use [secure value tokens](storing-project-settings-in-version-control.md#Managing+Tokens) instead to set up parameter values.

```Kotlin
params {
   password("my.password.param", "credentialsJSON:<token>")
}
```

#### Select

Choose this editor type to display a combobox next to the parameter label and limit the range of possible values by multiple predefined options.

<img src="dk-params-selectParameter.png" width="706" alt="Select parameter"/>

Each item of the editor drop-down menu has a publicly visible label and an underlying value assigned to the parameter. Use the `label => value` syntax to set different strings, or `value` if you want them to match.

The following sample illustrates a parameter that allows users to select a value passed to the [](vcs-labeling.md) build feature.

```Kotlin
object MyBuildConf : BuildType({
    name = "Select Parameter Sample"

    params {
        select("release.status", "", label = "Deployment Tag",
                options = listOf("EAP" to "eap",
                        "CTP" to "ctp",
                        "Alpha preview" to "alpha",
                        "Beta preview" to "beta",
                        "Release" to "release")
        )
    }
   
features {
    vcsLabeling {
        vcsRootId = "${MavenRepoRootGithub.id}"
        labelingPattern = "%\release.status%"
    }
}
})
```

If the **Allow multiple selection** option is enabled, users can tick multiple options at once.

<img src="dk-params-multiselect.png" width="706" alt="Multiselect parameter"/>

In this case, the parameter value is a string that contains values of all selected items separated by a given char.



 <seealso>
        <category ref="admin-guide">
            <a href="configuring-build-parameters.md">Configuring Build Parameters</a>
            <a href="using-build-parameters.md">Using Build Parameters</a>
            <a href="predefined-build-parameters.md">List of Predefined Build Parameters</a>
        </category>
</seealso>