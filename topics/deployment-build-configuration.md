[//]: # (title: Deployment Build Configuration)
[//]: # (auxiliary-id: Deployment Build Configuration)

TeamCity provides the _Deployment_ type of build configuration. Build configurations which perform deploying to some environment can be marked with this type: these are usually build configurations that have snapshot or artifact dependencies on the builds whose results they deploy.

After [creating a build configuration](creating-and-editing-build-configurations.md), open its [General Settings](configuring-general-settings.md) and set the _Build configuration type_ to _Deployment_. The type change does not affect the build configuration functionality but offers some handy features to distinguish the deployment build from other builds.   
You can change the type back to _Regular_ anytime.

<tip>

You can [disable revisions synchronization](build-chain.md#Disabling+Revisions+Synchronization+Between+Chain+Parts) between some parts of a build chain to make your deployment setup more flexible.

</tip>

Once a configuration is marked as _Deployment_, TeamCity changes behavior in the following way:
* The __Run__ button caption for this configuration changes to __Deploy__.
* If there is a build used as a [dependency](configuring-dependencies.md) in a deployment configuration, then, after the build is run, its [__Build Results__](working-with-build-results.md) page will display the __Deployments__ section allowing you to quickly deploy the build.   

   <img src="Deployments.png" alt="Build deployments" width="750"/>

* The __Change details__ page (the page where you can see which configurations are affected by the change and which builds have been executed with this change) has the __Deployments__ tab that shows builds in Deployment configurations where this change was deployed for the first time.   
   
   <img src="ChangeDetails.png" alt="Deployments in Change details" width="750"/>
   
* For deployment configurations, TeamCity always shows the latest started build regardless of the changes it contains; unlike regular build configurations, for which the build with the latest changes is displayed.
* When setting the build configuration type to _Deployment_, several settings are automatically changed to reflect the best practices: _"Limit the number of simultaneously running builds"_ is set to "1" and the _"Allow triggering personal builds"_ option is turned off.

<seealso>
        <category ref="concepts">
            <a href="build-chain.md">Build Chain</a>
            <a href="dependent-build.md">Dependent Build</a>
        </category>
        <category ref="admin-guide">
            <a href="snapshot-dependencies.md">Snapshot Dependencies</a>
            <a href="build-dependencies-setup.md">Build Dependencies Setup</a>
        </category>
</seealso>