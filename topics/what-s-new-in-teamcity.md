[//]: # (title: What's New in TeamCity 2022.12)
[//]: # (auxiliary-id: What's New in TeamCity 2022.12;What's New in TeamCity)

## Build reordering is supported by the Sakura UI

You can now manually reorder builds in the build queue by dragging them to the desired position in the Sakura UI.

## Amazon EC2 launch templates customization

Starting from this version, TeamCity allows customizing [Amazon EC2 launch templates](setting-up-teamcity-for-amazon-ec2.md#Amazon+EC2+Launch+Templates+support). You can now use the same launch template to run various instances that differ in some parameters only.
Check the **Customize Launch Template** box and modify the launch template's values as required.

## New parameter for Perforce shelved changelist ID 

Our users requested a parameter for [the shelved changelist ID](https://youtrack.jetbrains.com/issue/TW-78722/) regardless of the VCS Root ID. TeamCity now provides the `vcsRoot.1.shelvedChangelist` [configuration parameter](predefined-build-parameters.md) with the ID of the changelist whose changes triggered this build.

## Changes in metrics reporting

### OpenMetrics compliance

The reported [Prometheus metrics](teamcity-monitoring-and-diagnostics.md#Metrics) of types _"summary"_ and _"histogram"_ are now compliant with [the OpenMetrics specification](https://openmetrics.io/).

As a part of this change, the summary metrics that had `_total` suffixes earlier have `_sum` suffixes now.
For instance, the `build_queue_optimization_time_milliseconds_total` metric is now named `build_queue_optimization_time_milliseconds_sum`.

To preserve the history in Grafana, use of the options below:

* _Recommended_. Use the `or` operator in graph specifications to use data  with both `_total` and `_sum` suffixes:

```Text
sum(increase(vcs_changes_checking_milliseconds_sum{type="COLLECT_CHANGES"}[1m])) or
sum(increase(vcs_changes_checking_milliseconds_total{ type="COLLECT_CHANGES"}[1m]))
```

* Use the Prometheus relabeling configuration to rename the metrics on the fly, before they are sent to the Prometheus database. This will convert the new `_sum` metrics to the previous `_total` metrics.

### Removal of the "experimental" tag

TeamCity will no longer report the `experimental` tag for metrics. The `?experimental=true` URL parameter for metrics endpoints will still work, and some of the metrics will still have the experimental status.

## Fixed issues
{product="tcc"}

See [TeamCity Build ??? release notes](teamcity-release-notes-build-???.md).

## Roadmap

See the [TeamCity roadmap](https://www.jetbrains.com/teamcity/roadmap/#teamcity-roadmap) to learn about future updates.