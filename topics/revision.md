[//]: # (title: Revision)
[//]: # (auxiliary-id: Revision)

A _revision_ refers to a specific state of a version control history; basically it is a version of the source code. When changes occur, they are usually identified by a number or letter code, termed "revision".

When displaying [changes](change.md) included in a finished or queued build, TeamCity also displays the corresponding revision. This page explains how TeamCity chooses VCS revisions for a build.

TeamCity remembers the current repository revision when you add a new [VCS root](vcs-root.md) for a [project](project.md) or [build configuration](managing-builds.md), or when you [modify settings](configuring-vcs-settings.md) of an existing VCS root.

After that, TeamCity does not monitor the whole repository but only collects changes for the scope of the repository specified in TeamCity: the configured [VCS root settings](configuring-vcs-settings.md) with [checkout rules](vcs-checkout-rules.md).   
The revision of the sources corresponding to the latest detected change affecting your build will be displayed as the VCS root revision on the [Changes page](viewing-user-changes-in-builds.md) accessible via the link on the __Projects__ page or on the [Changes tab](build-results-page.md#Changes+Tab) of __Build Results__.

## Revision order

On detecting a new commit in any VCS root, TeamCity assigns this commit to an incrementally increasing internal ID. All detected commits on the TeamCity server are sorted by these IDs.

When starting a build, TeamCity scans VCS roots of the build and, for each root, checks out the revision that corresponds to the commit with the latest ID, according to the internal sorting. This way, TeamCity orders source revisions based on the ID of their latest commits.   
This is used, for example, to set apart a _[history build](history-build.md)_ that is a build which highest-ordered revision has a lower order than any build that run earlier.

## Revision change on modifying VCS root

If the settings of a VCS root get modified since the last detected change, the revision in TeamCity will be different from the last change in the newly configured VCS root. TeamCity does not have any information on the previous change in this new root, so it starts to monitor changes with the new settings and sets the build revision to the first discovered change. Until the change is discovered, there is no way to get any revision other than the current revision of the repository. Therefore, while TeamCity builds the correct revision of the sources, the revision for the first build after a VCS root change will not be equal to the last change under the specified path.

 <seealso>
        <category ref="concepts">
            <a href="change.md">Change</a>
            <a href="managing-builds.md">Build Configuration</a>
        </category>
        <category ref="user-guide">
            <a href="investigating-and-muting-build-failures.md">Investigating and Muting Build Failures</a>
        </category>
</seealso>