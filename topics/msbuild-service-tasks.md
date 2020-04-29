[//]: # (title: MSBuild Service Tasks)
[//]: # (auxiliary-id: MSBuild Service Tasks)
For MSBuild, TeamCity provides the following service tasks that implement the same options as the [Build Script Interaction with TeamCity](build-script-interaction-with-teamcity.md):

## TeamCitySetBuildNumber

`TeamCitySetBuildNumber` allows user to change BuildNumber:

```XML
<TeamCitySetBuildNumber BuildNumber="1.3_{build.number}" />

```

It is possible to use `{build.number}` as a placeholder for older build number.

## TeamCityProgressMessage

`TeamCityProgressMessage` allows you to write progress message.

```XML
<TeamCityProgressMessage Text="Progress message text" />

```

## TeamCityPublishArtifacts

`TeamCityPublishArtifacts` allows you to publish all artifacts taken from MSBuild item group

```XML
<ItemGroup>
    <Files Include="*.dll" />
  </ItemGroup>
  <TeamCityPublishArtifacts SourceFiles="@(Files-> '%(FullPath)' )" Condition=" '$(TEAMCITY_VERSION)' != '' "/>

```


## TeamCityReportStatsValue

`TeamCityReportStatsValue` is a handy task to publish statistic values


```XML
<TeamCityReportStatsValue Key="StatsValueType" Value="42" />

```


## TeamCityBuildProblem

`TeamCityBuildProblem` task reports a build problem which actually fails the build. Build problems appear on the build results page and also affect build status text.


```XML
<TeamCityBuildProblem description="description" identity="identity"/>

```

	
* Mandatory `description` attribute is a human\-readable text describing the build problem. By default `description` appears in build status text.


[//]: # (Internal note. Do not delete. "MSBuild Service Tasksd214e94.txt")    

* `identity` is an optional attribute and characterizes particular build problem instance. Shouldn't change throughout builds if the same problem occurs, e.g. the same compilation error. Should be a valid Java id up to 60 characters. By default `identity` is calculated based on `description`.

## TeamCitySetStatus

`TeamCitySetStatus` is a task to change current build status text.   
Prior to TeamCity 8.0, this task was also used for changing build status to failure. However since TeamCity 7.1 [TeamCityBuildProblem](#TeamCityBuildProblem) task should be used for this purpose.



```XML
<TeamCitySetStatus Status="<status value>" Text="{build.status.text} and some aftertext" />

```

`{build.status.text` is substituted with older status text.   
Status can have `SUCCESS` value.

__ __