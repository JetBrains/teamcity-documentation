[//]: # (title: Archiving Projects)
[//]: # (auxiliary-id: Archiving Projects)
If a project is not in an active development state, you can _archive_ it using the __Actions__ menu in the top right of the project settings page. On the __Archive project__ screen, you can choose whether to cancel the builds which are already in the queue using the corresponding option (enabled by default).

When _archived_:
* All the __subprojects__ of the current project __are archived__ as well.
* All project's __build configurations are__ automatically __[paused](build-configuration.md#Build+Configuration+State)__.
* Automatic checking for changes in the project's [VCS roots](configuring-vcs-roots.md) is not performed if the VCS roots are not used in other non\-archived projects.
* As part of pausing, automatic build triggering is disabled. However, builds of the project can be triggered manually or automatically as a part of a build chain.
* All data (settings, build results, artifacts, build logs, and so on) of the project's build configurations are preserved \- you can still edit settings of the archived project or its build configurations.
* Archived projects do not appear in most user\-facing projects lists and in IDEs including the list of build configurations for remote run.

By default, permissions to archive projects are given to project and system administrators.

If the parent project of an archived subproject is displayed in the project list (for example, on the [Projects Overview](managing-projects-and-build-configurations.md#Configuring+Visibility) page), the archived subprojects are displayed with the corresponding note.

### Dearchiving Projects

If you need to dearchive a project, you can do it using the __Actions__ menu in the top right of the project settings page or the __More__ menu next to the project on the Root project settings page.



 __  __

__See also:__



__Concepts__: [Project](project.md) | [Build Configuration](build-configuration.md)
