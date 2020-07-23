[//]: # (title: Custom Chart)
[//]: # (auxiliary-id: Custom Chart)

In addition to statistic charts generated automatically by TeamCity on the Statistics tab, it is possible to configure your own statistical charts based on the set of [statistic values provided by TeamCity](#Default+Statistics+Values+Provided+by+TeamCity) or values reported from a build script. In the latter case you will need to configure your build script to report custom statistical data to TeamCity.

You can view statistic values reported by the build on the Build parameters page.

<tip>

The information in this section refers to __TeamСity 10\+__. For other versions, refer to the [listing](https://confluence.jetbrains.com/display/TW/Documentation) to choose the corresponding documentation.
</tip>


## Managing Custom Charts via the TeamCity Web UI

It is possible to manage custom charts using the TeamCity web UI.

### Adding Custom Charts

* The __Statistics__ tab for a project or build configuration provides an option to create a new chart. Note that only one build configuration can be currently added as the data source. More configurations can be added manually.
* On the __Parameters__ tab of the [build results](working-with-build-results.md) page, the list of __Reported statistic values__ provides checkboxes to select the statistics type for a new [project- or build-configuration-level](statistic-charts.md) chart. 
    * A project\-level chart will be added to the selected target project. The [root project](project.md) cannot be selected as the target.
    * A build\-configuration\-level chart will be added to all build configurations of the selected target project and its subprojects. Specifying the [root project](project.md#Root+Project) as the target will add the chart to all build configurations available on the server.

### Modifying Custom Charts

Use the pencil ![pencil.JPG](pencil.JPG) icon to edit or delete a custom chart. Note that the __Add Statistic Values__ drop\-down displays all statistic values registered on the server without filtering them by build. If you select a value non\-existent in the current build configuration or project when editing a chart, the chart will not be saved.

Using the cog ![cog.JPG](cog.JPG) icon, you can also configure the Y\-axis settings and save them as defaults for all users.

Note that there is a number of [limitations](edit-custom-chart-limitations.md) to editing charts from the TeamCity UI.

### Reordering Custom Charts

__To reorder custom charts__ for a project/build configuration, click the __Reorder__ button and drag\-and\-drop the charts to arrange them as required and apply your changes.

## Managing Custom Charts Manually

<note>

__Since TeamCity 10__, manual editing of custom charts has changed. For earlier versions, see this page in the corresponding [documentation](https://confluence.jetbrains.com/display/TW/Documentation).
</note>

To manually create custom charts to be displayed in the TeamCity web UI, configure the `<`[`TeamCity Data Directory`](teamcity-data-directory.md)`>/config/projects/<`[`ProjectID`](identifier.md)`>/project-config.xml` file. The file has the `<project-extensions>` element which contains all project features, including custom charts. For each chart an `<extention>`  element is added.

### Displaying Custom Chart in TeamCity Web UI

To make TeamCity display a custom chart in the web UI, update the `<`[`TeamCity Data Directory`](teamcity-data-directory.md)`>/config/projects/<`[`ProjectID`](identifier.md)`>/project-config.xml` configuration file adding a new `<extention>` sub\-element to the `<project-extensions>` element.

Each extension must have a unique `id` in the project.

The `type` attribute is set to 
* `<project-graphs>` for Project\-level chart
* `<buildType-graphs>` for Build Configuration\-level chart.

Each chart is described by the `<param>` element. It must contain the `<param>` sub\-elements with data shown in the chart in `name/value` pairs; the "__`series"`__ parameter uses the JSON format to list series of data shown on the chart. 

See the example below:

__Custom build configuration\-level chart in project\-config.xml__


```XML
<project-extensions>
 <extension id="customChart1" type="buildtype-graphs">
   <parameters>
     <param name="title" value="Custom chart"/>
     <param name="hideFilters" value="showFailed"/>
     <param name="seriesTitle" value="Some key"/>
     <param name="format" value="duration"/>
     <param name="series"><![CDATA[[
{
 "type": "valueType",
 "key": "BuildDuration",
 "title": "duration1",
 "sourceBuildTypeId": "my_first_configuration_id"
}, {
 "type": "valueType",
 "key": "customKey",
 "title": "Custom data",
 "color": "#ee0055 "
}, {
 "type": "valueTypes",
 "pattern": "buildStageDuration:*",
 "title": "Stage: {1}"
}
]]]>
     </param>
     <param name="properties.width" value="300"/>
     <param name="properties.height" value="300"/>
     <param name="properties.axis.y.type" value="logarithmic"/>
     <param name="properties.axis.y.includeZero" value="false"/>
     <param name="properties.axis.y.max" value="10000"/>
   </parameters>
 </extension>
 <extension id="secondChart" type="buildtype-graphs">
   <parameters>
     <param name="title" value="empty"/>
   </parameters>
 </extension>
</project-extensions>
```



This chart will be shown on the __Statistics__ tabs of the Build Configurations of the project where the `project-config.xml` file is located and all its subprojects. To display a chart for all Build Configurations, add it to the `project\-config.xml` of the [Root Project](project.md#Root+Project).

#### Parameters Reference

____

<table><tr>

<td>

Name

</td>

<td>

Description

</td></tr><tr>

<td>

`title`

</td>

<td>

The title above the chart.

</td></tr><tr>

<td>

`seriesTitle`

</td>

<td>

The title above the list of series used on the chart (in the singular form). The default is "Serie".

</td></tr><tr>

<td>

`defaultFilters`

</td>

<td>

The list of comma\-separated options to be checked by default. Can include the following:

* `showFailed` – include results from failed builds by default.
* `averaged` – by default, show averaged values on the chart.

</td></tr><tr>

<td>

`hideFilters`

</td>

<td>

The list of comma\-separated filter names that will not be shown next to the chart:

* `all` – hide all filters.
* `series` – hide series filter (you won't be able to show only data from specific valueType specified for the chart.)
* `range` – hide the date range filter.
* `showFailed` – hide the checkbox which allows including data for failed builds.
* `averaged` – hide the checkbox which allows viewing averaged values.
`Defaults`– empty (all filters are shown).

</td></tr><tr>

<td>

`format`

</td>

<td>

The format of the y\-axis values. Supported formats are:

* `duration`, data should be in milliseconds;
* `percent`, data should be in percents (from 0 to 100);
* `percentby1`, the format will show data between 0 and 1 as percents (from 0 to 100);
* `size`, data should be in bytes.  If no format is specified, the numeric format is used.

</td></tr></table>

____



The `<series>` parameter uses JSON format to list series of data shown on the chart. Each series is drawn in a separate color and you can choose one or another series using a filter.


____
<table><tr>

<td>

Name

</td>

<td>

Description

</td></tr><tr>

<td>

`type`

</td>

<td>

* `valueType` describes a series of data shown on the chart. Each series is drawn with a separate color and you may choose one or another series using a filter.
* `valueTypes` allows displaying several series on the chart by a `pattern` (described [below](#patternedValues))

</td></tr><tr>

<td>

`key`

</td>

<td>

The name of the valueType (series). It can be predefined by TeamCity, like `BuildDuration` or `ArtifactsSize` (see [below](#Default+Statistics+Values+Provided+by+TeamCity) for the complete list of predefined statistic values), or you can provide your own data by reporting it from the build script.

</td></tr><tr>

<td>

`title`

</td>

<td>

The series name shown in the series selector. Defaults to `<key>`. For several series, pattern group markers can be used: `{1}` stands for the first captured group in the pattern, `{0}` stands for the whole pattern.

</td></tr><tr>

<td>

`sourceBuildTypeId`

</td>

<td>

This field allows you to explicitly specify a build configuration to use the data from for the given series. This field is mandatory for the first valueType used in a chart if the chart is added at the project level. In other cases it is optional. However, note that TeamCity chooses the build configuration to take the data from according to the following rules:

1. if the `sourceBuildTypeId` is set within the `valueType`, the data is taken from this build configuration even if it belongs to a different project.
2. if the `sourceBuildTypeId` is not set within current the `valueType`, but it is set in the `valueType` above the current one within the chart, the data from the build configuration referenced above will be taken. See example for the `plugin-settings.xml` file above.
3. if the `sourceBuildTypeId` is not set within current the `valueType` and is not set above, the chart will show data for the current build configuration, i.e. this chart will work only for build configurations.

</td></tr><tr>

<td>

`color`

</td>

<td>

The color of a series to be used in the chart. Standard web color formats can be used \- "#RRGGBB", color names, etc. For more information see [HTML Colors reference](http://www.w3schools.com/html/html_colors.asp) and [HTML Color Names reference](http://www.w3schools.com/html/html_colornames.asp). If not specified, an automatic color will be assigned based on the series title.

</td></tr><tr>

<td>

<anchor name="patternedValues"/>

`pattern`

</td>

<td>

Pattern for names of the Value Types (or series) to be shown on the chart. The asterisk (\*) sign is allowed to filter Value Types (or series) either predefined by TeamCity, like `BuildDuration` or `ArtifactsSize` (see [below](#Default+Statistics+Values+Provided+by+TeamCity) for the complete list of predefined statistic values), or your own data can be provided by reporting it from the build script.

</td></tr></table>
____

 

#### Chart Dimensions

You can set the custom chart width/height in pixels using the `properties.width` and` properties.height` attributes for the `param` elements: `<param name="properties.width" value="300"/>.`

#### Chart Axis Settings

You can also customize the default axis settings for a chart via parameter names starting with `properties`, e.g. "`properties.axis.y.type`"

Supported properties:

____

<table><tr>

<td>

Name

</td>

<td>

Description

</td></tr><tr>

<td>

`properties.axis.y.type`

</td>

<td>

* `linear` (default) for the standard scale.
* `logarithmic` for the logarithmic Y axis scale

</td></tr><tr>

<td>

`properties.axis.y.includeZero`

</td>

<td>

Whether the zero value is included on the Y axis:

*  `true` (default)
* `false` (zero is not included)

</td></tr><tr>

<td>

`properties.axis.y.min`

</td>

<td>

An integer value to start the Y axis from.

</td></tr><tr>

<td>

`properties.axis.y.max`

</td>

<td>

An integer value to use as the maximum for the Y axis value .

</td></tr></table>

____

<anchor name="listOfDefaultStatisticValues"/>

<anchor name="Build Metrics Provided by TeamCity"/>
 
<anchor name="predefinedStatisticsKeys"/>

 

#### Default Statistics Values Provided by TeamCity

The table below lists the predefined value providers that can be used to configure a custom chart. The values reported for each build differ depending on your build configuration settings.

<anchor name=" reportedStatValues"/>

You can view the all statistic values reported by the build on the __Build Results | Parameters | Reported statistic values__ tab. For each of the values, a statistics chart is available on clicking the _View Trend_ icon ![ViewTrend.PNG](ViewTrend.PNG).

____

<table><tr>

<td>

Key

</td>

<td>

Description

</td>

<td>

Unit

</td></tr><tr>

<td>

`ArtifactsSize`

</td>

<td>

The sum of all [artifact](build-artifact.md) file sizes in the artifact directory

</td>

<td>

Bytes

</td></tr><tr>

<td>

`VisibleArtifactsSize`

</td>

<td>

The sum of all [artifact](build-artifact.md) file sizes excluding hidden artifacts (those placed under .teamcity directory)

</td>

<td>

Bytes

</td></tr><tr>

<td>

`buildStageDuration:artifactsPublishing`

</td>

<td>

The duration of the artifact publishing step in the build

</td>

<td>

Milliseconds

</td></tr><tr>

<td>

`buildStageDuration:buildStepRunner_<N>`

</td>

<td>

The duration of each step. The build step names are generated automatically to avoid issues when a build step name is too long or a build step is renamed. The ID of each step can be found in the xml file of the build configuration.

</td>

<td>

Milliseconds

</td></tr><tr>

<td>

`buildStageDuration:sourcesUpdate`

</td>

<td>

The duration of the source checkout step

</td>

<td>

Milliseconds

</td></tr><tr>

<td>

`buildStageDuration:dependenciesResolving`

</td>

<td>

The duration of resolving dependencies of the build

</td>

<td>

Milliseconds

</td></tr><tr>

<td>

`BuildDuration`

</td>

<td>

The build duration (all build stages)

</td>

<td>

Milliseconds

</td></tr><tr>

<td>

`BuildDurationNetTime`

</td>

<td>

The build steps duration (excluding the checkout and artifact publishing time, etc.)

</td>

<td>

Milliseconds

</td></tr><tr>

<td>

`CodeCoverageB`

</td>

<td>

Block\-level code coverage

</td>

<td>

%

</td></tr><tr>

<td>

`CodeCoverageC`

</td>

<td>

Class\-level code coverage

</td>

<td>

%

</td></tr><tr>

<td>

`CodeCoverageL`

</td>

<td>

Line\-level code coverage

</td>

<td>

%

</td></tr><tr>

<td>

`CodeCoverageM`

</td>

<td>

Method\-level code coverage

</td>

<td>

%

</td></tr><tr>

<td>

`CodeCoverageR`

</td>

<td>

Branch Coverage

</td>

<td>

%

</td></tr><tr>

<td>

`CodeCoverageS`

</td>

<td>

Statement Coverage

</td>

<td>

%

</td></tr><tr>

<td>

`CodeCoverageAbsBCovered`

</td>

<td>

The number of covered blocks

</td>

<td>

int

</td></tr><tr>

<td>

`CodeCoverageAbsBTotal`

</td>

<td>

The total number of blocks

</td>

<td>

int

</td></tr><tr>

<td>

`CodeCoverageAbsCCovered`

</td>

<td>

The number of covered classes

</td>

<td>

int

</td></tr><tr>

<td>

`CodeCoverageAbsCTotal`

</td>

<td>

The total number of classes

</td>

<td>

int

</td></tr><tr>

<td>

`CodeCoverageAbsLCovered`

</td>

<td>

The number of covered lines

</td>

<td>

int

</td></tr><tr>

<td>

`CodeCoverageAbsLTotal`

</td>

<td>

The total number of lines

</td>

<td>

int

</td></tr><tr>

<td>

`CodeCoverageAbsMCovered`

</td>

<td>

The number of covered methods

</td>

<td>

int

</td></tr><tr>

<td>

`CodeCoverageAbsMTotal`

</td>

<td>

The total number of methods

</td>

<td>

int

</td></tr><tr>

<td>

`CodeCoverageAbsRCovered`

</td>

<td>

The number of covered branches

</td>

<td>

int

</td></tr><tr>

<td>

`CodeCoverageAbsRTotal`

</td>

<td>

The total number of branches

</td>

<td>

int

</td></tr><tr>

<td>

`CodeCoverageAbsSCovered`

</td>

<td>

The number of covered statements

</td>

<td>

int

</td></tr><tr>

<td>

`CodeCoverageAbsSTotal`

</td>

<td>

The total number of statements

</td>

<td>

int

</td></tr><tr>

<td>

`DuplicatorStats`

</td>

<td>

The number of code duplicates found

</td>

<td>

int

</td></tr><tr>

<td>

`TotalTestCount`

</td>

<td>

The total number of tests in the build

</td>

<td>

int

</td></tr><tr>

<td>

`PassedTestCount`

</td>

<td>

The number of successfully passed tests in the build

</td>

<td>

int

</td></tr><tr>

<td>

`FailedTestCount`

</td>

<td>

The number of failed tests in the build

</td>

<td>

int

</td></tr><tr>

<td>

`IgnoredTestCount`

</td>

<td>

The number of ignored tests in the build

</td>

<td>

int

</td></tr><tr>

<td>

`InspectionStatsE`

</td>

<td>

The number of inspection errors in the build

</td>

<td>

int

</td></tr><tr>

<td>

`InspectionStatsW`

</td>

<td>

The number of inspection warnings in the build

</td>

<td>

int

</td></tr><tr>

<td>

`SuccessRate`

</td>

<td>

An indicator whether the build was successful

</td>

<td>

0 \- failed, 1 \- successful

</td></tr><tr>

<td>

`TimeSpentInQueue`

</td>

<td>

How long the build was queued

</td>

<td>

Milliseconds

</td></tr></table>




[//]: # (Internal note. Do not delete. "Custom Chartd106e1036.txt")    




#### Custom Build Metrics

If the predefined build metrics do not cover your needs, you can report custom metrics to TeamCity from your build script and use them to create a custom chart. There are two ways to report custom metrics to TeamCity:
* using [service messages](build-script-interaction-with-teamcity.md#Reporting+Build+Statistics) from your build,
* or (obsolete approach) using the [Build Script Interaction with TeamCity](build-script-interaction-with-teamcity.md#Providing+data+using+the+teamcity-info.xml+file) file.
Note that custom value keys should be unique and should not interfere with value keys predefined by TeamCity.

<seealso>
        <category ref="concepts">
            <a href="code-coverage.md">Code Coverage</a>
            <a href="code-inspection.md">Code Inspection</a>
            <a href="code-duplicates.md">Code Duplicates</a>
        </category>
        <category ref="user-guide">
            <a href="statistic-charts.md">Statistic Charts</a>
        </category>
        <category ref="admin-guide">
            <a href="build-script-interaction-with-teamcity.md">Build Script Interaction with TeamCity</a>
            <a href="https://plugins.jetbrains.com/docs/teamcity/custom-statistics.html">Custom Statistics</a>
        </category>
</seealso>