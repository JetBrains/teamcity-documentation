[//]: # (title: Copy, Move, Delete Build Configuration)
[//]: # (auxiliary-id: Copy, Move, Delete Build Configuration)

To copy, move or delete a build configuration, use the __Actions__ pop-up menu located in the upper right corner of the __Build Configuration Settings__. Alternatively, you can manage build configurations on the __General Settings__ tab of the parent project's settings.

### Copy and Move Build Configuration

Build configurations can be copied and moved to another project by project administrators:
* A copy duplicates all the settings of the original build configuration, but no data related to builds is preserved. The copy is created with an empty build history and no statistics. You can copy a build configuration into the same or another project.
* When moving a build configuration between projects, TeamCity preserves its settings and associated data as well as its build history and dependencies.

On copying, TeamCity automatically assigns a new [ID](configuring-general-settings.md#Build+Configuration+ID) to the copy. It is also possible to change the ID manually. Select the _Copy associated user, agent and other settings_ option to ensure that all the settings, such as notification rules or agent's compatibility, are exactly the same for the copied and original build configurations for all the users and agents affected.

If the build configuration uses VCS roots or is associated with a template which is not accessible in the target project (does not belong to the target project or one of its parent projects), the copies of these VCS roots and the template will be created in the target project (also see the related issue [TW-28550](http://youtrack.jetbrains.com/issue/TW-28550)).

<note>

When running TeamCity in the [Professional mode](licensing-policy.md#Licensing+Overview) with the maximum allowed number of build configurations (100 build configurations, or 20 if prior to TeamCity 2017.2) unless you purchased additional build agent licenses), the __Copy__ option will not be displayed for build configurations.
</note>

### Delete Build Configuration

When you delete a build configuration, TeamCity will remove its `.xml` configuration file. After the deletion, there is a [configurable](clean-up.md#Deleted+Build+Configurations+Clean-up) timeout (5 days by default) before the builds of the deleted configuration are removed during the build history clean-up.

If you attempt to delete a build configuration which has [dependent build configurations](dependent-build.md), TeamCity will warn you about it. If you proceed with deletion, the dependencies will no longer function.

__ __