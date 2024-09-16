[//]: # (title: Configuring Finish Build Trigger)
[//]: # (auxiliary-id: Configuring Finish Build Trigger)

The _finish build trigger_ starts a build of the current build configuration when a build of the selected build configuration is finished.

## Key Takeaways

* When the **Finish build trigger** is added to configuration A, it watches the target configuration B, and spawns a new A build whenever B finishes another build.
* You can choose whether a build should be triggered only if the target configuration's build is successful.
* Finish build triggers benefit from [snapshot dependencies](snapshot-dependencies.md) pointing to the same target configurations.
* Alternatively to tracking all new builds of the target configuration, you can trigger new builds only if those target builds have new changes. To do this, use the [Schedule Trigger](configuring-schedule-triggers.md#Build+Changes) instead.


## Trigger Settings

<img src="dk-finishbuildtrigger.png" width="706" alt="Finish build trigger settings"/>

* **Build configuration** — choose a build configuration to track. See the [](#Trigger+Limitations) section to learn why the snapshot dependency warning can be shown.
* **Trigger after successful build only** — allows you to avoid triggering new build if a build of a target configuration failed.
* **Branch filter** — a set of rules in the `+|-:<branch_name>` format that allows you to watch builds of specific branches only. See [](branch-filter.md) for more information.
* **Build customization** — provides options similar to the [Run Custom Build dialog](running-custom-build.md). See the [](#Triggered+Build+Customization) section for more information.


## Finish Build Triggers and Snapshots

To understand the difference between **finish build triggers** and [](snapshot-dependencies.md), let's have a look at the following example.

A sample project has three child build configurations:

<img src="dk-finish-build-diagram.png" width="706" alt="Chain diagram"/>

<table><tr><td>

<tabs>

<tab title="Update Release Date">

This configuration determines the current date and writes it to the project's `product.release.date` [parameter](configuring-build-parameters.md). Note that since the new parameter is written with the `setParameter` [Service Message](service-messages.md), the new value is in effect only in the scope of this build.

<br/>

```Kotlin
import jetbrains.buildServer.configs.kotlin.*
import jetbrains.buildServer.configs.kotlin.buildSteps.csharpScript

object UpdateReleaseDate : BuildType({
    name = "Update Release Date"

    steps {
        csharpScript {
            id = "csharpScript"
            content = """
                var date = DateTime.Now.ToString("MM.dd.yyyy");
                var serviceMessage = "##teamcity[setParameter name='product.release.date' value='" + date + "']";
                Console.WriteLine(serviceMessage);
            """.trimIndent()
            tool = "%\teamcity.tool.TeamCity.csi.DEFAULT%"
        }
    }
})
```

</tab>

<tab title="Update Build Version">

This configuration increments the project's `build.version` [parameter](configuring-build-parameters.md). Unlike "Update Release Date", this configuration sends a [](teamcity-rest-api.md) request to permanently change the parameter value.

<br/>

```Kotlin
import jetbrains.buildServer.configs.kotlin.*
import jetbrains.buildServer.configs.kotlin.buildSteps.script

object UpdateBuildVersion : BuildType({
    name = "Update Build Version"

    steps {
        script {
            id = "simpleRunner"
            scriptContent = """
                version=%\build.version%
                ((version=version+1))
                
                curl --location --request PUT 'http://<server_URL>/app/rest/projects/<project_name>/parameters/build.version' \
                --header 'Accept: */*' \
                --header 'Content-Type: text/plain' \
                --header 'Authorization: Bearer your_token' \
                --data ${'$'}version
            """.trimIndent()
        }
    }
})
```
</tab>

<tab title="Update Packages">

This configuration re-publishes artifacts. To do so, it first needs the actual release date and build number, which it retrieves from the other two configurations.

* The updated release date is accessible via the `dep.<configuration_name>.<parameter_name>` syntax because a [snapshot dependency](snapshot-dependencies.md) on the "Update Release Date" configuration exists.
* The updated build version is taken directly from the project's `build.version` parameter because the "Update Build Version" configuration utilizes REST API to write a permanent value.

<br/>

```Kotlin
import jetbrains.buildServer.configs.kotlin.*
import jetbrains.buildServer.configs.kotlin.buildSteps.script
import jetbrains.buildServer.configs.kotlin.triggers.finishBuildTrigger

object Delivery : BuildType({
    name = "Update Packages"

    steps {
        script {
            id = "simpleRunner"
            scriptContent = """
                echo "Delivery date is ${UpdateReleaseDate.depParamRefs["product.release.date"]}"
                echo "Build version is %\build.version%"
                
                # TODO: Re-publish packages with updated version and date values
            """.trimIndent()
        }
    }

    triggers {
        finishBuildTrigger {
            buildType = "${UpdateBuildVersion.id}"
        }
    }

    dependencies {
        snapshot(UpdateReleaseDate) {
            reuseBuilds = ReuseBuilds.NO
        }
    }
})
```

</tab>

</tabs>

</td></tr></table>

In this setup, running new builds will result the following:

* If a new "Update Release Date" build is triggered, other configurations will not be affected. A build will calculate a new parameter value that will not be used anywhere.

    <img src="dk-fbt-onlyB.png" width="706" alt="Trigger Update Release Date"/>

* If the "Update Build Version" configuration is triggered, once this build finishes a new "Update Packages" configuration build will be spawned because of the **Finish build trigger** added to this configuration. However, since "Update Packages" also has a snapshot dependency on "Update Release Date" (with disabled [build reuse](snapshot-dependencies.md#Suitable+Builds)), it will also request a current date. As a result, the entire "Update Build Version &rarr; Update Release Date &rarr; Update Packages" pipeline will run.

    <img src="dk-fbt-all3.png" width="706" alt="Trigger Update Build Version"/>

* If a new "Update Packages" build is triggered, it will first run its dependency "Update Release Date" build. The release version will be taken from the project parameter without running a new "Update Release Version" build.

    <img src="dk-fbt-B-and-C.png" width="706" alt="Trigger Update Packages"/>

As a result, delivery builds always ensure the date is correct, but reuse the existing product version unless the "Update Build Version" configuration produces a new build. Incrementing a build version automatically triggers the delivery process.

## Trigger Limitations

When used alone (without a [snapshot dependency](snapshot-dependencies.md) on the same configuration), the **Finish build trigger** has the following limitations:

* A newly triggered build may not have the same revisions as the finished tracked build even if both configurations have the same VCS settings.

* If a build configuration with the finish build trigger has an artifact dependency on the target build configuration, there is no guarantee that artifacts of a watched build will be used. This can happen because another build of a tracked configuration may finish while the triggered build sits in the build queue.

* The build triggered by the finish build trigger will always be triggered in the default branch even if the finished build uses another branch.

* The algorithm does not guarantee that every finished build in a monitored configuration will trigger a respective build in a configuration with the finish build trigger. In certain cases, for example, if two monitored builds finish somewhat simultaneously, only one respective build might be triggered. For more details on this limitation and its possible workarounds, see the [related task](https://youtrack.jetbrains.com/issue/TW-19836) in our issue tracker; if this limitation restrains your build pipeline anyhow, please leave a comment to this issue describing your use case.

All these limitations __do not apply__ if a build configuration with the finish build trigger has a snapshot dependency on the same build configuration. In this case, the trigger will run build on the same revisions and will attach the build to the chain. It will also use consistent artifacts if they are produced by dependencies. For that reason, the finish build trigger settings dialog shows a warning if a corresponding snapshot dependency does not exist, suggesting you use both features in tandem.

Note that if a build configuration with the finish build trigger __has__ a snapshot dependency on the selected build configuration, the trigger will be able to detect if the last monitored build has already been promoted to the current build configuration (either manually or by another trigger). In this case, the trigger will not run a new build.


## Triggered Build Customization

<include from="configuring-vcs-triggers.md" element-id="triggered-build-customization"/>

<seealso>
        <category ref="admin-guide">
            <a href="build-dependencies-setup.md">Build Dependencies Setup</a>
        </category>
        <category ref="concepts">
            <a href="build-chain.md">Build Chain</a>
        </category>
</seealso>