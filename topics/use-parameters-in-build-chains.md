[//]: # (title: Use Parameters in Build Chains)

This topic illustrates how you can use TeamCity [build parameters](configuring-build-parameters.md) to exchange simple data between configurations of a [build chain](build-chain.md).

## Access Parameters of Preceding Chain Builds

Dependent builds can access predefined and custom parameters of the previous chain builds as `dep.<bcID>.<parameter_name>`, where `bcID` is the ID of a source build configuration whose parameter value you need to access.

<img src="dk-params-in-chains.png" width="706" alt="Parameters in dependent builds"/>

You can use `dep...` parameters to access parameters from a configuration even if the current configration has only indirect dependencies. For example, in the A &rarr; B &rarr; C chain where C depends on B and B depends on A, configuration C can access A's parameters.

The following build configuration builds and pushes a Docker image. The name of this image is written to the `DockerImageName` parameter.

```Shell
TAG=v1
docker build -f Dockerfile --tag your.registry/MyApp:${TAG}
docker push your.registry/MyApp:v1
echo "##teamcity[setParameter name='DockerImageName' value='MyApp:${TAG}']"
```

If this configuration's ID is "ConfigA", builds executed further down the build chain can access the image name as `dep.ConfigA.DockerImageName`:

```Shell
docker run -d your.registry/%\dep.ConfigA.DockerImageName%
```

> For security reasons, parameters of the _Password_ type do not expose their values when referenced as `%\dep.<build_config_ID>.password_parameter%` in dependent builds.
> 
{style="note"}

## Override Parameters of Preceding Configurations

Add a parameter with the `reverse.dep.<build_conf_ID>.<parameter_name>` name syntax to override the `<parameter_name>` parameter defined in the target configuration that precedes the current configuration.

For example, the following [Kotlin](kotlin-dsl.md) code defines a project with three build configurations united in a single build chain (ConfigA &rarr; ConfigB &rarr; ConfigC). Each build configuration has a `chain.ConfigX.param` parameter with its custom value. The last configuration has the additional `reverse.dep.ChainConfigA.chain.ConfigA.param` parameter.

```Kotlin
import jetbrains.buildServer.configs.kotlin.*

project {
    buildType(ChainConfigA)
    buildType(ChainConfigB)
    buildType(ChainConfigC)
}

object ChainConfigA : BuildType({
    name = "ChainConfigA"

    params {
        param("chain.ConfigA.param", "Config A")
    }

    steps {
        script {
            scriptContent = """echo "Parameter value is: %\chain.ConfigA.param%""""
        }
    }
})

object ChainConfigB : BuildType({
    name = "ChainConfigB"

    params {
        param("chain.ConfigB.param", "Config B")
    }

    steps {
        script {
            scriptContent = """echo "Parameter value is: %\chain.ConfigB.param%""""
        }
    }

    dependencies {
        snapshot(ChainConfigA) {
            reuseBuilds = ReuseBuilds.NO
        }
    }
})

object ChainConfigC : BuildType({
    name = "ChainConfigC"

    params {
        param("chain.ConfigC.param", "Config C")
        param("reverse.dep.ChainConfigA.chain.ConfigA.param", "Value Overridden in ConfigC")
    }

    steps {
        script {
            scriptContent = """echo "Parameter value is: %\chain.ConfigC.param%""""
        }
    }

    dependencies {
        snapshot(ChainConfigB) {
            reuseBuilds = ReuseBuilds.NO
        }
    }
})
```

If you run the ConfigA or the ConfigA &rarr; ConfigB sub-chain, the first configuration will report its original parameter value.

```Plain Text
# ConfigA build log
Parameter value is: Config A
```

However, if you run a full build chain that ends with ConfigC, this last configuration will feed ConfigA a custom parameter value.

```Plain Text
# ConfigA build log
Parameter value is: Value Overridden in ConfigC
```

You can use `*` wildcards in parameter names to the same parameters in multiple preceding configurations. For example, the ConfigC in the following sample has the `reverse.dep.ChainConfig*.MyParam` parameter, which overrides `MyParam` in both ConfigA and ConfigB.

```Kotlin
object ChainConfigA : BuildType({
    params {
        param("MyParam", "OriginalValue_A")
    }
})

object ChainConfigB : BuildType({
    params {
        param("MyParam", "OriginalValue_B")
    }

    dependencies {
        snapshot(ChainConfigA) {
            reuseBuilds = ReuseBuilds.NO
        }
    }
})

object ChainConfigC : BuildType({
    params {
        param("reverse.dep.ChainConfig*.MyParam", "CustomValue_C")
    }

    dependencies {
        snapshot(ChainConfigB) {
            reuseBuilds = ReuseBuilds.NO
        }
    }
})
```

### Conflicting Parameter Overrides

If in the ConfigA &rarr; ... &rarr; ConfigB &rarr; ... &rarr; ConfigC chain both ConfigB and ConfigC configurations try to override ConfigA's parameter ConfigC has a higher priority since it depends on ConfigB (either directly or through intermediate configurations).

```Kotlin
object ChainConfigA : BuildType({
    params {
        param("MyParam", "OriginalValue_A")
    }
})

object ChainConfigB : BuildType({
    params {
        // Lower priority
        param("reverse.dep.ChainConfigA.MyParam", "CustomValue_B")
    }

    // Depends on config A
    dependencies {
        snapshot(ChainConfigA) {
            reuseBuilds = ReuseBuilds.NO
        }
    }
})

object ChainConfigC : BuildType({
    params {
        // Higher priority
        param("reverse.dep.ChainConfigA.MyParam", "CustomValue_C")
    }

    // Depends on config B
    dependencies {
        snapshot(ChainConfigB) {
            reuseBuilds = ReuseBuilds.NO
        }
    }
})
```

However, if ConfigB and ConfigC do not depend on each other, an ambiguity regarding which configuration should have a priority emerges. TeamCity tries to resolve this ambiguity by comparing parameter names and prioritizing a parameter with the most specific build configuration ID.

* Highest priority: parameters with no wildcards in build configuration IDs (for example, `reverse.dep.ChainConfigA.MyParam`).
* Medium priority: parameters with partial configuration IDs (for example, `reverse.dep.Chain*A.MyParam`). The more specific the target configuration ID is, the higher the priority of this parameter. For instance, the `ChainConf*A` ID has a priority over the `Chain*A` ID since it is considered more specific.
* Lowest priority: parameters with the `*` wildcard instead of configuration IDs (for example, `reverse.dep.*.MyParam`).

If all conflicting configurations have similar parameter names and neither of them is a clear winner, TeamCity reports a conflict and creates additional `conflict.<build_config_ID>.<parameter_name>=<value>` parameters (one for each conflicting configuration).

```Kotlin
object ChainConfigA : BuildType({
    params {
        param("MyParam", "OriginalValue_A")
    }
})

object ChainConfigB : BuildType({
    params {
        // Equal priority
        param("reverse.dep.ChainConfigA.MyParam", "CustomValue_B")
    }

    // Depends on config A
    dependencies {
        snapshot(ChainConfigA) {
            reuseBuilds = ReuseBuilds.NO
        }
    }
})

object ChainConfigC : BuildType({
    params {
        // Equal priority
        param("reverse.dep.ChainConfigA.MyParam", "CustomValue_C")
    }

    // Depends on config A
    dependencies {
        snapshot(ChainConfigA) {
            reuseBuilds = ReuseBuilds.NO
        }
    }
})

// Composite build configuration that runs the entire chain
object ChainABC : BuildType({
    type = BuildTypeSettings.Type.COMPOSITE
    dependencies {
        snapshot(ChainConfigB) {}
        snapshot(ChainConfigC) {}
    }
})
```

<img src="dk-params-overrideConflict.png" width="706" alt="Conflicting Overrides"/>

### Other Considerations


* The `reverse.dep.*` parameters are processed on queuing a build where these parameters are defined. Since parameter values should be already known at this stage, these values must be assigned either in the build configuration or in the [custom build dialog](running-custom-build.md). Setting the parameter to a value calculated during a build has no effect.

* Pushing a new parameter into a build overrides the "_[Do not run new build if there is a suitable one](snapshot-dependencies.md#Suitable+Builds)_" snapshot dependency option and may trigger a new build if the parameter is set to a non-default value.

* Values of the `reverse.dep.` parameters are pushed to the dependency builds "as is", without [reference resolution](configuring-build-parameters.md#Parameter+References). `%`-references, if any, will be resolved in the destination (target) build's scope.
