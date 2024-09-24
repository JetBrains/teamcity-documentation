[//]: # (title: Entity IDs)
[//]: # (auxiliary-id: Entity IDs;Identifier)

An _ID_ is an identifier given to TeamCity entities ([projects](project.md), [build configurations](managing-builds.md), [templates](build-configuration-template.md), [VCS roots](vcs-root.md), and so on).

Each entity has two identifiers:
* [external ID](#External+IDs)
* [Universally Unique ID](#Universally+Unique+IDs), or UUID

## External IDs

The so-called _external identifiers_ are configured in the TeamCity web UI (for example, Project ID) and must be unique within all the objects of the same type on the entire server. Build configurations and templates share the same ID space.

IDs can contain only alphanumeric characters and underscores (`_`) — maximum 80 characters — and should start with a Latin letter.

### Using External IDs

External IDs are used:
* in URLs of the web interface (including the [NuGet](nuget.md) feed), for example, [`https://teamcity.jetbrains.com/project.html?projectId=TeamCityPluginsByJetBrains`](https://teamcity.jetbrains.com/project.html?projectId=TeamCityPluginsByJetBrains);
* in the [`dep.`](predefined-build-parameters.md#Dependency+Parameters) and [`vcsRoot.`](predefined-build-parameters.md#VCS+Parameters) parameter references;
* in [REST API](https://www.jetbrains.com/help/teamcity/rest/teamcity-rest-api-documentation.html) and build scripts used to automate actions with TeamCity (for example, download artifacts via direct URLs or Ivy);
* in the configuration files storing settings of projects and build configurations under [`<TeamCity Data Directory>`](teamcity-data-directory.md)`/config`;
* in file and directory names under [`<TeamCity Data Directory>`](teamcity-data-directory.md)`/system` (for example, build artifacts storage).

 <anchor name="AssigningIDs"/>

### Assigning External IDs

By default, TeamCity automatically suggests an ID for an object by converting its name to match the ID requirements and prefixing that with the ID of the parent project. The ID can be modified manually.

It is recommended to leave the automatically generated IDs as is unless there are special considerations to modify them.

If you consider moving projects between several TeamCity server installations, it is a good practice to make sure that all the IDs are globally unique.

<note>

On changing the ID of a project or build configuration, all the related URLs (including the web UI, artifact download links, and REST API) will change. If any of the URLs containing the old IDs were bookmarked or hardcoded in the scripts, they will stop functioning and will need to be updated. At the moment of the ID change, the correspondingly named directories under TeamCity Data Directory (including directories storing settings and artifacts) are renamed, and this can consume some time.
</note>

To reset the IDs to match the default scheme for all projects, VCS roots, build configurations, and templates, use the __Bulk edit IDs__ action on the __Administration__ page of the parent [project](project.md). To use the automatically generated ID after it has been modified or after you change an existing object name, you can regenerate ID using the __Regenerate ID__ action.

When you copy a project, TeamCity automatically assigns new IDs to all the child elements. The IDs can be previewed and changed in the _Copy_ dialog. When you move an object, its ID is preserved and you might want to use __Regenerate ID__ action to make the ID reflect the new placement.

## Universally Unique IDs

TeamCity projects, build configurations, and VCS roots have a UUID which is an automatically generated, globally unique ID. UUID is stored in the corresponding entity XML configuration file located in the [`<TeamCity Data Directory>`](teamcity-data-directory.md)`/config` directory. These UUID must never be edited manually. When a new entity is created by placing a file to the Data Directory, it should have no `uuid` attribute. TeamCity will generate the UUID automatically, and it will persist in the file.

<!--[//]: # (Internal note. Do not delete. "Identifierd161e161.txt")-->    

<seealso>
        <category ref="concepts">
            <a href="project.md">Project</a>
            <a href="managing-builds.md">Build Configuration</a>
        </category>
        <category ref="admin-guide">
            <a href="creating-and-editing-projects.md">Creating and Editing Projects</a>
            <a href="creating-and-editing-build-configurations.md">Creating and Editing Build Configurations</a>
            <a href="configuring-vcs-roots.md">Configuring VCS Roots</a>
            <a href="accessing-server-by-http.md">Accessing Server by HTTP</a>
            <a href="patterns-for-accessing-build-artifacts.md">Patterns For Accessing Build Artifacts</a>
            <a href="https://www.jetbrains.com/help/teamcity/rest/teamcity-rest-api-documentation.html">REST API</a>
        </category>
</seealso>
