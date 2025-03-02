[//]: # (title: What's New in TeamCity On-Premises 2025.03)

<snippet id="2025-03-tc">

## Output Parameters

The **Parameters** tab of build configuration setting now features two tabs: input and output parameters.

<img src="dk-add-output-param.png" width="706" alt="Create output param"/>

* Input parameters are your regular build parameters that existed before version 2025.03.
* Output parameters are build parameters with an explicit access permission. Values of these parameters can be read by any dependent configuration via the `dep.<config name>.<parameter name>` syntax.

This update enhances security and grants you more control over parameters. Previously, dependent configurations could read any non-password parameter. Starting with version 2025.03, only explicitly exposed parameters can be shared.

Learn more: [](use-parameters-in-build-chains.md)


## Perforce Manual and Automatic Merge Support

TeamCity now supports merging code changes from one Perforce stream to another. This enhancement enables two features:

* The [](automatic-merge.md) build feature now supports build configurations that utilize Perforce VCS roots.
* The **Actions** build menu now includes the option to [merge code changes manually](working-with-feature-branches.md#Manual+Branch+Merging).


</snippet>

