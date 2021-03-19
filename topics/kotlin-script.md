[//]: # (title: Kotlin Script)
[//]: # (auxiliary-id: Kotlin Script)

The _Kotlin Script_ runner allows executing a [Kotlin](https://kotlinlang.org/) script on Windows, Linux, or macOS.

>This runner in a part of TeamCity 2021.1 Early Access Program.

Refer to [Configuring Build Steps](configuring-build-steps.md) for a description of common build steps' settings. Refer to [Docker Wrapper](docker-wrapper.md) to learn how you can run this step inside a Docker container.

## Prerequisites

A [Kotlin](https://kotlinlang.org/) compiler must be [installed as an agent tool](installing-agent-tools.md) to run this step. Kotlin 1.4.31 is already bundled with TeamCity, but you can install other versions if necessary.

## Kotlin settings

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

Select a compiler version: use the default, bundled, or enter a custom one.

</td>

</tr>

<tr>

<td>

Script type

</td>

<td>


</td>

</tr>

<tr>

<td>

Kotlin script

</td>

<td>



</td>

</tr>

<tr>

<td>

Script parameters

</td>

<td>

parameter references

</td>

</tr>

<tr>

<td>

JDK

</td>

<td>

</td>

</tr>

<tr>

<td>

JVM command line parameters

</td>

<td>

</td>

</tr>

</table>