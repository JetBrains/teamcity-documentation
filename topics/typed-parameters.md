[//]: # (title: Create and Set Up Custom Parameters)
[//]: # (help-id: Changing Build Parameter Type and UI Appearance;Typed Parameters)


This topic explains how to create custom TeamCity parameters and configure their appearance and behavior.

## Name Restrictions

The names of configuration parameters must contain only the `[a-zA-Z0-9._-*]` characters and start with an ASCII letter.

## How to Create a New Parameter

### In TeamCity UI

1. Go to [Project or Configuration settings](project-administrator-guide.md#Edit+and+View+Modes) and switch to the **Parameters** tab. See this article for information on parameter priority and inheritance rules: [](levels-and-priority-of-build-parameters.md).

2. Switch to the **Input Parameters** tab. Output parameters are used in [build chains](build-chain.md) exclusively, see this article for more information: [](use-parameters-in-build-chains.md).

3. Click the **Add new parameter** button.

    <img src="dk-params-createNewParam.png" width="706" alt="Create New Parameter"/>

4. Specify the parameter kind and enter the parameter name. See this article for more information on the difference between different parameter types: [](configuring-build-parameters.md).

5. Choose the required **Value type** option. These options control what values a parameter can have.

   * **Text** — the default type that allows the parameter to have any string value. You can optionally choose a required option under **Show allowed value** to limit the allowed values to only those that match the specific RegEx pattern, or ensure a parameter is never empty.

   * **Checkbox** — limits the number of possible parameter values to two. Rendered as a checkbox in the [Run Custom Build](running-custom-build.md) dialog, allowing users to toggle between these values. The default values for checked and unchecked states are `true` and `null` respectively. You can set up your custom value pairs (yes/no, 1/0, debug/release, and so on) via the **Checked value** and **Unchecked value** fields.
    {help-id="CheckboxParameter"}

   * **Password** — similar to the "Text" type, "Password" parameters can accept any string as a value. However, this value is never exposed outside a build: TeamCity hides this sensitive value from the UI, build logs, DSL code, and REST API response payloads.

      <p instance="tc">Passwords are stored in the configuration files under <a href="teamcity-data-directory.md">TeamCity Data Directory</a>. Depending on the <a href="teamcity-configuration-and-maintenance.md#encryption-settings">server encryption settings</a>, the value is either scrambled or encrypted with a custom key.</p>
      
      > Password values are hidden from the build log by a plain search-and-replace algorithm. If you have a trivial password such as "123", all occurrences of the "123" string will be replaced in the log, which could potentially expose the password.
      > 
      > It is also important to remember that setting the parameter to the "Password" type does not completely guarantee the safety of your data. Any project administrator can retrieve the parameter's raw value, and any developer who can change the build script can potentially write malicious code to leak the password.
      >
      {style="warning"}

   * **Select** — allows you to specify a set of predefined values. Users that invoke the [Run Custom Build](running-custom-build.md) dialog can choose one or multiple values from the list, depending on the **Allow multiple selection** value. Values can be supplied with optional values displayed in TeamCity UI (for example, `Windows => win`).

   * **Remote secret** — a parameter whose value cannot be entered manually. Instead, a value is securely retrieved from a remote storage when the running builds needs this value. See the following article to learn more: [](hashicorp-vault.md).

6. Optional: Click **Customize settings for the "Run Custom Build" dialog** to specify additional options that affect users who run [custom builds](running-custom-build.md).

   * **Display** — specifies whether users can (or should) edit this parameter.

      * **Normal** parameters are default parameters that are shown in the [Run Custom Build](running-custom-build.md) dialog.
      * **Hidden** parameters are not visible in the [Run Custom Build](running-custom-build.md) dialog. Use this type for service parameters that you do not want users to see. Unlike with secret parameters, it is possible to echo a value of a hidden parameter to a build log, request it via REST API, and so on.

      * **Prompt** parameters invoke the [Run Custom Build](running-custom-build.md) dialog every time users trigger a new build to ensure a valid value is provided for each run. You can also use this type to implement custom confirmation dialogs (see the Examples section below).

   * **Description** and **Label** fields allow you to add hints that help users choose a correct parameter value.
   
      <img src="dk-newparameters-labelanddescription.png" width="706" alt="Parameter Label and Description"/>

   * **Read-only** parameters display disabled editors in the [Run Custom Build](running-custom-build.md) dialog, which prevents users from changing parameter values. If along with locking the value you also want to hide this parameter from users, set the **Display** option to **Hidden**.
   
7. Optional: If your custom parameter should have a default value, enter it in the corresponding field. You can also leave this field empty if the final parameter value should be set in child projects or configurations, calculated during a build, or if you need different agents to report different values for this parameter. See the following article to learn more about available value sources: [](levels-and-priority-of-build-parameters.md).




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


### From a Build Step

Send the `##teamcity[setParameter name='ddd' value='fff']` [service message](service-messages.md#set-parameter) to update a parameter value or, if a parameter with this name does not yet exist, create a new one.

See this article for more information: [](service-messages.md#set-parameter).



### Create a Secret

A secret is a parameter that holds sensitive data and must be securely stored to prevent leaks. Depending on your scenario and intended usage, you have the following options.


<deflist>
<def title="Password parameter">
If you want a secret value to be stored and used within a specific configuration or project, create a regular input parameter with the <b>Password</b> type as described in the <a href="typed-parameters.md#In+TeamCity+UI"/> section.
</def>


<def title="Remote secret">
If you have an external secrets vault that stores values, create parameter of the <b>Remote secret</b> type. With an additionally configured vault connection, these parameters can securely retrieve values on demand.

Currently, only HashiCorp Vault secrets storage is supported.

See the following article to learn more: <a href="hashicorp-vault.md"/>
</def>

<def title="Token">
If your project stores settings <a href="storing-project-settings-in-version-control.md">in a remote VCS</a>, you may need to configure secret values without exposing them. To do this, create a server-side token to store the secret, then pass its name as a parameter value.

See this topic for more information: <a href="storing-project-settings-in-version-control.md#Managing+Tokens"/>
</def>


<def title="Scrambled secret">
You can also create tokens without using versioned settings. In this case, a token can only be used inside project configuration files stored on a disk (inside the <a href="teamcity-data-directory.md"/>).

To create such token, invoke the project <b>Actions</b> menu and click <b>Scramble secure value...</a>.

<img src="dk-scramble-value.png" width="706" alt="Scramble value"/>

In a dialog that pops up, enter a secret value and hit <b>Scramble</b>. Remember to copy the scrambled value before closing the dialog.
</def>
</deflist>



### Using REST API

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


You can also send requests to the `/app/rest/buildQueue` endpoint to create one-time parameters for a single build run only. The following request starts a new build with a new password parameter.

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

This method allows you to declare parameters available only for those build configurations that share the same VCS root. Parameters defined this way are not visible in the TeamCity UI and are passed directly to the build process.

1. Create a text file with the `teamcity.default.properties` name.
2. Populate it with parameters in the `system.<name>=<value>` or `env.<name>=<value>` format. For example, `env.CATALINA_HOME=C:\tomcat_6.0.13`.
3. Push this file to the root directory of the target repository.
4. Set up required [checkout settings](configuring-vcs-settings.md#Configure+Checkout+Rules) and ensure the file checks out to the [](build-working-directory.md).

You can modify the name and path to the properties file via the `teamcity.default.properties` parameter of a build configuration.


## Examples


### Checkbox parameter

This parameter shows a checkbox in the **Run Custom Build** dialog. The parameter can be toggled between `release` (checked) and `debug` (unchecked) values.

<img src="dk-checkbox-param.png" width="706" alt="Checkbox parameter"/>

<tabs>

<tab title="TeamCity UI">

<img src="dk-checkbox-settings.png" width="460" alt="Checkbox parameter settings"/>

</tab>

<tab title="Kotlin DSL">

```Kotlin
object Test : BuildType({
   name = "Test"
   params {
      checkbox("CheckBoxDefaultParam",
              "debug", // Initial value
              label = "Release configuration",
              description = """Check to run the build in "Release" configuration. Otherwise, the "Debug" configuration is used.""",
              display = ParameterDisplay.PROMPT,
              checked = "release",
              unchecked = "debug")
   }
})
```

</tab>

<tab title="REST API">

**JSON payload:**

```JSON
{
  "name": "CheckBoxDefaultParam",
  "value": "debug",
  "type": {
    "rawValue": "checkbox description='Check to run the build in \"Release\" configuration. Otherwise, the \"Debug\" configuration is used.' label='Release configuration' uncheckedValue='debug' checkedValue='release' display='prompt'"
  }
}
```

**XML payload:**

```XML
<property name="CheckBoxDefaultParam" value="debug">
    <type rawValue="checkbox description='Check to run the build in &quot;Release&quot; configuration. Otherwise, the &quot;Debug&quot; configuration is used.' label='Release configuration' uncheckedValue='debug' checkedValue='release' display='prompt'"/>
</property>
```

</tab>

</tabs>


### RegEx Parameter

This parameter accepts only string values that match the given regular expression. TeamCity does not allow running a new build if an invalid value is entered.

<img src="dk-regexparam-overview.png" width="706" alt="RegEx Parameter"/>

<tabs>

<tab title="TeamCity UI">

<img src="dk-newparams-email.png" width="460" alt="RegEx parameter settings"/>

</tab>

<tab title="Kotlin DSL">

```Kotlin
object MyBuildConfig : BuildType({
   params {
      text("EmailRegExParam",
           "johndoe@jetbrains.com",
           regex = """^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}${'$'}""",
           validationMessage = "Invalid email address")
   }
})
```

</tab>

<tab title="REST API">

**JSON payload:**

```JSON
{
    "name": "EmailRegExParam",
    "value": "johndoe@jetbrains.com",
    "type": {
        "rawValue": "text regexp='^|[\\w-\\.|]+@(|[\\w-|]+\\.)+|[\\w-|]{2,4}$' validationMode='regex' validationMessage='Invalid email address' display='normal'"
    }
}
```

**XML payload:**

```XML
<property name="EmailRegExParam" value="johndoe@jetbrains.com">
    <type rawValue="text regexp='^|[\w-\.|]+@(|[\w-|]+\.)+|[\w-|]{2,4}$' validationMode='regex' validationMessage='Invalid email address' display='normal'"/>
</property>
```

</tab>

</tabs>


### Single-Select Parameter

This parameter defines multiple values but allows users to select only one value at a time. Values are displayed as combobox items in the **Run Custom Build** dialog.

<img src="dk-newparams-singleselect1.png" width="706" alt="Single selection parameter 1"/>

<img src="dk-newparams-singleselect2.png" width="706" alt="Single selection parameter 2"/>


<tabs>

<tab title="TeamCity UI">

<img src="dk-newparams-singleselect.png" width="460" alt="Single select parameter settings"/>

</tab>

<tab title="Kotlin DSL">

```Kotlin
object MyBuildConf : BuildType({
   params {
      select("SingleSelectParam",
              "linux",
              options = listOf(
                      "Windows" to "win",
                      "Linux" to "linux",
                      "macOS" to "mac"
              )
      )
   }
})
```

</tab>

<tab title="REST API">

**JSON payload:**

```JSON
{
  "name": "SingleSelectParam",
  "value": "linux",
  "type": {
    "rawValue": "select data_5='mac' label_5='macOS' label_3='Linux' display='normal' data_1='win' label_1='Windows' data_3='linux'"
  }
}
```

**XML payload:**

```XML
<property name="SingleSelectParam" value="linux">
    <type rawValue="select data_5='mac' label_5='macOS' label_3='Linux' display='normal' data_1='win' label_1='Windows' data_3='linux'"/>
</property>
```

</tab>

</tabs>



### Multi-Select Parameter

This parameter allows users to select multiple values from the predefined list.

<img src="dk-newparams-multiselect.png" alt="Multiselect parameter" width="706"/>

If multiple items are selected, the parameter joins their values using the specified separator char. For example, if the separator was changed from a default comma (`,`) to a vertical bar (`|`), the parameter value looks like the following: `2023.03|2023.11|2024.03`.


<tabs>

<tab title="TeamCity UI">

<img src="dk-multiselect-uisettings.png" width="460" alt="Multiselect parameter UI settings"/>

</tab>

<tab title="Kotlin DSL">

```Kotlin
object Test : BuildType({
   name = "Test"
   params {
      select("MultiSelectParam",
              "2023.03",
              allowMultiple = true,
              valueSeparator = "|",
              options = listOf(
                      "master" to "2023.03",
                      "2023.05",
                      "2023.11",
                      "2024.03",
                      "2024.06"))
   }
})
```

</tab>

<tab title="REST API">

**JSON payload:**

```JSON
{
  "name": "MultiSelectParam",
  "value": "2023.03",
  "type": {
    "rawValue": "select data_6='2024.06' data_5='2024.03' display='normal' multiple='true' valueSeparator='||' data_1='2023.03' label_1='master' data_4='2023.11' data_3='2023.05'"
  }
}
```

**XML payload:**

```XML
<property name="MultiSelectParam" value="2023.03">
    <type rawValue="select data_6='2024.06' data_5='2024.03' display='normal' multiple='true' valueSeparator='||' data_1='2023.03' label_1='master' data_4='2023.11' data_3='2023.05'"/>
</property>
```

</tab>

</tabs>



### Confirmation Dialog

If a configuration has a **prompt** type parameter, every time a user attempts to run a new build the [Run Custom Build](running-custom-build.md) dialog pops up. The build will start only after a user enters a valid value for this parameter. You can use this behavior to implement a custom confirmation dialog that protects a configuration from excessive runs.

<img src="dk-params-startPrompt.png" width="706" alt="Starting Prompt"/>


<tabs>

<tab title="TeamCity UI">

<img src="dk-promptdialog-uisettings.png" width="460" alt="Prompt dialog settings"/>

</tab>


<tab title="Kotlin DSL">

```Kotlin
object Test : BuildType({
   name = "Test"
   params {
      text("PromptConfirmation",
              "",
              label = "Deployment build confirmation",
              description = "This configuration triggers the deployment chain, which uploads updated NuGet packages and Docker images to public sources. Do you want to continue?",
              display = ParameterDisplay.PROMPT, 
              regex = "deploy",
              validationMessage = """Type "deploy" to run this build""")
   }
})
```

</tab>

<tab title="REST API">

**JSON payload:**

```JSON
{
  "name": "PromptConfirmation",
  "value": "",
  "type": {
    "rawValue": "text regexp='deploy' validationMessage='Type \"deploy\" to run this build' display='prompt' description='This configuration triggers the deployment chain, which uploads updated NuGet packages and Docker images to public sources. Do you want to continue?' label='Deployment build confirmation' validationMode='regex'"
  }
}
```

**XML payload:**

```XML
<property name="PromptConfirmation" value="">
    <type rawValue="text regexp='deploy' validationMessage='Type &quot;deploy&quot; to run this build' display='prompt' description='This configuration triggers the deployment chain, which uploads updated NuGet packages and Docker images to public sources. Do you want to continue?' label='Deployment build confirmation' validationMode='regex'"/>
</property>
```

</tab>

</tabs>












 <seealso>
        <category ref="admin-guide">
            <a href="configuring-build-parameters.md">Configuring Build Parameters</a>
            <a href="using-build-parameters.md">Using Build Parameters</a>
            <a href="predefined-build-parameters.md">List of Predefined Build Parameters</a>
        </category>
</seealso>