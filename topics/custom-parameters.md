[//]: # (title: Custom Parameters)

This topic explains how to create custom TeamCity parameters and configure their appearance and behavior.

## How to Create a New Parameter

### In TeamCity UI

1. Go to the **Project Settings** or **Build Configuration Settings** page and switch to the **Parameters** tab. See this article for the information on parameters priority and inheritance rules: [](parameters-lifecycle-and-priority.md).

2. Click the **Add new parameter** button.

3. Specify the parameter kind and enter the parameter name. See this article for more information on the difference between different parameter types: [](configuring-build-parameters.md). 

    <img src="dk-params-createNewParam.png" width="706" alt="Create New Parameter"/>

4. Optional: If your custom parameter should have a default value, enter it in the corresponding field. You can also leave this field empty if the final parameter value should be set in child projects or configurations, calculated during a build, or if you need different agents to report different values for this parameter. See the following article to learn more about available value sources: [](parameters-lifecycle-and-priority.md).

5. Optional: Set up the required parameter specification. Using parameter specs you can force users to enter parameter value each time they start a build, hide parameter values from TeamCity UI and REST API requests, and more. Refer to corresponding sections of this document to learn more.


### In Kotlin DSL



### Via REST API


## Naming Conventions


## Parameter Specifications

### Display Modes

#### Normal

#### Hidden

#### Prompt

This display mode shows the "Run custom build" dialog every time users trigger a new build, which forces them to specify a parameter value.

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

#### Checkbox

#### Password

#### Select

#### Text