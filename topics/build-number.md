[//]: # (title: Build Number)
[//]: # (auxiliary-id: Build Number)

Each build in TeamCity is assigned a build number, which is a string identifier composed according to the pattern specified in the build configuration setting on the [General Settings](configuring-general-settings.md) page.  
This number is displayed in the UI and passed into the build as a [predefined property](predefined-build-parameters.md).

A build number can be:

* [used to download artifacts](patterns-for-accessing-build-artifacts.md#Obtaining+Artifacts) 	
* [referenced as a property](predefined-build-parameters.md)
* [shared for builds connected by a dependency](how-to.md#Share+the+Build+number+for+Builds+in+a+Chain+Build)	
* [used in artifact dependencies](artifact-dependencies.md)	
* [set with help of service messages](service-messages.md#Reporting+Build+Number)

 <seealso>
        <category ref="admin-guide">
            <a href="configuring-general-settings.md">Configuring General Settings</a>
        </category>
</seealso>