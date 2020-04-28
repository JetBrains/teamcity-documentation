[//]: # (title: Automatic Merge)
[//]: # (auxiliary-id: Automatic Merge)

The _Automatic Merge_ build feature tracks builds in branches matched by the configured filter and merges them into a specified destination branch if the build satisfies the condition configured (for example, the build is successful).   
The feature is supported for Git and Mercurial VCS roots for build configurations with enabled [feature branches](working-with-feature-branches.md).   
TeamCity also allows merging branches [manually](working-with-feature-branches.md#Manual+branch+merging).

## Automatic Merge Settings

Check [Adding Build Features](adding-build-features.md) for notes on how to add a build feature.   
All branches that are used in this feature __must__ be present in a repository and included into the __Branch Specification__ of the current build configuration.

<table>

<tr><td>

Option

</td>
<td>

Description

</td>
</tr><tr>
<td>

Watch builds in branches

</td><td>

Specify the branches whose builds' sources will be merged. Read more in [Branch Filter](branch-filter.md).

</td>
</tr><tr>
<td>

Merge into branch

</td><td>

A [logical name](working-with-feature-branches.md#Logical+branch+name) of the destination branch the sources will be merged to. Parameter references are supported here. The branch __must__ be present in a repository and included into the __Branch Specification__.

</td>
</tr><tr>
<td>

Merge commit message

</td><td>

A message for a merge commit. The default is set to `Merge branch '%teamcity.build.branch%'`. Parameter references are supported here.

</td>
</tr><tr>
<td>

Perform merge if

</td><td>

A condition defining when the merge will be performed (either for successful builds only, or if build from the branch does not add new problems to destination branch).

</td>
</tr><tr>
<td>

Merge policy

</td><td>

Select to create a merge commit or do a fast\-forward merge.

</td>
</tr><tr>
<td>

Run policy

</td><td>

Choose when to merge:
* _Merge after build finish_: the build finishes, and then the merge starts. The build duration does not include the merging time. Dependent builds can start even if merging is still in process.
* _Merge before build finish_: the build is considered finished only when the merge is completed. Dependent builds will start only after the merge is completed.

</td>
</tr>
</table>

## Cascading Merge

It is possible to define a cascade of merge operations by adding several such build features to a build configuration.

For example, you want to automatically merge all feature branches into an `integration` branch, and then configure another merge from the `integration` to the default branch. To achieve this, you can add two _Automatic Merge_ build features: one watching `+:feature-*` branches and merging into `integration` branch and the second watching `+:integration` branch and merging into `default` branch. The build configuration should then allow for building `feature-*` and `integration` branches.
    
See also a related [TeamCity blog post](http://blog.jetbrains.com/teamcity/2013/10/automatic-merge/).

__ __