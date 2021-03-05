[//]: # (title: Managing Projects and Build Configurations)
[//]: # (auxiliary-id: Managing Projects and Build Configurations)

[Projects](project.md) and their entities in TeamCity can be configured:
* via the web UI
* using the [REST API](https://www.jetbrains.com/help/teamcity/rest/teamcity-rest-api-documentation.html)
* programmatically using DSL based on the Kotlin language if the [versioned settings](storing-project-settings-in-version-control.md) feature is enabled

## Configuring Visibility

The __Configure Visible Projects__ pop-up menu on the __Projects__ page allows reordering the projects and change their visibility in the list of projects.

To add an individual project to the __Projects__ page or remove it from this page, toggle the "eye" button on the __Project Home__ page:

<img src="eye-button.png" width="750" alt="Disk usage in details"/>

Note that if a project has [archived subprojects](archiving-projects.md) / [paused build configurations](build-configuration.md#Pausing+%2F+Activating+several+build+configurations+of+a+project), they will also be displayed on the __Overview__ page and will be marked correspondingly.

<seealso>
        <category ref="concepts">
            <a href="project.md">Project</a>
            <a href="build-configuration.md">Build Configuration</a>
        </category>
</seealso>