[//]: # (title: Using Build Parameters)
[//]: # (auxiliary-id: Using Build Parameters)

This topic illustrates simple use cases where you might opt for referencing parameters in TeamCity UI instead of specifying plain values. Refer to the [](configuring-build-parameters.md#Main+Use+Cases) section for the general overview of parameters' usage scenarios.


## Store a Docker Registry Name

If you have various configurations that utilize the same image registry, you can create a custom parameter for the **&lt;Root&gt;** project to store this registry's name. As a result, you can reference this parameter in any [Docker step](docker.md) that pulls or pushes your images.

> Parameters declared inside the **&lt;Root&gt;** project are available in all TeamCity build configurations.
>
{style="tip"}

<img src="dk-params-reuseParameter.png" width="706" alt="Use a custom parameter in TeamCity"/>





## Specify the JDK Version

The following [](gradle.md) step always uses JDK 19 instead of the default version referenced by the `JDK_HOME` environment variable. The runner retrieves a path for this required JDK from the corresponding `env.` parameter.

```Kotlin
steps {
    gradle {
        name = "Gradle step"
        tasks = "build-dist"
        jdkHome = "%\env.JDK_19_0_ARM64%"
    }
}
```





## Set Additional .NET Parameters

In the following sample, the value of the [](net.md) runner's **Command line parameters** field is specified using a reference to the `dotnet.output.type` parameter.

<img src="dk-params-additionalSettings.png" width="706" alt="Additional parameters"/>

```Kotlin
object MyBuildConfig : BuildType({
    params {
        param("dotnet.output.type", "WinExe")
    }

    steps {
        dotnetBuild {
            args = "-p:OutputType=%\dotnet.output.type%"
        }
    }
})
```

> You can achieve the same result even faster by creating the `system.dotnet.output.type` parameter and setting its value to `OutputType=WinExe`. TeamCity will write this value to the response (.rsp) file with .NET settings, so you do not need to set the **Command line parameters** field in TeamCity UI.
>
> This approach is based on the mechanism that passes all parameters with the `system.` prefix to a build engine. See this section for more information: [](configuring-build-parameters.md#Pass+Values+to+Builders%27+Configuration+Files).
>
{style="tip"}






## Specify Artifact Paths

When setting artifact paths on the **Build Configuration Settings | General Settings | Artifact paths** page, you can utilize custom configuration parameters to substitute plain values.

```Kotlin
object GoalInBuildScripts : BuildType({
    artifactRules = "testfile1.txt => %\default.artifact.path%"
    params {
        param("default.artifact.name", "/bin/artifacts/build_artifact.tar.gz")
    }
})
```







## Modify the Build Numbering Pattern


The **Build Configuration Settings | General Settings | Build Number Format** field allows you to customize the numbering pattern for builds of this configuration.



A build's default zero-based integer index is stored in the `build.counter` parameter. The sample below adds the name of a repository branch to the build number.


```Kotlin
object MyBuildConf : BuildType({
    buildNumberPattern = "%\build.counter%-%\teamcity.build.branch%"
})
```





## Label Builds

The [](vcs-labeling.md) build feature allows build configurations to tag repository sources.

<img src="dk-params-vcs-labeling.png" width="706" alt="VCS Labeling with Parameters"/>

The following setup illustrates how to use the values of the `release.status` parameter as tags. See also: [Parameters Display Mode](typed-parameters.md).


```Kotlin
object MyBuildConf : BuildType({
    params {
        param("release.status", "EAP")
    }
    features {
        vcsLabeling {
            vcsRootId = "${DslContext.settingsRoot.id}"
            labelingPattern = "%\release.status%"
        }
    }
})
```






## Specify Checkout Rules

[](vcs-checkout-rules.md) can be configured on the **Build Configuration Settings | Version Control Settings** page. If your organization has a certain convention for branch names, you can store these default names as parameters and specify checkout rules as shown below.

```Kotlin
object GoalInBuildScripts : BuildType({
    params {
        param("branch.ignored", "refs/heads/sandbox")
        param("branch.default", "refs/heads/main")
    }

    vcs {
        root(YourVcsRootName, "+:%\branch.default%", "-:%\branch.ignored%")
    }
})
```

 <seealso>
        <category ref="admin-guide">
            <a href="configuring-build-parameters.md">Configuring Build Parameters</a>
            <a href="predefined-build-parameters.md">List of Predefined Build Parameters</a>
        </category>
</seealso>