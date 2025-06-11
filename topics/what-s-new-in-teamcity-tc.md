[//]: # (title: What's New in TeamCity On-Premises 2025.07)

<snippet id="2025-07-tc">


## Feature 1
{instance="tc"}

TBD

## UI Improvements
{instance="tc"}

We remain committed to delivering a modern, intuitive TeamCity experience for all users, whether you prefer pipelines or classic build chains for your CI/CD workflows. This update brings several UI improvements, including:

* A redesigned navigation sidebar with a **+** button for quickly adding subprojects, configurations, and pipelines. You can also set the panel to auto-hide to maximize workspace.
* A new **What’s New** widget to keep you informed about major updates in each release.
* An ability to open [build logs](build-log.md) in full-screen mode.


## Miscellaneous Enhancements

* [SSH keys](ssh-keys-management.md) uploaded to or generated in TeamCity are now stored encrypted in the [](teamcity-data-directory.md) in encrypted form. TeamCity uses a [custom encryption key](teamcity-configuration-and-maintenance.md#encryption-settings) from the general server settings, or a built-in key if none is specified. Note that only newly uploaded or generated keys are encrypted, re-upload existing keys to apply encryption.
<!--* TeamCity can now import a custom encryption key from the `TEAMCITY_ENCRYPTION_KEYS` environment variable, disabling the option to enter it manually in TeamCity UI. Keys imported from an environment variable are not stored in the server configuration, making it more secure to keep the configuration directory in a remote VCS repository. See the following article for more information: [](teamcity-configuration-and-maintenance.md#encryption-settings).-->

</snippet>