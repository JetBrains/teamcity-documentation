[//]: # (title: VCS root)
[//]: # (auxiliary-id: VCS root)

<chunk include-id="VCSRoot">

A _VCS root_ in TeamCity defines a [connection to a version control system](configuring-vcs-settings.md) and consists of a set of settings (paths to sources, username, password, and other settings) that defines how TeamCity communicates with a version control (SCM) system to [monitor changes](configuring-vcs-roots.md#Common+VCS+Root+Properties) and get sources for a build. A VCS root can be attached to a build configuration or template. You can add several VCS roots to a build configuration or a template and specify portions of the repository to check out and target paths via [VCS Checkout Rules](vcs-checkout-rules.md).

<anchor name="SharedVCSRoots"/>

VCS roots are created in a project and are available to all the build configurations defined in that project or its [subprojects](project.md#Settings+Propagation).

You can view all VCS roots configured within the project and create/edit/delete/detach them using the __VCS Roots__ section of the __Project Settings__.   
If someone attempts to modify a VCS root that is used in more than one project or build configuration, TeamCity will issue a warning that the changes to the VCS root could potentially affect other projects or build configurations. The user is then prompted to either save the changes and apply them to all the affected projects and build configurations, or to make a copy of the VCS root to be used by either a specific build configuration or project.

On an attempt to create a new VCS root, TeamCity checks whether there are other VCS roots accessible in this project with similar settings. If such VCS roots exist, TeamCity suggests using them.

<anchor name="ConfiguringVCSRoots-CommonVCSRootProps"/>

Once a VCS root is configured, TeamCity regularly queries the version control system for new changes and displays the changes in the [Build Configurations](build-configuration.md) that have the root attached. You can set up your build configuration to trigger a new build each time TeamCity detects changes in any of the build configuration's VCS roots, which suits most cases. When a build starts, TeamCity gets the changed files from the version control and applies the changes to the [Build Checkout Directory](build-checkout-directory.md).

</chunk>
 
 
<seealso>
        <category ref="concepts">
            <a href="project.md">Project</a>
            <a href="build-configuration.md">Build Configuration</a>
            <a href="build-configuration-template.md">Build Configuration Template</a>
        </category>
        <category ref="admin-guide">
            <a href="configuring-vcs-roots.md">Configuring VCS Roots</a>
            <a href="vcs-checkout-rules.md">VCS Checkout Rules</a>
            <a href="vcs-checkout-mode.md">VCS Checkout Mode</a>
            <a href="vcs-labeling.md">VCS Labeling</a>
        </category>
        <category ref="external">
            <a href="https://youtu.be/fttWwJG7C38">Video tutorial: Improving your first build configuration</a>
        </category>
</seealso>