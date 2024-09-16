[//]: # (title: VCS Labeling)
[//]: # (auxiliary-id: VCS Labeling)

TeamCity can label (tag) sources of a particular build (automatically or manually) in your Version Control System. The list of applied labels and their application status is displayed on the __[Changes](build-results-page.md#Changes+Tab)__ tab of __Build Results__.

## Automatic VCS labeling

You can set TeamCity to automatically label the sources of a build depending on the build status. Labeling is not a standard [notification event](adding-notification-rules.md#Which+Events+Will+Trigger+Notifications) — it takes place in the background after the build finishes and does not affect the build status. However, the users subscribed to [notifications about failed builds](adding-notification-rules.md#Which+Events+Will+Trigger+Notifications) of the current build configuration will be notified about a labeling failure.

Labeling is configured per a build configuration/template, as a [build feature](adding-build-features.md). When adding this feature, you need to specify the root to label and a labeling pattern. If there are [branches configured](working-with-feature-branches.md) for the current build configuration, you can label only builds from specific [branches you select](branch-filter.md).

It is possible to override the labeling settings inherited from a template completely and apply different labels to different VCS roots.

>Labeling uses credentials specified for the VCS root. The write access to the source repository is required.
> 
{style="note"}

Note that if you change VCS settings of a labeled build configuration, they will be used for labeling only in the following new builds.

"Moving" labels (a label with the same name for different builds, for example, `SNAPSHOT`) are currently supported only for CVS.

For an example of using the Teamcity VCS labeling feature to automate tag creation, refer to this [external post](http://laurentkempe.com/2010/06/03/Build-and-Deployment-automation-VCS-Root-and-Labeling-in-TeamCity/).

## Manual VCS labeling

To label the sources manually, navigate to the __[Build Results](working-with-build-results.md)__ page, click __Actions__, and select _Label this build sources_ from the drop-down menu.

Manual labeling uses the VCS settings actual for the build.

<anchor name="SubversionLabelingRules"/>

## Subversion Labeling Rules
[//]: # (AltHead: SubversionLabelingRules)

To label Subversion VCS roots, it is required to set [labeling rules](subversion.md#Labeling+settings) defining the SVN repository structure.

Labeling rules are specified as newline-delimited rules in the following format:

```Plain Text
TrunkOrBranchRepositoryPath => tagDirectoryRepositoryPath

```

The repository paths can be relative and absolute (starting with `/`). Absolute paths are resolved from the SVN repository root (the topmost directory you have in your repository), relative paths are resolved from the TeamCity VCS root.

When creating a label, the sources residing under `TrunkOrBranchRepositoryPath` will be put into the `tagDirectoryRepositoryPath/tagName` directory, where `tagName` is the name of the label as defined by the labeling pattern of the build configuration.

If no sources match the `TrunkOrBranchRepositoryPath`, no label will be created.

The `tagDirectoryRepositoryPath` path must already exist in the repository.

If the `tagDirectoryRepositoryPath` directory already contains a subdirectory with the current label name, the labeling process will fail, and the old tag directory won't be deleted or affected.

For example, there is a VCS root with the URL `svn://address/root/project` where `svn://address/root` is the repository root, and the repository has the structure:

```Plain Text
-project
--trunk
--branch1
--branch2
--tags

```

In this case, the labeling rules should be:

```Plain Text
/project/trunk=>/project/tags
/project/branch1=>/project/tags
/project/branch2=>/project/tags

```

## Labeling in Perforce

Since 2021.2, TeamCity creates [automatic labels](https://www.perforce.com/manuals/p4guide/Content/P4Guide/labels.alias.html) instead of static ones. Automatic labels work as aliases for changelists. In a label's `Revision` field, TeamCity displays the revision checked out in the current build. For the `View` field, it uses the mapping associated with all paths of the current VCS root.

For Perforce labels, TeamCity supports only include rules and ignores exclude rules.

The optional message format setting allows you to specify a message written to the description field of Perforce labels.

<img src="dk-vcslabeling-p4message.png" width="706" alt="Custom message"/>

If you prefer using static labels, you can enable the previous behavior by setting the `teamcity.perforce.useStaticLabels=true` [internal property](server-startup-properties.md#TeamCity+Internal+Properties).
{product="tc"}

## Labeling Rule Examples

You can use variables substitution in both labeling rules and labeling patterns. See a labeling rule example in a VCS root used in different configurations:

```Plain Text
/projects/%\projectName%/trunk => /projects/%\projectName%/tags

```

This will require you to set the `%\projectName%` [configuration parameter](configuring-build-parameters.md) in the build configuration settings.

By default, TeamCity will append the label name to the end of the specified target path. If you want to have a different directory structure and put the label in the middle of the target path, you can use the following syntax:

```Plain Text
/project/trunk => /tagged_configurations/%\%system.build.label%%/project
/modules/module1/trunk => /tagged_configurations/%\%system.build.label%%/module1
/modules/module2/trunk => /tagged_configurations/%\%system.build.label%%/module2

```

Thus, `%\%system.build.label%%` will be replaced with the tag name (note the double `%\%` sign at the beginning — it is important).