[//]: # (title: IntelliJ IDEA)
[//]: # (auxiliary-id: IntelliJ IDEA)

The IntelliJ IDEA coverage engine in TeamCity is the same engine that is used within IntelliJ IDEA to measure code coverage. This coverage attaches to the JVM as a Java agent and instruments classes on the fly when they are loaded by the JVM. In particular, it means that classes are not changed on the disk and can be safely used for distribution packages.

The IntelliJ IDEA coverage engine currently supports Class, Method, and Line coverage. There is no Branch/Block coverage yet.

>Make sure your tests run in the `fork=true` mode. Otherwise, the coverage data may not be properly collected.

<note>

Note that IDEA coverage is not currently supported for Android projects built via [Gradle](gradle.md). See the related feature request in [our tracker](https://youtrack.jetbrains.com/issue/TW-42167).
</note>

To configure code coverage using IntelliJ IDEA engine, follow these steps:

1. While creating/editing Build Configuration, go to the __Build Step__ page.
2. Select the [Ant](ant.md), [IntelliJ IDEA Project](intellij-idea-project.md), [Gradle](gradle.md) or [Maven](maven.md) build runner.
3. In the __Code Coverage__ section, select __IntelliJ IDEA__ as a coverage tool in the __Choose coverage runner__ drop-down menu.
4. Set up the coverage options - refer to the description of the available options below.

<table><tr>

<td>

Option

</td>

<td>

Description

</td></tr><tr>

<td>

Classes to instrument

</td>

<td>

Specify Java packages for which code coverage will be gathered. Use new-line delimited patterns that start with a valid package name and contain `*`. For example, `org.apache.*`.  

</td></tr><tr>

<td>

Classes to exclude from instrumentation

</td>

<td>

Use newline-separated patterns for fully qualified class names to be excluded from the coverage, for example: `*Test`. Exclude patterns have priority over include patterns.

</td></tr></table>

<seealso>
        <category ref="concepts">
            <a href="build-runner.md">Build Runner</a>
        </category>
        <category ref="admin-guide">
            <a href="configuring-test-reports-and-code-coverage.md">Configuring Test Reports and Code Coverage</a>
            <a href="configuring-java-code-coverage.md">Configuring Java Code Coverage</a>
            <a href="emma.md">EMMA</a>
        </category>
</seealso>