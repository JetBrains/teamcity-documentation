[//]: # (title: Deployment Build Configuration)
[//]: # (auxiliary-id: Deployment Build Configuration)

TeamCity provides the __Deployment type__ of build configuration. Build configurations which perform deploying to some environment can be marked with this type: these are usually build configurations which have snapshot or artifact dependencies on the builds, whose results they deploy.  

You can [create a build configuration](creating-and-editing-build-configurations.md) and use the [General Settings](configuring-general-settings.md) tab to set its type to Deployment.

<tip>

You can [disable revisions synchronization](build-chain.md#Disabling+Revisions+Synchronization+Between+Chain+Parts) between some parts of a build chain to make your deployment setup more flexible.

</tip>

Once a configuration is marked as Deployment, TeamCity changes behavior in the following way:
* The __Run__ button caption for this configuration changes to __Deploy__.
* If there is a build used as a dependency in a Deployment configuration, then, after the build is run, its [build results page](working-with-build-results.md) will display the __Deployments__ section allowing you to quickly deploy the build.   

   <img src="Deployments.png" alt="Build deployments" width="1557"/>

* The __Change details__ page (the page where you can see which configurations are affected by the change, and which builds have been executed with this change) has the __Deployments__ tab that shows builds in Deployment configurations where this change was deployed for the first time.   
   
   <img src="ChangeDetails.png" alt="Deployments in Change details" width="1312"/>
   
* For Deployment configurations, TeamCity always shows the latest started build regardless of the changes it contains; unlike regular build configurations, for which the build with the latest changes is displayed.
* When setting the build configuration type to "Deployment", several settings are automatically changed to reflect the best practices: "Limit the number of simultaneously running builds" is set to "1" and "allow triggering personal builds" option is turned off.