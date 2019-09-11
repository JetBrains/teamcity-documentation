[//]: # (title: Managing Projects and Build Configurations)
[//]: # (auxiliary-id: Managing Projects and Build Configurations)
[Projects](project.md) and their entities in TeamCity can be configured:
* via the web UI
* using the [REST API](rest-api.md)
* programmatically using DSL based on the Kotlin language if the [versioned settings](storing-project-settings-in-version-control.md) feature is enabled

### Configuring Visibility

The __Configure Visible Projects__ pop-up window on the __Projects Overview__ page allows reordering the projects and change their visibility in the list of projects.

To add an individual project to the __Projects Overview__ page or remove it from this page, toggle the "eye" button on the __Project Home__ page:

<img src="eye-button.png" width="986" alt="Disk usage in details"/>

Note that if a project has [archived subprojects](archiving-projects.md) / [paused build configurations](build-configuration.md#Pausing+%2F+Activating+several+build+configurations+of+a+project), they will also be displayed on the overview page and will be marked correspondingly.


In this section:

<toc>
</toc>

__  __

__See also:__

__Concepts__: [Project](project.md) | [Build Configuration](build-configuration.md)
