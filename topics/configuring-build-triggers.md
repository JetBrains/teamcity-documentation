[//]: # (title: Configuring Build Triggers)
[//]: # (auxiliary-id: Configuring Build Triggers)

Once a build configuration is created, builds can be triggered manually by clicking the [Run](triggering-a-custom-build.md#Run+Custom+Build+dialog) button or initiated automatically with the help of _triggers_.

A _build trigger_ is a rule which initiates a new build on certain events. The build is put into the [build queue](build-queue.md) and is started when there are agents available to run it.

While creating/editing a build configuration, you can configure triggers using the __Triggers__ sections of the __Build Configuration Settings__ page: click __Add new trigger__ and specify the trigger settings. For configuration details on each trigger, refer to the corresponding sections. It is possible to disable a configured build trigger temporarily or permanently using the option in the last column of the trigger list.

For each build configuration the following triggers can be configured:
* [VCS trigger](configuring-vcs-triggers.md): the build is triggered when changes are detected in the version control system roots attached to the build configuration.
* [Schedule trigger](configuring-schedule-triggers.md): the build is triggered at a specified time.
* [Finish build trigger](configuring-finish-build-trigger.md): the build is triggered after a build of the selected configuration is finished.
* [Maven artifact dependency trigger](configuring-maven-triggers.md#Maven+Artifact+Dependency+Trigger): the build is triggered if there is a content modification of the specified Maven artifact which is detected by the checksum change.
* [Maven snapshot dependency trigger](configuring-maven-triggers.md#Maven+Snapshot+Dependency+Trigger): the build is triggered if there is a modification of the snapshot dependency content in the remote repository which is detected by the checksum change.
* __Retry build trigger__: the build is triggered if the last build failed or failed to start.
* [Branch remote run trigger](branch-remote-run-trigger.md): personal build is triggered automatically each time TeamCity detects new changes in particular branches of the VCS roots of the build configuration. Supports Git and Mercurial.
* [NuGet dependency trigger](nuget-dependency-trigger.md): starts a build if there is a NuGet package update detected in the NuGet repository.
<tip>

Note that if you create a build configuration from a template, it inherits build triggers defined in the template, and they cannot be edited or deleted. However, you can specify additional triggers or disable a trigger permanently or temporarily.
</tip>

In addition to the triggers defined for the build configuration, you can also trigger a build by an [HTTP GET request](accessing-server-by-http.md#Triggering+a+Build+From+Script), or manually by running a custom build.

__ __