[//]: # (title: Project)
[//]: # (auxiliary-id: Project)

A _project_ in TeamCity is a collection of [build configurations](managing-builds.md). A TeamCity project can correspond to a software project, a specific version/release of a project or any other logical group of the build configurations.   
The project has a name, an [ID](identifier.md), and an optional description.   
In TeamCity, user [roles and permissions](managing-roles-and-permissions.md) are managed on a per-project basis.

## Project Hierarchy

Projects can be nested and organized into a tree allowing for hierarchical display and settings propagation. The hierarchy is defined by the project administrators and is the same for all the TeamCity users.   
You can view the hierarchy on the overview page, in the __Projects__ pop-up menu, and in breadcrumbs.

>To see the Projects menu, click the arrow button next to the _Projects_ link in the upper left corner, or use the "P" keyboard shortcut.
>
{style="tip"}

### Settings Propagation

The project hierarchy is used in the following ways:

Settings defined on a project level are propagated to all the subprojects (recursively). These include:
* [Parameters](configuring-build-parameters.md)
* [Clean-up rules](teamcity-data-clean-up.md#Clean-up+Rules)
* [Versioned Settings](storing-project-settings-in-version-control.md) (Settings for synchronizing the project settings with version control) 
* it is possible to export a project with all its subprojects and external dependencies using the [Settings Export](project-export.md) page.

Entities defined in a project become available to all the build configurations residing under the project and its subprojects. These include:
* [VCS Roots](vcs-root.md)
* [Build Configuration Template](build-configuration-template.md)
* [Issue Trackers](integrating-teamcity-with-issue-tracker.md) 
* [Shared Resources](shared-resources.md)
* [SSH keys](ssh-keys-management.md)
* [Maven Settings](maven-server-side-settings.md#User-Level+Settings)
* [Meta-Runners](working-with-meta-runner.md)

For example, if you want to share a VCS root among several projects, you have to move it to the common parent of all these projects. If a VCS root must be shared among all projects, it must be created in the __&lt;Root project&gt;__.

A setting referencing a project affects the project and all its subprojects. These include:
* [User and User group roles](managing-roles-and-permissions.md)
* [Investigations](investigating-and-muting-build-failures.md)
* [Muted Problems](investigating-and-muting-build-failures.md)
* [Notification rules](adding-notification-rules.md)

Note that associating a project with an [agent pool](configuring-agent-pools.md) is not propagated to its subprojects and affects only the build configurations residing directly in the project.

### Root Project

TeamCity always has a __&lt;Root project&gt;__ as the top of the project hierarchy. The root project has most of the properties of a usual project, and the settings configured in the root project are available to all the other projects on the server.

The root project is special in the following ways:
* It is present by default and cannot be deleted.
* It is the top-level project, so it has no parent project.
* It can have no build configurations.
* It does not appear in the user-level UI and is mostly present as an entity in Administration UI only.

 <seealso>
        <category ref="concepts">
            <a href="managing-builds.md">Build Configuration</a>
        </category>
        <category ref="admin-guide">
            <a href="creating-and-editing-projects.md">Creating and Editing Projects</a>
            <a href="creating-and-editing-build-configurations.md">Creating and Editing Build Configurations</a>
            <a href="project-export.md">Project Export</a>
        </category>
</seealso>