[//]: # (title: Build Tag)
[//]: # (auxiliary-id: Build Tag)
Build tags are labels that can help you to:
* organize the history of your builds
* quickly navigate to the builds marked with a specific tag
* search for a build with a particular tag
* create an artifact dependency on a build with a particular tag   
 
You can assign any number of tags for a single build, for example, "EAP" or "release" using the __Edit tags__ dialog by entering several tags separated by a space, comma, semi\-colon, and so on.

Clicking a tag filters out all builds in the history: only the builds marked with the tag are displayed. Additionally, you can search for builds with particular tags using the [search field](search.md).

 

__To tag a build:__

The TeamCity Web UI provides the following ways to tag a particular build:
* using the [Build Configuration Home Page](viewing-build-configuration-details.md): 
     * go to either the __Overview__ or __History__ tab
     * go to the build history table
     * use the __Tags__ column for the desired build and add/modify the build tag
* using the [Build Results Page](working-with-build-results.md) for the particular build: 
     * click the __Actions__ button
     * select __Tag__ and add/modify the build tag
* using the Queued build page: 
     * click the __Actions__ button
     * select __Tag__ and add/modify the build tag
 * using the [Run Custom Build](triggering-a-custom-build.md) dialog 
    * go to the __Comments and Tags__ tab and add/modify the build tag
 
* Using the [Pin/Unpin build](pinned-build.md) dialog, where you can tag the build in addition to pinning it. For builds with snapshot dependencies, there is an option to pin and tag the build as well as its snapshot dependencies
 


[//]: # (Internal note. Do not delete. "Build Tagd46e113.txt")    



