[//]: # (title: Build Script Interaction with TeamCity)
[//]: # (auxiliary-id: Build Script Interaction with TeamCity)

If TeamCity doesn't support your testing framework or build runner out of the box, you can still avail yourself of many TeamCity benefits by customizing your build scripts to interact with the TeamCity server. This makes a wide range of features available to any team regardless of their testing frameworks and runners. Some of these features include displaying real-time test results and customized statistics, changing the build status, and publishing artifacts before the build finishes. The build script interaction can be implemented by means of:
* [service messages](service-messages.md) in the build script;
* the [`teamcity-info.xml`](teamcity-info-xml.md) file (obsolete approach, consider using service messages instead).