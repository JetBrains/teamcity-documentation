[//]: # (title: What's New in TeamCity 2022.12)
[//]: # (auxiliary-id: What's New in TeamCity 2022.12)

## Improved support for draft pull requests in the Pull Requests plugin

Starting from this version, you can configure the [Pull Requests build feature](pull-requests.md)
to ignore [GitHub draft pull requests](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests#draft-pull-requests) by checking the **Ignore Drafts** box in the build feature settings.
TeamCity will ignore draft pull requests until their status changes.

By default, the Pull Requests build feature loads the GitHub draft pull request information and runs builds on draft pull requests.
The build page displays the "Draft" status and icon next to the pull request number:

<img src="draft_pr.png" alt="Draft PR" width="556"/>

When the status of the draft pull request changes to "Ready for review" in GitHub, the build page reflects the change:
<img src="draft_pr_changed.png" alt="Ready for review PR" width="556"/>

## Build reordering is supported by the Sakura UI

You can now manually reorder builds in the build queue by dragging them to the desired position in the Sakura UI.

## Amazon EC2 launch templates customization

Starting from this version, TeamCity allows customizing [Amazon EC2 launch templates](setting-up-teamcity-for-amazon-ec2.md#Amazon+EC2+Launch+Templates+support). You can now use the same launch template to run various instances that differ in some parameters only.
Check the **Customize Launch Template** box and modify the launch template's values as required.

## New parameter for Perforce shelved changelist ID

Our users requested a parameter for [the shelved changelist ID](https://youtrack.jetbrains.com/issue/TW-78722/) regardless of the VCS Root ID. TeamCity now provides the `vcsRoot.1.shelvedChangelist` [configuration parameter](predefined-build-parameters.md) with the ID of the changelist whose changes triggered this build.



## Roadmap

See the [TeamCity roadmap](https://www.jetbrains.com/teamcity/roadmap/#teamcity-roadmap) to learn about future updates.