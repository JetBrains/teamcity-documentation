[//]: # (title: Kotlin DSL)
[//]: # (auxiliary-id: Kotlin DSL)

Besides [storing settings in version control](storing-project-settings-in-version-control.md) in XML format, TeamCity allows storing the settings in the DSL (based on the [Kotlin language](https://kotlinlang.org/)).

Using the version control-stored DSL enables you to define settings programmatically. Since Kotlin is statically typed, you automatically receive the autocompletion feature in an IDE which makes the discovery of available API options much simpler.

Check out the [blog post series](https://blog.jetbrains.com/teamcity/2019/03/configuration-as-code-part-1-getting-started-with-kotlin-dsl) on using Kotlin DSL in TeamCity.

On this page:

<tag-list of="chapter" mode="tree" depth="4"/>

## Getting Started with Kotlin DSL

This [Kotlin tutorial](https://play.kotlinlang.org/koans/overview) helps quickly learn most Kotlin features.

To start working with Kotlin DSL, create an empty sandbox project on your server and follow these steps:
1. [Enable versioned settings](storing-project-settings-in-version-control.md#Synchronizing+Settings+with+VCS) for your project.
2. Select Kotlin as the format. Make sure the _Generate portable DSL scripts_ option is enabled.
3. Click __Apply__, and TeamCity will commit the settings to your repository.

<note>

You might not be able to edit a project in the web UI right after enabling Kotlin for the project settings. TeamCity needs to detect its own commit in the repository, and only after that editing will be enabled. Usually it takes a minute or two.
</note>

<anchor name="portableDSL"/>

### Project Settings Structure

After the commit to the repository, you will get the `.teamcity` settings directory with the following files:
* `settings.kts` – the main file containing all the project configuration
* `pom.xml` – only required when opening the project in an IDE to get the autocompletion feature, and ability to compile code and write unit tests for it

### Opening Project in IntelliJ IDEA

To open the Kotlin DSL project in IntelliJ IDEA, open the `.teamcity/pom.xml` file as a project. All necessary dependencies will be resolved automatically right away. If all dependencies have been resolved, no errors in red will be visible in `settings.kts`. If you already have an IntelliJ IDEA project and want to add a Kotlin DSL module to it, follow [related instructions](#How+to+Add+.teamcity+as+a+New+Module+to+a+Project%3F).

<note>

If you use macOS, note that the `.teamcity` directory will be hidden, and IntelliJ IDEA will not show it in the file chooser by default. To display the hidden files and directories, press `Command` + `Shift` + `.` inside the file chooser.

</note>

### Editing Kotlin DSL

If you created an empty project, that's what you'll see in your IDE when you open `settings.kts`:

```kotlin
import jetbrains.buildServer.configs.kotlin.v2018_1.*
/* some comment text  */
version = "2018.1"
 
project {
}

```

`project { }` represents the current project whose settings you'll define in the DSL. This is the same project where you enabled versioned settings on the previous step. This project ID and name can be accessed via a special `DslContext` object but cannot be changed via the DSL. 

You can create different entities in this project by calling `vcsRoot()`, `buildType()`, `template()`, or `subProject()` methods.

For instance, to add a build configuration with a command line script, do the following:

```kotlin
import jetbrains.buildServer.configs.kotlin.v2018_1.*
import jetbrains.buildServer.configs.kotlin.v2018_1.buildSteps.script 

version = "2018.1"

project {
  buildType {
    id("HelloWorld")
    name = "Hello world"
    steps {
        script {
            scriptContent = "echo 'Hello world!'"
        }
    }
  }
}
```

<anchor name="id-or-name"/>

Here, `id` will be used as the value of the _[Build configuration ID](identifier.md)_ field in TeamCity. If `id` is not specified, the value of the `name` parameter will be used as the build configuration ID instead.

After that, you can submit this change to the repository – TeamCity will detect and apply it. If there are no errors during the script execution, you should see a build configuration named "Hello world" in your project.

<note>

To get familiar with Kotlin API, see the online documentation on your local server, accessible via the link on the __Versioned Settings__ project tab in the UI or by running the `mvn -U dependency:sources` command in the IDE. See the documentation on the public TeamCity server as an [example](https://teamcity.jetbrains.com/app/dsl-documentation/jetbrains.build-server.configs.kotlin.v2018_1/index.html).

The documentation is generated in a separate Java process and might take several minutes to build after the server restart. 

You can also use the __Download settings in Kotlin format__ option from the project __Actions__ menu. For instance, you can find a project that defines some settings that you want to use in your Kotlin DSL project and use this "download" action to see what the DSL generated by TeamCity looks like.
</note>

### Patches

TeamCity allows editing a project via the web interface, even though the project settings are stored in Kotlin DSL. For every change made in the project via the web interface, TeamCity will generate a patch in the Kotlin format, which will be checked in under the project _patches_ directory with subdirectories for different TeamCity entities. For example, if you change a build configuration, TeamCity will submit the `.teamcity/patches/buildTypes/<id>.kt` script to the repository with necessary changes.

For instance, the following patch adds the [Build files cleaner (Swabra)](build-files-cleaner-swabra.md) build feature to the build configuration with the ID `SampleProject_Build`:

```kotlin
changeBuildType(RelativeId("SampleProject_Build")) { // this part finds the build configuration where the change has to be done  
    features {
        add {
            // this is the part which should be copied to a corresponding place of the settings.kts file
            swabra {
                filesCleanup = Swabra.FilesCleanup.DISABLED
            }
        }
    }
}
```

It is implied that you move the changes from the patch file to your `settings.kts` and delete the patch file. Patches generation allows smooth transition from editing settings via the web UI to Kotlin DSL.

### Sharing Kotlin DSL Scripts

Besides simplicity, one of the advantages of the portable DSL script is that the script can be used by more than one project or more than one server (hence the name: portable).

If you have a repository with `.teamcity` containing settings in portable format, you can easily create another project based on these settings. The [Create Project From URL](creating-and-editing-projects.md) feature can be used for this.

Point TeamCity to your repository, and it will detect the `.teamcity` directory and offer to import settings from there:

<img src="NewKotlinProjectURL.png" width="638" alt="Importing Kotlin settings when creating a project from URL"/>

<note>

The `settings.kts` script can always access a `DslContext` object which contains the ID and some other settings of the current project.
</note>

Based on the context, the DSL script can generate slightly different settings, for instance:

```kotlin
var deployTarget = if (DslContext.projectId == AbsoluteId("Trunk")) {
    "staging"
} else {
    "production"
}
```

## Advanced Topics

### Restoring Build History After ID Change

To identify a build configuration in a project based on the portable DSL, TeamCity uses the [ID](#id-or-name) assigned to this build configuration in DSL. We recommend keeping this ID constant, so the changes made in the DSL source are consistently applied to the respective build configuration in TeamCity.   
However, if you need to modify the build configuration ID in the DSL, note that for TeamCity it looks like the configuration with the previous ID was deleted and a new configuration with the new ID was created. In this case, you can still restore the history of builds of the deleted configuration.

If your DSL-based project uses [versioned settings](storing-project-settings-in-version-control.md#Synchronizing+Settings+with+VCS), TeamCity detects any change in the source DSL and reapplies all the changed settings to affected TeamCity entities. When you modify the ID of a build configuration in the [VCS source](#id-or-name), TeamCity deletes the "old" configuration from the web UI, but keeps the history of its builds in the database for 5 days and only then cleans it up. TeamCity provides a way to attach this history to any existing DSL-based build configuration.

To restore the build history after changing the build configuration ID, go to the __Build Configuration Settings__ of the newly created configuration in TeamCity, open the __Actions__ menu, and click __Attach build history__. You will be redirected to the temporary __Attach Build History__ tab. Select the recently deleted build configuration in the list and click __Attach__.

<img src="attach-build-history.png" width="1283" alt="Attaching a build history"/>

TeamCity will restore the history of builds of the selected configuration and attach it to the history of builds of the new configuration.

### Non-Portable DSL

Versions before 2018.1 used a different format for Kotlin DSL settings. This format can still be enabled by turning off the _Generate portable DSL scripts_ checkbox on the __Versioned Settings__ page.

When TeamCity generates a non-portable DSL, the project structure in the `.teamcity` directory looks as follows:
* `pom.xml`
* `<project id>/settings.kts`
* `<project id>/Project.kt`
* `<project id>/buildTypes/<build conf id>.kt`
* `<project id>/vcsRoots/<vcs root id>.kt`

where `<project id>` is the ID of the project where versioned settings are enabled. The Kotlin DSL files producing build configurations and VCS roots are placed under the corresponding subdirectories.

#### settings.kts

In the non-portable format each project has the following `settings.kts` file:

```kotlin
package MyProject
import jetbrains.buildServer.configs.kotlin.v2018_1.*
/* ... */
version = "2018.1"

project(MyProjectId.Project)

```

This is the entry point for project settings generation. Basically, it represents a Project instance which generates project settings.

#### Project.kt

The `Project.kt` file looks as follows: 

```kotlin
package MyPackage
import jetbrains.buildServer.configs.kotlin.v2018_1.*
import jetbrains.buildServer.configs.kotlin.v2018_1.Project

object Project : Project({
   uuid = "05acd964-b90f-4493-aa09-c2229f8c76c0"
   id("MyProjectId")
   parentId("MyParent")
   name = "My project"
   ...
})

```

where:
* `id` is the absolute ID of the project, the same ID you'll see in browser address bar if you navigate to this project
* `parentId` is the absolute ID of a parent project where this project is attached
* `uuid` is some unique sequence of characters.    
The` uuid` is a unique identifier which associates a project, build configuration or VCS root with its data. If the `uuid` is changed, then the data is lost. The only way to restore the data is to revert the `uuid` to the original value. On the other hand, the `id` of an entity can be changed freely, if the `uuid` remains the same. This is the main difference of the non-portable DSL format from portable. The portable format does not require specifying the `uuid`, but if it happened so that a build configuration lost its history (e.g. on changing build configuration external id) you can reattach the history to the build configuration using the __Attach build history__ option from the __Actions__ menu. See [details](#Restoring+Build+History+After+ID+Change).

<note>

If the build history is important, it should be restored as soon as possible: after the deletion, there is a [configurable](clean-up.md) timeout (5 days by default) before the builds of the deleted configuration are removed during the build history clean-up.
</note>

In case of a non-portable DSL, patches are stored under the project _patches_ directory of `.teamcity`:
* `pom.xml`
* `<project id>/settings.kts`
* `<project id>/Project.kt`
* `<project id>/patches/<uuid>.kt`

Working with patches is the same as in portable DSL: you need to move the actual settings from the patch to your script and remove the patch.

### Debugging Maven ‘generate’ Task

The `pom.xml` file provided for a Kotlin project has the `generate` task, which can be used to generate TeamCity XML files locally from the Kotlin DSL files. This task supports debugging. If you are using IntelliJ IDEA, you can easily start debugging of a Maven task:
1. Navigate to __View | Tool Windows | Maven Projects__. The __Maven Projects__ tool window is displayed.
2. Locate the task node: __Plugins | teamcity-configs | teamcity-configs:generate__, the __Debug__ option is available in the context menu for the task:

<img src="NewKotlinDebug.png" width="1320" alt="Debugging a task in IDEA"/>

### Ability to Use External Libraries

You can use external libraries in your Kotlin DSL code, which allows sharing code between different Kotlin DSL-based projects.

To use an external library in your Kotlin DSL code, add a dependency on this library to the `.teamcity/pom.xml` file in the settings repository and commit this change so that TeamCity detects it. Then, before starting the generation process, the TeamCity server will fetch the necessary dependencies from the Maven repository, compile code with them, and then start the settings generator.

## View DSL in UI

When viewing your build configuration settings in the UI, you can click __View DSL__ in the sidebar: the DSL representation of the current configuration will be displayed and the setting being viewed (for example, a build step, a trigger, dependencies) will be highlighted. To go back, click __Edit in UI__.

## FAQ and Common Problems

<anchor name="nonUniformIDs"/>

### Why portable DSL requires the same prefix for all IDs?

Templates, build configurations, and VCS roots have [unique IDs](identifier.md#Universally+Unique+IDs) throughout all the TeamCity projects on the server. These IDs usually look like: `<parent project id>_<entity id>`.

Since these IDs must be unique, there cannot be two different entities in the system with the same ID.

However, one of the reasons why portable DSL is called portable is because the same `settings.kts` script can be used to generate settings for two different projects even on the same server.   
To achieve this while overcoming IDs uniqueness, TeamCity operates with relative IDs in portable DSL scripts. These relative IDs do not have parent project ID prefix in them. So when TeamCity generates portable Kotlin DSL scripts, it has to remove the parent project ID prefix from the IDs of all of the generated entities.

But this will not work if not all of the project entities have this prefix in their IDs. In this case the following error can be shown:

_Build configuration id '&lt;some id&gt;' should have a prefix corresponding to its parent project id: '&lt;parent project id&gt;'_

To fix this problem, change the ID of the affected entity. If there are many such IDs, use the __Bulk edit IDs__ project action to change all of them at once.

### How to Add .teamcity as a New Module to a Project?

_Question_: How to add the `.teamcity` settings directory as a new module to an existing project in IntelliJ IDEA?

_Solution_: In your existing project in  IntelliJ IDEA:
* Go to __File | Project Structure__, or press __Ctrl+Shift+Alt+S__.
* Select __Modules__ under the __Project Settings__ section.
* Click the plus sign, select __Import module__ and choose the folder containing your project settings. Click __Ok__ and follow the wizard.
* Click __Apply__. The new module is added to your project structure.

### New URL of Settings VCS Root (Non portable format)

_Problem_: I changed the URL of the VCS root where settings are stored in Kotlin, and now TeamCity cannot find any settings in the repository at the new location.

_Solution_:  
* Fix the URL in the Kotlin DSL in the version control and push the fix.
* Disable versioned settings to enable the UI.
* Fix the URL in the VCS root in the UI.
* [Enable versioned settings](storing-project-settings-in-version-control.md#Synchronizing+Settings+with+VCS) with the same VCS root and the Kotlin format again. TeamCity will detect that the repository contains the `.teamcity` directory and ask you if you want to import settings.
* Choose to import settings.

### How to Read Files in Kotlin DSL

_Problem_: I want to generate a TeamCity build configuration based on the data in some file residing in the VCS inside the `.teamcity` directory.

_Solution_: TeamCity executes DSL with `.teamcity` as the current directory, so files can be read using the paths relative to the `.teamcity` directory, for example, `File("data/setup.xml")`. Files outside the `.teamcity` directory are not accessible to Kotlin DSL.

### Kotlin DSL API documentation is not initialized yet

_Problem_:
* `app/dsl-documentation/index.html` on our Teamcity server displays "Kotlin DSL API documentation is not initialized yet"
* `OutOfMemoryError` during TeamCity startup with `org.jetbrains.dokka` in stack trace
 
 _Solution_: set the internal property `teamcity.kotlinConfigsDsl.docsGenerationXmx=768m`.

### Passwords-Related Questions

__Prior to TeamCity 2017.1__

_Problem_: I do not want the passwords to be committed to the VCS, even in a scrambled form.

_Solution_: You can move the passwords to the parent project whose settings are not committed to a VCS.


_Problem_: I want to change passwords after the settings have been generated.

_Solution_: The passwords will have to be scrambled manually using the following command in the Maven project with settings:



```Shell
mvn -Dtext=<text to scramble> org.jetbrains.teamcity:teamcity-configs-maven-plugin:scramble
```
 

__Since TeamCity 2017.1__

_Solution_: Use tokens instead of passwords. Refer to the [related section](storing-project-settings-in-version-control.md#Generating+Tokens).



__  __
 
__See also:__

__Administrator's Guide__: [Storing Project Settings in Version Control](storing-project-settings-in-version-control.md)   
__TeamCity blog__: [Configuration as Code, Part 1: Getting Started with Kotlin DSL](https://blog.jetbrains.com/teamcity/2019/03/configuration-as-code-part-1-getting-started-with-kotlin-dsl/)

__ __
