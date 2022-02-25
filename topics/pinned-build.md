[//]: # (title: Pinned Build)
[//]: # (auxiliary-id: Pinned Build)

A build can be _pinned_ to prevent it from being removed when a clean-up procedure is executed, as stipulated by the [clean-up policy](teamcity-data-clean-up.md).

You can pin/unpin builds as follows:
* On the [Build Results page](working-with-build-results.md), select __Pin__ in the _Actions_ drop-down menu.
* On the __Overview__ tab of the __Build Configuration Home__ page: 
  * To pin an individual build, select the __Pin__ action from the context menu of the required build.
  * To pin multiple builds at once, select the required builds (checkboxes appear when hovering builds) and click __Pin__ in the popup menu that appears on selecting builds. If you need to select a range of builds, you can press `Shift` and click build checkboxes at the edges of the range to be selected.

The _Pin/Unpin_ build dialog also allows you to tag the build.

If the current build is a part of a [chain](build-chain.md) and has [snapshot dependencies](snapshot-dependencies.md) on another builds, the _"Apply to all snapshot dependencies"_ box appears. Check this box to apply the actions (pinning/unpinning, adding / removing tags) to all dependency (preceding) builds of the current build.

<seealso>
        <category ref="concepts">
            <a href="teamcity-data-clean-up.md">Clean-up policy</a>
        </category>
</seealso>