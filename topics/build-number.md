[//]: # (title: Build Number)
[//]: # (auxiliary-id: Build Number)

Each build in TeamCity is assigned a build number, which is a string identifier composed according to the pattern specified in the build configuration setting on the [Configuring General Settings](configuring-general-settings.md) page. 
This number is displayed in the UI and passed into the build as a [Predefined Build Parameter](predefined-build-parameters.md). 



A build number can be:

* [Used to download artifacts](patterns-for-accessing-build-artifacts.md#Obtaining+Artifacts) 	
* [Referenced as a property](predefined-build-parameters.md)
* [Shared for builds connected by a dependency](how-to.md#Share+the+Build+number+for+Builds+in+a+Chain+Build)	
* [Used in artifact dependencies](artifact-dependencies.md)	
* [Set with help of service messages](build-script-interaction-with-teamcity.md#Reporting+Build+Number)


 __  __

__See also:__

__Administrator's Guide__: [Configuring General Settings](configuring-general-settings.md)
