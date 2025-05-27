[//]: # (title: Ordering Projects and Build Configurations)
[//]: # (help-id: Ordering Projects and Build Configurations)

By default, TeamCity displays projects, their subprojects, build configurations, and templates in the alphabetical order.

Project administrators can change this default order:

1. <include from="common-templates.md" element-id="open-project-settings-tab"><var name="tab-name" value="General"/></include>
2. Click **Reorder** in the subprojects or build configurations section.

    <img src="dk-reorder-configs-and-subprojects.png" width="706" alt="Reorder projects and configurations"/>

Users can customize their **Favorites** list to quickly access selected projects. Projects are added automatically for their creators. To edit the list or reorder projects, click the pencil icon next to **Favorites** in the TeamCity sidebar.

<img src="dk-edit-favorites.png" width="706" alt="Edit Favorites"/>

Note that if a project has [archived subprojects](archiving-projects.md) / [paused build configurations](changing-build-configuration-status.md#Pausing+Several+Build+Configurations+in+Project), they will also be displayed on the __Overview__ page and will be marked correspondingly.

<seealso>
        <category ref="admin-guide">
            <a href="kotlin-dsl.md#How+to+reorder+projects+and+build+configurations">How to reorder projects and build configurations via Kotlin DSL</a>
        </category>
</seealso>
