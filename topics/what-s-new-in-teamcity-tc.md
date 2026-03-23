[//]: # (title: What's New in TeamCity On-Premises 2026.1)

<snippet id="2026-1-tc" instance="tc">

## Release Cycle Updates

Starting with this release, we return to the pre-2022 versioning scheme: major TeamCity versions will use the “YYYY.N” format, where “N” indicates the release number rather than the month. This change makes the release cycle more predictable and better aligned with other JetBrains products.

This year, we also expect to separate the release cadence for TeamCity On-Premises and Cloud. On-Premises will continue to receive two major releases per year. Cloud, on the other hand, will be updated more frequently, so new features and improvements become available sooner without waiting for a major On-Premises release.




## Miscellaneous Enhancements

* The [HashiCorp Vault Connection](hashicorp-vault.md#Set+Up+a+Vault+Connection) now supports authentication via [Google Cloud Platform authentication](https://developer.hashicorp.com/vault/docs/auth/gcp).

* All TeamCity build configurations now automatically record agent hardware usage during builds. This change introduces the following updates:

    * The **PerfMon** build feature is no longer required and has been renamed to **Performance Monitor (Legacy)**.
    * The corresponding tab on the **Build Results** page is now called [Performance Monitor](build-results-page.md#Performance+Monitor+Tab), reflecting that the data is no longer tied to the deprecated feature.
    * A new `teamcity.perfmon.feature.enabled` parameter allows you to disable CPU, disk, and memory usage collection for specific build configurations or projects.


* When building [Perforce shelved changelists](integrating-teamcity-with-perforce.md#Running+Builds+on+Perforce+Shelved+Files), earlier versions of TeamCity replaced checked-out files with corresponding shelved ones. Starting with version 2026.1, TeamCity uses a more sophisticated approach by running `p4 resolve` after unshelving, allowing it to detect and resolve conflicting changes.

</snippet>
