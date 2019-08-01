[//]: # (title: VCS Labeling)
[//]: # (auxiliary-id: VCS Labeling)
TeamCity can label (tag) sources of a particular build (automatically or manually) in your version control. The list of labels applied and their application status is displayed on the on the [Changes tab](working-with-build-results.md#Changes) of the build results page.

On this page:

<tag-list of="chapter" mode="tree" depth="4"/>

## Automatic VCS labeling

You can set TeamCity to label the sources of a build depending on the build status automatically. The process takes place in the background after the build finishes and does not affect the build status, which means that a labeling failure is not a standard [notification event](subscribing-to-notifications.md#Which+Events+Will+Trigger+Notifications). However, the users subscribed for [notifications about failed builds](subscribing-to-notifications.md#Which+Events+Will+Trigger+Notifications) of the current build configuration will be notified about a labeling failure.

Any errors encountered during labeling are reported on the [Changes tab](working-with-build-results.md#Changes) of the build results page.

Labeling is configured for a build configuration/template.

Automatic VCS labeling is configured on the [Build Features page](adding-build-features.md) of the Build Configuration settings.

To configure automatic labeling, you need to specify the root to label and the labeling pattern. If you have [branches configured](working-with-feature-branches.md) for your build configuration, you can label builds from [branches you select](branch-filter.md).

You can override the labeling settings inherited from a template completely; you can also apply different labels to different VCS roots.

<note>

Labeling uses the credentials specified for the VCS root and the write access to the sources repository is required.
</note>

Note that if you change the VCS settings of a build configuration, they will be used for labeling only in the new builds.

"Moving" labels (a label with the same name for different builds, for example, `SNAPSHOT`) are currently supported only for CVS.

For an example of using the Teamcity VCS labeling feature to automate tag creation, refer to this [external posting](http://laurentkempe.com/2010/06/03/Build-and-Deployment-automation-VCS-Root-and-Labeling-in-TeamCity/).

## Manual VCS labeling

To label the sources manually:

Navigate to the [build results](working-with-build-results.md) page, click __Actions__ and select __Label this build sources__ from the drop\-down.

Manual labeling uses the VCS settings actual for the build.

<anchor name="SubversionLabelingRules"/>

## Subversion Labeling Rules
[//]: # (AltHead: SubversionLabelingRules)

To label Subversion VCS roots, additional configuration – [labeling rules](subversion.md#Labeling+settings) defining the SVN repository structure – is required.

Labeling rules are specified as newline\-delimited rules in the following format:

```Plain Text
TrunkOrBranchRepositoryPath => tagDirectoryRepositoryPath

```

The repository paths can be relative and absolute (starting from `/`). Absolute paths are resolved from the SVN repository root (the topmost directory you have in your repository), relative paths are resolved from the TeamCity VCS root.

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



In this case the labeling rules should be:


```Plain Text
/project/trunk=>/project/tags
/project/branch1=>/project/tags
/project/branch2=>/project/tags

```



## Labeling Rule Examples

You can use variables substitution in both labeling rules and labeling patterns. See a labeling rule example in a VCS root used in different configurations:


```Plain Text
/projects/%projectName%/trunk => /projects/%projectName%/tags

```



This will require you to set the `%projectName%` [configuration parameter](configuring-build-parameters.md) in the build configuration settings.

By default, TeamCity will append the label name to the end of the specified target path. If you want to have a different directory structure and put the label in the middle of the target path, you can use the following syntax:


```Plain Text
/project/trunk => /tagged_configurations/%%system.build.label%%/project
/modules/module1/trunk => /tagged_configurations/%%system.build.label%%/module1
/modules/module2/trunk => /tagged_configurations/%%system.build.label%%/module2

```



Thus, `%%system.build.label%%` will be replaced with the tag name (note the double `%%` sign at the beginning \- it is important).

 
