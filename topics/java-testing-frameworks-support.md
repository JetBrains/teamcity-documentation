[//]: # (title: Java Testing Frameworks Support)
[//]: # (auxiliary-id: Java Testing Frameworks Support)

TeamCity supports JUnit and TestNG by means of following build runners:

* [Gradle](gradle.md)
* [Ant](ant.md) (when tests are run by JUnit and testng tasks directly within the script)
* [Maven](maven.md) (when tests are run by Surefire/Failsafe Maven plugins, on-the-fly reporting is not available)
* [IntelliJ IDEA Project](intellij-idea-project.md): IntelliJ IDEA's JUnit and TestNG run configurations are supported. Note, that such run configurations should be shared and checked in to the version control.

__ __