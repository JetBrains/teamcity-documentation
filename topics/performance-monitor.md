[//]: # (title: Performance Monitor (Legacy))
[//]: # (help-id: Performance Monitor)


This feature ensures backward compatibility for build configurations created before the 2026.1 update. Starting with version 2026.1, it is no longer needed, as all TeamCity builds automatically track agent CPU, disk, and memory usage. Collected data is shown on the **Performance Monitor** tab of the [](build-results-page.md).

<img src="dk-perfmon.png" width="706" alt="Performance Monitor tab"/>

Automatic data collection is disabled if:

* This build configuration or any of its parent projects have the `teamcity.perfmon.feature.enabled=false` [parameter](configuring-build-parameters.md).
* This build configuration has the **Performance Monitor (Legacy)** build feature, but it's currently [disabled](adding-build-features.md#How+to+Add%2C+Remove%2C+and+Disable+Build+Features).

Learn more: [](build-results-page.md#Performance+Monitor+Tab).