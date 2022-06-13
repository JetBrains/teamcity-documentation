[//]: # (title: What's New in TeamCity 2022.06)
[//]: # (auxiliary-id: What's New in TeamCity 2022.06;What's New in TeamCity)

This is a quality-targeted release which focuses on improving performance. It includes the following new features:

## Support .NET 7 in C# script runner

[C# script](c-script.md) can now run on TeamCity agents with .Net 7. 


## Support for non-default streams/feature branches in Perforce Shelve Trigger

If stream support is enabled in a Perforce VCS Root, the [Perforce Shelve Trigger](perforce-shelve-trigger.md) will now detect the target stream from the changed files and trigger the personal build in this stream or multiple streams, if several streams are affected.


## Fixed issues
{product="tcc"}


## Upgrade notes


## Roadmap

See the [TeamCity roadmap](https://www.jetbrains.com/teamcity/roadmap/#teamcity-roadmap) to learn about future updates.