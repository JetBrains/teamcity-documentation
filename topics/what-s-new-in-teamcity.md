[//]: # (title: What's New in TeamCity 2022.12)
[//]: # (auxiliary-id: What's New in TeamCity 2022.12;What's New in TeamCity)

## Build reordering is supported by the Sakura UI

You can now manually reorder builds in the build queue by dragging them to the desired position in the Sakura UI.

## Amazon EC2 launch templates customization

Starting from this version, TeamCity allows customizing [Amazon EC2 launch templates](setting-up-teamcity-for-amazon-ec2.md#Amazon+EC2+Launch+Templates+support). You can now use the same launch template to run various instances that differ in some parameters only.
Check the **Customize Launch Template** box and modify the launch template's values as required.

## New parameter for Perforce shelved changelist ID 

Our users requested a parameter for [the shelved changelist ID](https://youtrack.jetbrains.com/issue/TW-78722/) regardless of the VCS Root ID. TeamCity now provides the `vcsRoot.1.shelvedChangelist` [configuration parameter](predefined-build-parameters.md) with the ID of the changelist whose changes triggered this build.


## Fixed issues
{product="tcc"}

See [TeamCity Build 124079 release notes](teamcity-release-notes-build-124079.md).

## Roadmap

See the [TeamCity roadmap](https://www.jetbrains.com/teamcity/roadmap/#teamcity-roadmap) to learn about future updates.