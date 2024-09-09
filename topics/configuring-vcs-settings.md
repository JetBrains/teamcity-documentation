[//]: # (title: Configuring VCS Settings)
[//]: # (auxiliary-id: Configuring VCS Settings)

A Version Control System (VCS) is a system for tracking the revisions of the project source files. It is also known as SCM (source code management) or a revision control system. The following VCSs are supported by TeamCity out-of-the-box: [Git](git.md), [Subversion](subversion.md), [Mercurial](mercurial.md), [Perforce](perforce.md), [Azure DevOps](team-foundation-version-control.md), [CVS](cvs.md), [StarTeam](starteam.md).
{instance="tc"}

A Version Control System (VCS) is a system for tracking the revisions of the project source files. It is also known as SCM (source code management) or a revision control system. The following VCSs are supported by TeamCity out-of-the-box: [Git](git.md), [Subversion](subversion.md), [Mercurial](mercurial.md), [Perforce](perforce.md), [Azure DevOps](team-foundation-version-control.md).
{instance="tcc"}

Connection to a version control system is defined by a TeamCity [VCS root](vcs-root.md). A [project](project.md) or a [build configuration](managing-builds.md) in TeamCity can have one or more VCS roots attached; a build configuration can also define the workspace for the builds via other checkout options like [Checkout Rules](vcs-checkout-rules.md).

TeamCity always monitors the repositories from the server-side to detect changes and display them in the UI. Depending on the specified [VCS Checkout Mode](vcs-checkout-mode.md) the actual repository checkout can also happen on the agent-side.

TeamCity performs VCS-related operations per each VCS root separately, thus it is advised to reuse VCS roots with same settings.

When [parameter references](using-build-parameters.md) are used in a VCS root, TeamCity performs VCS-related operations per each "VCS root instance", where "instance" is a unique set of VCS root parameters after references resolution. Adding parameters to the VCS roots does not reduce the number of VCS operations performed, it just allows sharing settings more effectively.

## Attach VCS Root

<procedure title="Attach or Create VCS Root">
<step><p>Go to <b>Administration</b> and click the project you want to configure.</p></step>
<step><p>From the project's <b>General Settings</b> page, click the relevant build under <b>Build Configurations</b></p></step>
<step><p>Select <b>Version Control Settings</b> from the sidebar.</p></step>
<step><p>Click <b>Attach VCS root</b>.</p></step>
<step><p>If there is at least one existing VCS root available, TeamCity offers a choice between the following actions:</p>
<list>
<li><p><b>Attach existing VCS root</b> — select the VCS root to attach and then complete the <b>Update Checkout Rules</b> form.</p></li>
<li><p><b>Create new VCS root</b> — configure the new VCS root, following the guidelines in <a href="configuring-vcs-roots.md">Configuring VCS Roots</a>.</p></li>
</list>
<note>
<p>If there is no existing VCS root available, TeamCity jumps straight to the <b>Create new VCS root</b> page.</p>
</note>
</step>
</procedure>

### Configure Checkout Rules

When several VCS roots are attached or you need to check out only a portion of the repository, specify the [checkout rules](vcs-checkout-rules.md) for the VCS root to provide advanced possibilities to control sources checkout. With the rules you can exclude and/or map paths to a different location on the build agent during checkout.

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

<table><tr>

<td>

Setting

</td>

<td>

Description

</td></tr><tr>

<td>

<anchor name="show-changes-from-snapshot-dependencies"/>

Show changes from snapshot dependencies

</td>

<td>

Configure whether TeamCity will __[show changes from snapshot dependencies](build-dependencies-setup.md#show-changes-from-dependencies)__. This also affects treatment of pending changes in schedule trigger.

</td></tr><tr>

<td>

<anchor name="ConfiguringVCSSettings-excludeDefaultBranch"/>

Exclude default branch changes from other branches

</td>

<td id="excludeDefaultBranch">

By default, when displaying pending changes in a feature branch or changes of a build on a branch, TeamCity includes changes in the [default branch](working-with-feature-branches.md#Default+Branch) (till a build in the default branch) as well. This allows tracking the cases when a commit that broke a build was fixed in the default branch, but not in a feature branch.

However, for large projects with multiple teams simultaneously working on lots of different branches this means that all the project committers (regardless of the branch they are committing to) will be notified when, for example, a commit in the default branch broke the build or if a force push was performed.

If you want to see the changes in a feature branch only, check the box to exclude changes in the default branch from being displayed in other branches.

</td></tr></table>

### Branch Filter

You can use a [branch filter](branch-filter.md) to limit the set of branches available for the build configuration. By default, no limits are applied.

## Other VCS-Related Settings

* Configure a [VCS trigger](configuring-vcs-triggers.md) if you want the build to be started on new changes detection.
* Additionally, you can add a label into the version control system for the sources used for a particular build by means of the [VCS Labeling](vcs-labeling.md) build feature.
 
 <seealso>
        <category ref="admin-guide">
            <a href="configuring-vcs-roots.md">Configuring VCS Roots</a>
            <a href="vcs-checkout-rules.md">VCS Checkout Rules</a>
            <a href="vcs-checkout-mode.md">VCS Checkout Mode</a>
            <a href="vcs-labeling.md">VCS Labeling</a>
        </category>
</seealso>