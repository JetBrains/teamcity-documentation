[//]: # (title: Revision)
[//]: # (auxiliary-id: Revision)

A _revision_ refers to a specific state of a version control history; basically it is a version of the source code. When changes occur, they are usually identified by a number or letter code, termed "revision".

When displaying [changes](change.md) included in a finished or queued build, TeamCity also displays the corresponding revision. This section explains how the VCS revisions are chosen by TeamCity for a build.

TeamCity will use the current repository revision when a new [VCS root](vcs-root.md) is configured for a [project](project.md) or a [build configuration](build-configuration.md), or when the configured [VCS root settings](configuring-vcs-settings.md) have been modified.

After that TeamCity does not monitor the whole repository but only collects changes for the scope of the repository specified in TeamCity: the configured [VCS root settings](configuring-vcs-settings.md) with [checkout rules](vcs-checkout-rules.md). The revision of the sources corresponding to the latest detected change affecting your build will be displayed as the VCS root revision on the [Changes page](viewing-your-changes.md) accessible via the link on the __Projects__ page or on the [Changes tab](working-with-build-results.md#Changes) of the build results.

If the settings of a VCS Root get modified since the last detected change, the revision in TeamCity will be different from the last change in the newly configured VCS root. TeamCity does not have any information on the previous change in this new root, so it starts to monitor changes with the new settings and sets the build revision to the first discovered change. Until the change is discovered, there is no way to get any revision other than the current revision of the repository. Therefore, while TeamCity builds the correct revision of the sources, the revision for the first build after a VCS root change will not be equal to the last change under the specified path.



 __  __

__See also:__



__Concepts__: [Change](change.md), [Build Configuration](build-configuration.md)  
__User's Guide__: [Investigating and Muting Build Problems](investigating-and-muting-build-problems.md)
