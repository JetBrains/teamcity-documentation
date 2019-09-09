[//]: # (title: Including Third-Party Reports in the Build Results)
[//]: # (auxiliary-id: Including Third-Party Reports in the Build Results)
If your reporting tool produces reports in HTML format, you can extend TeamCity with a custom tab to show the information provided by the third\-party reporting tool.  The report provided by your tool can be then displayed either on the build results page, or on the project home page.

The general flow is as follows:
* configure the build script to produce the HTML report (preferably in a zip archive);
* configure publishing the report as the [build artifact](build-artifact.md) to the server: at this point you can check that the archive is available in the build artifacts;
* configure the __Report Tab__ to make the report available as an extra tab on the build or project level  (see below)

Report tabs support project hierarchy. There are two types of tabs available:
* __Build\-level__: appears on the [Build Results](working-with-build-results.md) page for each build that produced an artifact with the specified name. These report tabs are defined in a project and are inherited in its subprojects. To override an inherited Report tab in a subproject, create a new report tab with the same name as the inherited one in the subproject.
* __Project\-level__: appears on the Project home page for a particular project only if a build within the project produces the specified reports artifact.

To configure a report tab, go to the __Project Settings | Report Tabs__ and select the type of report tab you want to add.

For a __project report tab__, specify the following:

<table><tr>

<td>

Option


</td>

<td>

Description


</td></tr><tr>

<td>

Tab Title


</td>

<td>

Specify a unique title of the report tab that will be displayed in the web UI.


</td></tr><tr>

<td>

Get artifacts from


</td>

<td>

Select the build configuration and specify the build whose artifacts will be shown on the tab. Select whether the report should be taken from the last successful, pinned, finished build or the build with the specified build number or the last build with the specified tag.


</td></tr><tr>

<td>

Start page


</td>

<td>

Specify the path to the artifacts to be displayed as the contents of the report page. The path must be relative to the root of the build artifact directory.    
To use a file from an archive, use the `path-to-archive!relative-path` syntax, for example: `javadoc.zip!/index.html`. See the list of [supported archives](patterns-for-accessing-build-artifacts.md#Obtaining+Artifacts+from+a+Build+Script).

You can use the file browser ![chechoutdirBrowser.png](chechoutdirBrowser.png) next to the field to select artifacts. [Parameter references](configuring-build-parameters.md) are supported here, for example, `%\parameter%.zip!index.htm`


</td></tr></table>

For a __build report tab__, specify the following:

<table><tr>

<td>

Option


</td>

<td>

Description


</td></tr><tr>

<td>

Tab Title


</td>

<td>

Specify a unique title of the report tab that will be displayed in the web UI.


</td></tr><tr>

<td>

Start page


</td>

<td>

Specify the path to the artifacts to be displayed as the contents of the report page. The path must be relative to the root of the build artifact directory.    
To use a file from an archive, use the `path-to-archive!relative-path` syntax, for example: `javadoc.zip!/index.html`. See the list of [supported archives](patterns-for-accessing-build-artifacts.md#Obtaining+Artifacts+from+a+Build+Script).

You can use the file browser ![chechoutdirBrowser.png](chechoutdirBrowser.png) next to the field to select artifacts. [Parameter references](configuring-build-parameters.md) are supported here, for example, `%\parameter%.zip!index.htm`


</td></tr></table>
