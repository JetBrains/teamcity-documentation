[//]: # (title: Configuring VCS Settings)
[//]: # (auxiliary-id: Configuring VCS Settings)

## VCS Settings Overview 

A Version Control System (VCS) is a system for tracking the revisions of the project source files. It is also known as SCM (source code management) or a revision control system. The following VCSs are supported by TeamCity out-of-the-box: [Git](git.md), [Subversion](subversion.md), [Mercurial](mercurial.md), [Perforce](perforce.md), [Team Foundation Server](team-foundation-server.md), [CVS](cvs.md), [StarTeam](starteam.md), [Visual SourceSafe](visual-sourcesafe.md).

Connection to a version control system is defined by a TeamCity [VCS root](vcs-root.md). A project or a [build configuration](build-configuration.md) in TeamCity can have one or more VCS roots attached; a build configuration can also define the workspace for the builds via other checkout options like [Checkout Rules](vcs-checkout-rules.md).

TeamCity always monitors the repositories from the server-side to detect changes and display them in the UI. Depending on the specified [VCS Checkout Mode](vcs-checkout-mode.md) the actual repository checkout can also happen on the agent-side.

TeamCity performs VCS-related operations per each VCS root separately, thus it is advised to reuse VCS roots with same settings.

When [parameter references](configuring-build-parameters.md#Using+Build+Parameters+in+Build+Configuration+Settings) are used in a VCS root, TeamCity performs VCS-related operations per each "VCS root instance", where "instance" is a unique set of VCS root parameters after references resolution. Adding parameters to the VCS roots does not reduce the number of VCS operations performed, it just allows sharing settings more effectively.

## Attach VCS Root

VCS settings are configured on the __Version Control Settings__ page for a project or a build configuration: you can attach an existing [VCS root](configuring-vcs-roots.md) to your project/build configuration, or create a new one to be attached. This is the main part of VCS parameters setup; a VCS Root is a description of a version control system where project sources are located. Learn more about VCS Roots and configuration details [here](configuring-vcs-roots.md).

### Configure Checkout Rules

When several VCS roots are attached or you need to checkout only a portion of the repository, specify the [checkout rules](vcs-checkout-rules.md) for the VCS root to provide advanced possibilities to control sources checkout. With the rules you can exclude and/or map paths to a different location on the Build Agent during checkout.

## Configuring Checkout Options for Build Configuration

### Checkout Settings

<table><tr>

<td>

Setting

</td>

<td>

Description

</td></tr><tr>

<td>

VCS Сheckout Mode

</td>

<td>

To define how project sources reach an agent, use the [VCS Checkout Mode](vcs-checkout-mode.md) options.

</td></tr><tr>

<td>

Сheckout Directory

</td>

<td>

The [Build Checkout Directory](build-checkout-directory.md) is a directory on the TeamCity agent machine where all sources of all builds are checked out into.

</td></tr><tr>

<td>

Clean build


</td>

<td>

Define whether you want to clean all files in the checkout directory before the build. See [Clean Checkout](clean-checkout.md) for details.

</td></tr></table>

### Changes Calculation Settings

This section, formerly called the Display settings, was renamed into Changes calculation settings __since TeamCity 2017.1.__

 

<table><tr>

<td>

Setting

</td>

<td>

Description

</td></tr><tr>

<td>

Show changes from snapshot dependencies

</td>

<td>

Configure whether TeamCity will __[show changes from snapshot dependencies](build-dependencies-setup.md#show-changes-from-dependencies)__. This also affects treatment of pending changes in schedule trigger.

</td></tr><tr>

<td>

Exclude default branch changes from other branches

</td>

<td>

<anchor name="excludeDefaultBranch"/>

By default, when displaying pending changes in a feature branch or changes of a build on a branch, TeamCity includes changes in the [default branch](working-with-feature-branches.md#Default+branch) (till a build in the default branch) as well. This allows tracking the cases when a commit that broke a build was fixed in the default branch, but not in a feature branch.

However, for large projects with multiple teams simultaneously working on lots of different branches this means that all the project committers (regardless of the branch they are committing to) will be notified when, for example, a commit in the default branch broke the build or if a force push was performed.

If you want to see the changes in a feature branch only, check the box to exclude changes in the default branch from being displayed in other branches.

</td></tr></table>

### Branch Filter

You can use a [Branch Filter](branch-filter.md) to limit the set of branches available for the build configuration. By default, no limits are applied.


## Other VCS-Related Settings
* Configure [VCS trigger](configuring-vcs-triggers.md) if you want the build to be started on new changes detection.
* Additionally, you can add a label into the version control system for the sources used for a particular build by means of [VCS Labeling](vcs-labeling.md) build feature.
 
 
 
 __  __

__See also:__

__Administrator's Guide__: [Configuring VCS Roots](configuring-vcs-roots.md) | [VCS Checkout Rules](vcs-checkout-rules.md) | [VCS Checkout Mode](vcs-checkout-mode.md) | [VCS Labeling](vcs-labeling.md)

__ __