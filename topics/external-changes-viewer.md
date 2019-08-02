[//]: # (title: External Changes Viewer)
[//]: # (auxiliary-id: External Changes Viewer)
TeamCity supports integration with external changes viewers like JetBrains Upsource,  Atlassian Fisheye, and so on. 

__Since TeamCity 2017.2__, some of the viewers are supported out of the box: commits to GitHub, VSTS or Bitbucket are automatically recognized and links are provided enabling you to view the changes externally.

To enable other external change viewers, create and configure the \<[TeamCity Data Directory](teamcity-data-directory.md)\>/change-viewers.properties file. These settings should be specified for _each VCS root_ you want to use the external changes viewer for. A detailed example of the configuration file including the description of available formats, variables, and other parameters can be found in the `change-viewers.properties.dist` file in the \<[TeamCity Data Directory](teamcity-data-directory.md)\>/config directory.

When the configuration file is created, links to the external viewer (![extChangesViewerIcon.png](extChangesViewerIcon.png)) will appear on the following pages:
* Changes popups on the __Projects__ and project home page, __Overview__ tab and the __Change Log__ tab of the build configuration home page)   

   <img src="externalChangesViewer.png" width="600" alt="Viewing external changes"/>
   
* __[Changes](working-with-build-results.md#Changes)__ tab of the build results page
* __Change Details__ page available by clicking the link when hovering over the changes on the __Overview__ and __Change Log__ tabs for a project and build configurations and on the __Changes__ tab of the build results page
* [TeamCity file diff page](difference-viewer.md)