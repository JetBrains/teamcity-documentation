[//]: # (title: Maven-related Data)
[//]: # (auxiliary-id: Maven-related Data)

## Maven Project Data

In TeamCity you can find information about settings specified in your Maven project's `pom.xml` file on the dedicated __Maven__ tab of build configuration. In addition to getting a quick overview of the settings, you can find __Provided parameters__ in the upper section of this page, for example, `maven.project.name`, `maven.project.groupId`, `maven.project.version`, `maven.project.artifactId`. You can use these parameters within your build. You can reference them within the build number pattern using the %\-notation. For example, `%maven.project.version%.{0}`.



## Maven Build Information

For each Maven build TeamCity agent gathers Maven specific build details, that are displayed on the __Maven Build Info__ tab of the build results after the build is finished.   
This page can be useful for build engineers when adjusting build configurations. 
