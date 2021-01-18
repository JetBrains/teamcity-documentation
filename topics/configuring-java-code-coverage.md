[//]: # (title: Configuring Java Code Coverage)
[//]: # (auxiliary-id: Configuring Java Code Coverage)

TeamCity supports Java code coverage via:

* [IntelliJ IDEA coverage engine](intellij-idea.md)
* [EMMA open-source toolkit](emma.md)
* [JaCoCo](jacoco.md)

>We recommend configuring code coverage either in the project source __or__ in the TeamCity runner. If it is configured in both places, this might result in conflicts or stack overflow issues.

<seealso>
        <category ref="concepts">
            <a href="code-coverage.md">Code Coverage</a>
        </category>
</seealso>
