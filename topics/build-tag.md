[//]: # (title: Build Tag)
[//]: # (auxiliary-id: Build Tag)

Build tags are labels that can help you to:
* organize the history of your builds
* quickly navigate to the builds marked with a specific tag
* search for a build with a particular tag
* create an artifact dependency on a build with a particular tag   
 
You can assign any number of tags for a single build, for example, `EAP` or `release` using the _Edit tags_ dialog by entering several tags separated by any symbol like space, comma, or semicolon.

Clicking a tag filters out all builds in the history: only the builds marked with the tag are displayed. Additionally, you can search for builds with particular tags using the [search field](search.md).
{product="tc"}

Clicking a tag filters out all builds in the history: only the builds marked with the tag are displayed.
{product="tcc"}

The TeamCity web UI provides the following ways to tag a particular build:
* On the __[Build Configuration Home](viewing-build-configuration-details.md)__ page: 
     1. Go to either the __Overview__ or __History__ tab.
     2. In the build history table, open the context menu of the required build and select __Add tags__ (or __Edit tags__ to modify existing ones).
* On the __[Build Results](working-with-build-results.md)__ page of the particular build: 
     1. Click __Actions__.
     2. Select __Tag__ and add/modify the build tags.
* On the __Queued Build__ page: 
     1. Click __Actions__.
     2. Select __Tag__ and add/modify the build tags.
* In the _[Run Custom Build](running-custom-build.md)_ dialog:
    * Go to the __Comments and Tags__ tab and add/modify the build tags.
* In the _[Pin/Unpin Build](pinned-build.md)_ dialog where you can tag the build in addition to pinning it. For builds with snapshot dependencies, there is an option to pin and tag the build as well as its snapshot dependencies.

Alternatively, you can add and modify build tags using [TeamCity REST API](https://www.jetbrains.com/help/teamcity/rest/manage-builds.html#Build+Tags).

[//]: # (Internal note. Do not delete. "Build Tagd46e113.txt")    
