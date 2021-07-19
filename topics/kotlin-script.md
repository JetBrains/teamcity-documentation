[//]: # (title: Kotlin Script)
[//]: # (auxiliary-id: Kotlin Script)

The _Kotlin Script_ runner allows executing a [Kotlin](https://kotlinlang.org/) script on Windows, Linux, or macOS.

Refer to [Configuring Build Steps](configuring-build-steps.md) for a description of common build steps' settings.

## Prerequisites

A [Kotlin](https://kotlinlang.org/) compiler version 1.3.70 or later must be [installed as an agent tool](installing-agent-tools.md) to run this step. Kotlin 1.5.0 is already bundled with TeamCity.

## Kotlin Script Settings

<table>

<tr>

<td>

Setting

</td>

<td>

Description

</td>

</tr>

<tr>

<td>

Kotlin compiler

</td>

<td>

Select a compiler version: use the bundled or default version, or enter a custom path to the compiler, relative to the [build checkout directory](build-checkout-directory.md).

</td>

</tr>

<tr>

<td>

Script type

</td>

<td>

Choose one of the two options: enter a custom script body right inside the runner or specify a path to a Kotlin script file (`.kts`).

</td>

</tr>

<tr>

<td>

Kotlin script

</td>

<td id="custom-script" auxiliary-id="kotlin-custom-script">

Available for the __Custom Script__ type.

Enter a code of a Kotlin script.

To extend the script's functionality with external libraries, you can use annotation-based references to Maven dependencies. For example:

```Kotlin
#!/usr/bin/env kotlin

@file:Repository("https://jcenter.bintray.com")
@file:DependsOn("org.jetbrains.kotlinx:kotlinx-html-jvm:0.6.11")

import kotlinx.html.*; import kotlinx.html.stream.*; import kotlinx.html.attributes.*

val addressee = args.firstOrNull() ?: "World"

print(createHTML().html {
    body {
        h1 { +"Hello, $address!" }
    }
})

...

```

See [Kotlin Help](https://github.com/Kotlin/KEEP/blob/master/proposals/scripting-support.md#kotlin-main-kts) for details.

</td>

</tr>

<tr>

<td id="script-file" auxiliary-id="kotlin-script-file">

Kotlin script file

</td>

<td>

Available for the __Script File__ type.

Enter a path to the script file, relative to the [build checkout directory](build-checkout-directory.md).

To support [annotation-based references](#custom-script), the provided file must have the `.main.kts` extension.

</td>

</tr>

<tr>

<td>

Script parameters

</td>

<td>

Enter the parameters of the script, as in the command line. [Parameter references](configuring-build-parameters.md#parameter-reference) are supported.

For security reasons, we highly recommend that you avoid using parameter references directly inside scripts if these parameters represent secure values. You can pass such values via script parameters instead. For example, to pass a token value, [add a new build parameter](configuring-build-parameters.md) with the "Password" type, refer it in the runner’s _Script parameters_ field:

`%access_token%`

and call it as an argument within the script:

`val accessToken = args[0]`

This way, you can reuse the token’s value anywhere in the script.

This practice will ensure that these values are available on the agent only during the build. Otherwise, if the parameters are specified directly inside the script, their resolved values will be stored on the agent machine as long as the script itself is stored, which might compromise the security of your data.

</td>

</tr>

<tr>

<td>

JDK

</td>

<td id="kotlin-java9-note" auxiliary-id="kotlin-java9-note">

Select JDK to run the script:
* __Default__: the path to JDK Home is read either from the `JAVA_HOME` environment variable on the agent machine, or from the `env.JAVA_HOME` property specified in the [build agent configuration file](build-agent-configuration.md) (`buildAgent.properties`). If these values are not specified, TeamCity uses the Java Home of the build agent process itself.
* __Custom__: enter a path to a JDK installed on the agent.
* Select any installed __version by number__.

>If you use Java 9 or later and Kotlin 1.4.2 or later, you might get the following warning in the build log:  
> `An illegal reflective access operation has occurred`  
> This is caused by a known issue of the Kotlin compiler and will not affect your build anyhow. The details of the issue and its workaround are described [here](https://youtrack.jetbrains.com/issue/TW-70604#focus=Comments-27-4763145.0-0).
>
{type="warning"}

</td>

</tr>

<tr>

<td>

JVM command line parameters

</td>

<td>

Specify JVM command line parameters: for example, _maximum heap size_ or parameters enabling _remote debugging_. These values are passed by the JVM used to run your build.

Example:

```Shell
-Xmx512m -Xms256m

```

</td>

</tr>

</table>