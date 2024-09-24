[//]: # (title: Customizing Statistics Charts)
[//]: # (auxiliary-id: Customizing Statistics Charts)

To help you track the condition of your projects and individual build configurations over time, TeamCity gathers statistical data across all their history and displays it as visual charts. This page describes how to modify the [predefined](statistic-charts.md) project-level charts.

<note>

Refer to a separate page to add [custom charts](custom-chart.md) on the project or build configuration level.
</note>

## Modifying Predefined Project-level Charts

By default, the __Statistics__ tab on the project level shows charts for all build configurations in the current project, which have coverage, duplicates or inspections data. However, you can disable charts of a particular or specify build configurations to be used in the charts.

To modify predefined project level charts, you need to configure the [`<TeamCity Data Directory>`](teamcity-data-directory.md)`/config/projects/<project_name>/pluginData/plugin-settings.xml` file.   
In this file, a similar format is used for all types of pre-defined graphs:

<table><tr>

<td>

Chart Type


</td>

<td>

XML Tag Name


</td></tr><tr>

<td>

Code Coverage


</td>

<td>

coverage-graph


</td></tr><tr>

<td>

Code Duplicates (Java and .NET)


</td>

<td>

duplicates-graph


</td></tr><tr>

<td>

Code Inspections


</td>

<td>

inspections-graph


</td></tr></table>

### Disabling Charts of Particular Type on Project Level

To disable charts of particular type for a project, use the following syntax:


```
<coverage-graph enabled="false">

```

In this example, all code coverage charts will be removed from the Statistics page.

### Showing Charts Only for Specific Build Configurations on Project Level

To show the code coverage chart related only to a particular build configuration, use the following syntax:

```
<coverage-graph enabled="true">
    <build-type id="myConf1"/>
    <build-type id="myConf2"/>
</coverage-graph>

```

where __myConf1__ and __myConf2__ values are [build configuration IDs](configuring-general-settings.md#build-configuration-id). However, note that build configurations specified should contain code coverage data for the charts to be shown. If the data is available, two charts will be shown (one for each specified build configuration).

 <seealso>
        <category ref="user-guide">
            <a href="statistic-charts.md">Statistic Charts</a>
        </category>
        <category ref="admin-guide">
            <a href="build-script-interaction-with-teamcity.md">Build Script Interaction with TeamCity</a>
            <a href="configuring-test-reports-and-code-coverage.md">Configuring Test Reports and Code Coverage</a>
        </category>
</seealso>
