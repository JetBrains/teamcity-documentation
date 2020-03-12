[//]: # (title: Maven)
[//]: # (auxiliary-id: Maven)


Note that you can create a new Maven\-based build configuration [automatically from URL](creating-and-editing-projects.md#Creating+project+pointing+to+repository+URL), and set up a [dependency build trigger](configuring-maven-triggers.md#Maven+Artifact+Dependency+Trigger), if a specific Maven artifact has changed.

<note>

__Remote Run Limitations related to Maven runner__    
As a rule, a personal build in TeamCity doesn't affect any "regular" builds run on the TeamCity server, and its results are visible to its initiator only. However, in case of using Maven runner, this behavior may differ.   
TeamCity doesn't interfere anyhow with the Maven dependencies model. Hence, if your Maven configuration deploys artifacts to a remote repository, __they will be deployed there even if you run a personal build__. Thereby, a personal build may affect builds that depend on your configuration.   
For example, you have a configuration A that deploys artifacts to a remote repository, and these artifacts are used by configuration B. When a personal build for A has finished, your personal artifacts will appear in B. This can be especially injurious, if configuration A is to produce release\-version artifacts, because proper artifacts will be replaced with developer's ones, which will be hard to investigate because of Maven versioning model. Plus these artifacts will become available to all dependent builds, not only to those managed by TeamCity.   
To avoid this, we recommend not using remote run for build configurations which perform deployment of artifacts.
</note>

##  Maven runner settings

<table><tr>

<td>

Option


</td>

<td>

Description


</td></tr><tr>

<td>

Goals


</td>

<td>

In the __Goals__ field, specify the sequence of space\-separated Maven goals that you want TeamCity to execute.  Some Maven goals can use version control systems, and, thus, they may become incompatible with some [Configuring VCS Settings](configuring-vcs-settings.md). If you want TeamCity to execute such a goal:

* Select "_Automatically on agent_" in the __[VCS Checkout Mode](vcs-checkout-mode.md)__ drop\-down list on the __Version Control Settings__ page. This makes the version control system available to the goal execution software. To use the `release:prepare` goal with Perforce VCS, see the [section](#Using+Maven+Release+with+Perforce) below.


</td></tr><tr>

<td>

Path to POM file


</td>

<td>

Specify the path to the POM file relative to the [build working directory](build-working-directory.md).   
By default, the property contains a `pom.xml` file. If you leave this field blank, the same value is put in this field. The path may also point to a subdirectory, and as such `<subdirectory>/pom.xml` is used.


</td></tr><tr>

<td>

Additional Maven command line parameters


</td>

<td>

Specify the list of command line parameters.

<note>

The following parameters are ignored: `-q`, `-f`, `-s` (if __User settings path__ is provided)
</note>


</td></tr><tr>

<td>

Working directory


</td>

<td>

Specify the [Build Working Directory](build-working-directory.md) if it differs from the [build checkout directory](build-checkout-directory.md).


</td></tr></table>

<note>

When TeamCity discovers a Maven build step automatically, it sets the goal automatically to `clean test` and an additional Maven command line parameter `-Dmaven.test.failure.ignore` to `true` to ignore failed tests. This parameter is helpful when the `test` goal is used for a Maven project with multiple modules. With this property set to `true`, even if the tests fail in some module, all the following modules will be tested as well.   
If you change the goal from `test` to `deploy` (or any other sequential goal from the Maven build lifecycle: `package`, `verify`, or `install`), make sure to set `-Dmaven.test.failure.ignore` to `false` so the failed tests are not ignored.

</note>

### Maven Settings

Choose the Maven version you want to use. __Since TeamCity 2017.1__, you can [manage the installed versions](installing-agent-tools.md).

<table>


<tr>

<td>

</td>

<td>

</td>

</tr>

<tr>

<td>

&lt;Auto&gt;


</td>

<td>

The path to Maven installation is taken from the `M2_HOME` environment variable, otherwise the current default version is used.


</td></tr><tr>

<td>

&lt;Default&gt;


</td>

<td>

The bundled version 3.0.5 is used as default. __Since TeamCity 2017.1__, you can [change the defaults](installing-agent-tools.md).


</td></tr><tr>

<td>

&lt;Custom&gt;


</td>

<td>

Provide a path to a custom Maven version.


</td></tr></table>

### User Settings

Specify what kind of user settings to use here. This is equivalent to the Maven command line option `-s` or `--settings`. The available options are:

<table>

<tr>

<td>

</td>

<td>

</td>

</tr>

<tr>

<td>

&lt;Default&gt;


</td>

<td>

Settings are taken from the default Maven locations on the agent. For the server logic, see [Maven Server-Side Settings](maven-server-side-settings.md).


</td></tr><tr>

<td>

&lt;Custom&gt;


</td>

<td>

Enter the path to an alternative user settings file. The path should be valid on agent and also on the server, see [Maven Server-Side Settings](maven-server-side-settings.md).


</td></tr><tr>

<td>

Predefined settings


</td>

<td>

If there are settings files uploaded to the TeamCity server via the administration UI, you can select one of the available options here. To upload settings file to TeamCity, click _Manage settings files_.  Maven settings are defined on the project level. You can see the settings files defined in the current project or upload files on the __Project Settings__ page using __Maven Settings__. The files will be available in the project and its subprojects. The uploaded files are stored in the &lt;TeamCity Data Directory&gt;/config/projects/%projectID%/pluginData/mavenSettings directory. If necessary, they can be edited right there. The uploaded files are used both for the agent and server\-side Maven functionality.   
If Custom or Predefined settings are used, the path to the effective user settings file is available inside the maven process as the `teamcity.maven.userSettings.path` system property.


</td></tr></table>

<include src="ant.md" include-id="java-param"/>

### Local Artifact Repository Settings

Select one of the following options:

<table>

<tr>
<td>

Option

</td>
<td>

Description

</td>
</tr>

<tr>
<td>

Per agent (default)

</td>
<td>

Use a separate repository to store artifacts, produced by all builds run by an agent, under the agent system directory.

</td>
</tr>

<tr>
<td>

Per build configuration

</td>
<td>

Use a separate repository to store artifacts, produced by all builds of the current build configuration.

</td>
</tr>

<tr>
<td>

Maven default

</td>
<td>

Use the default Maven repository location. The repository is shared between all build configurations and all agents on the machine.

</td>
</tr>

</table>

### Incremental Building

Select the __Build only modules affected by changes__ check box to enable incremental building of Maven modules. The general idea is that if you have a number of modules interconnected by dependencies, a change most probably affects (directly or transitively) only some of them; so if we build only the affected modules and take the result of building the rest of the modules from the previous build, we will get the overall result equal to the result of building the whole project from scratch with less effort and time.

Since Maven itself has very limited support for incremental builds, TeamCity uses its own change impact analysis algorithm for determining the set of affected modules and uses a special preliminary phase for making dependencies of the affected modules.

First TeamCity performs own change impact analysis taking into account parent relationship and different dependency scopes and determines affected modules. Then the build is split into two sequential Maven executions.

The first Maven execution called preparation phase is intended for building the dependencies of the affected modules. The preparation phase is to assure there will be no compiler or other errors during the second execution caused by the absence or inconsistency of dependency classes.

The second Maven execution called main phase executes the main goal (for example, `test`), thus performing only those tests affected by the change.

Also check a related [blog post](http://blogs.jetbrains.com/teamcity/2012/03/14/incremental-building-with-maven-and-teamcity/) on the topic.

### Docker Settings

In this section, you can specify a Docker image which will be [used to run the build step](docker-wrapper.md).

### Code Coverage

Coverage support based on IDEA coverage engine is added to Maven runner. To learn about configuring code coverage options, refer to the [Configuring Java Code Coverage](configuring-java-code-coverage.md) page.

<note>

Only Surefire version 2.4 and higher is supported.
</note>

If you have several build agents installed on the same machine, by default they use the same local repository. However, there are two ways to allocate a custom local repository to each build agent:

* Specify the following property in the `teamcity-agent/conf/buildAgent.properties`:   

```Plain Text
system.maven.repo.local=%system.agent.work.dir%/<subdirectory_name>

```   
For instance, `%system.agent.work.dir%/m2-repository`.

* Run each build agent under different user account.

## Maven Release with Different VCSs

To run the `release:prepare` Maven task with different VCS's supported by TeamCity, make sure you're using at least 2.0 version of the [Maven Release Plugin](http://maven.apache.org/maven-release/maven-release-plugin/).

### Using Maven Release with Perforce

Check the following:

1. Use [ticket-based authentication](http://www.perforce.com/perforce/doc.current/manuals/p4sag/chapter.superuser.html#1080319) for Maven Release plugin.
2. Make sure that your `release:prepare` Maven task works when it is run from the command line without TeamCity.

In the [Perforce VCS Root](perforce.md) Settings of your build configuration in TeamCity:

1. Enable the [checkout on agent](perforce.md#Checkout+On+Agent+Settings).
2. Enable [Use ticket-based authentication](perforce.md#P4+Connection+Settings) in Perforce VCS root settings.
3. Make sure your build agent environment doesn't have any occasional P4 variables which can interfere with the execution of Maven Release Plugin.
4. Specify `release:prepare` in the __Goals__ field of the Maven build step and run the build.

### Using Maven Release with Git VCS 

1. Use Git SSH URL as [SCM URL](https://maven.apache.org/scm/git.html) in your `pom.xml`.
2. Make sure that your `release:prepare` maven task works when it is run from the command line without TeamCity.

__On the TeamCity agent__:

1\. Make sure that the agent has Git installed and added to the agent's `$PATH` on Unix\-like OS's and to the `%PATH%` environment variable on Windows.

2\. On the agent, set your account's identity by executing


```Shell
git config --system user.email "buildserver@example.com"
git config --system user.name "TeamCity Server"
```

3\. Make sure your Git VCS is added to the known hosts database on the agent.

__On the TeamCity server__:

1\. Upload [Git SSH key](ssh-keys-management.md) to your TeamCity server.

_In the settings for your build configuration in TeamCity_:

2\. On the __Version Control Settings__ page, enable the checkout on agent.

3\. In your Git VCS root, enable _Private Key_ authentication.

4\. Add the [SSH Agent](ssh-agent.md) build feature to your configuration.

5\. Specify `release:prepare` in the __Goals__ field of the Maven build step and run the build.


__  __

__See also:__

__Concepts__: [Build Runner](build-runner.md)    
__Administrator's Guide__: [Maven Artifact Dependency Trigger](configuring-maven-triggers.md) | [Creating Maven Build Configuration](creating-and-editing-build-configurations.md)

__ __