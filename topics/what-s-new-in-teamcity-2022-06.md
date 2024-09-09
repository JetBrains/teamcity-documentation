[//]: # (title: What's New in TeamCity 2022.06)
[//]: # (auxiliary-id: What's New in TeamCity 2022.06)

This is a quality-targeted release which focuses on improving performance.

## Support for non-default streams/feature branches in Perforce Shelve Trigger

If stream support is enabled in a Perforce VCS Root, the [Perforce Shelve Trigger](perforce-shelve-trigger.md) will now automatically detect the target stream from the changed files and trigger the personal build in this stream.

* Autodetection of the branch works in the run custom build dialog even if the default branch is specified.
* The same applies to the [REST API endpoint]((https://www.jetbrains.com/help/teamcity/rest/edit-build-configuration-settings.html#Manage+Build+Triggers)). You do not have to specify the stream explicitly there, but can be specified via the ```desiredStream``` HTTP parameter.
* Autodetection also works in the REST API when the ```desiredBranch parameter``` is not set in HTTP request.

## Fixed issues
{instance="tcc"}

See [TeamCity Build 115122 release notes](teamcity-release-notes-build-115122.md).

## Roadmap

See the [TeamCity roadmap](https://www.jetbrains.com/teamcity/roadmap/#teamcity-roadmap) to learn about future updates.