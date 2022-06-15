[//]: # (title: What's New in TeamCity 2022.06)
[//]: # (auxiliary-id: What's New in TeamCity 2022.06;What's New in TeamCity)

This is a quality-targeted release which focuses on improving performance. It includes the following new features:

## Support .NET 7 in C# script runner

[C# script](c-script.md) can now run on TeamCity agents with .Net 7. 


## Support for non-default streams/feature branches in Perforce Shelve Trigger

If stream support is enabled in a Perforce VCS Root, the [Perforce Shelve Trigger](perforce-shelve-trigger.md) will now automatically detect the target stream from the changed files and trigger the personal build in this stream.

* Autodetection of the branch works in the run custom build dialog even if the default branch is specified.
* The same applies to the [REST API endpoint]((https://www.jetbrains.com/help/teamcity/rest/edit-build-configuration-settings.html#Manage+Build+Triggers)). You do not have to specify the stream explicitly there, but can be specified via the ```desiredStream``` HTTP parameter.
* Autodetection also works in the REST API when the ```desiredBranch parameter``` is not set in HTTP request.

## Customization of Amazon EC2 Launch Templates

You can now override Amazon EC2 Launch Templates settings in TeamCity and run agents based on the template with your custom settings.


## Fixed issues
{product="tcc"}


## Upgrade notes


## Roadmap

See the [TeamCity roadmap](https://www.jetbrains.com/teamcity/roadmap/#teamcity-roadmap) to learn about future updates.