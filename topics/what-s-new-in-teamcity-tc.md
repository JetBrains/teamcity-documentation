[//]: # (title: What's New in TeamCity On-Premises 2026.1)

<snippet id="2026-1-tc" instance="tc">

TBD

## Miscellaneous Enhancements

* The [HashiCorp Vault Connection](hashicorp-vault.md#Set+Up+a+Vault+Connection) now supports authentication via [Google Cloud Platform authentication](https://developer.hashicorp.com/vault/docs/auth/gcp).

* All TeamCity build configurations now automatically record agent hardware usage during builds. This change introduces the following updates:

    * The **PerfMon** build feature is no longer required and has been renamed to **Performance Monitor (Legacy)**.
    * The corresponding tab on the **Build Results** page is now called [Performance Monitor](build-results-page.md#Performance+Monitor+Tab), reflecting that the data is no longer tied to the deprecated feature.
    * A new `teamcity.perfmon.feature.enabled` parameter allows you to disable CPU, disk, and memory usage collection for specific build configurations or projects.


</snippet>
