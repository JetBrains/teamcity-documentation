[//]: # (title: REST API Reference)
[//]: # (auxiliary-id: REST API Reference)

>This document is no longer supported. Please see the new version of the REST API documentation [here](https://www.jetbrains.com/help/teamcity/rest/teamcity-rest-api-documentation.html).
>
{type="warning"}
 
## Projects and Build Configuration/Templates Lists

<table>

<tr><td width="200"></td><td></td></tr>
<tr><td>List of projects</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/projects
 
```

</td></tr>
<tr>

<td>Project details</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/projects/<projectLocator>
 
```

where `<projectLocator>` can be `id:<internal_project_id>` or `name:<project%20name>`

</td></tr>

<tr><td>List of build configurations</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/buildTypes
 
```

</td></tr>

<tr><td>List of build configurations of a project</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/projects/<projectLocator>/buildTypes
 
```

</td></tr>

<tr><td>

Get projects with subprojects' build configurations data and their order as configured by the specified user on the __Overview__ page

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/projects?locator=selectedByUser:current&fields=count,project(id,parentProjectId,projects(count,project(id),$locator(selectedByUser:current)),buildTypes(count,buildType(id),$locator(selectedByUser:current)))
 
```

</td></tr>

<tr><td>List of templates for a particular project</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/projects/<projectLocator>/templates
 
```

</td></tr>

<tr><td>List of all the templates on the server</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/buildTypes?locator=templateFlag:true
 
```

</td></tr>


</table>

 
### Project Settings

<table>

<tr><td width="200"></td><td></td></tr>
<tr><td>

Get project details

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/projects/<projectLocator>
 
```

</td></tr>

<tr><td>

Delete a project

</td>

<td>

```Shell

DELETE http://teamcity:8111/app/rest/projects/<projectLocator>
 
```

</td></tr>

<tr><td>

Create a new empty project

</td>

<td>

```Shell

POST plain text (name) to http://teamcity:8111/app/rest/projects/
 
```

</td></tr>

<tr><td>

Create (or copy) a project

</td>

<td>

```Shell

POST XML <newProjectDescription name='New Project Name' id='newProjectId' copyAllAssociatedSettings='true'><parentProject locator='id:project1'/><sourceProject locator='id:project2'/></newProjectDescription>
 
```

to

```Shell

http://teamcity:8111/app/rest/projects

```

Also see an [example](#exampleNewProjectCurl).

</td></tr>

<tr><td>

Edit project parameters

</td>

<td>

```Shell

GET/DELETE/PUT http://teamcity:8111/app/rest/projects/<projectLocator>/parameters/<parameter_name>
 
```

Accepts plain text and XML and JSON. Produces XML, JSON, and plain text depending on the "Accept" header.

Also supported requests: `.../parameters/<parameter_name>/name` and `.../parameters/<parameter_name>/value`.

</td></tr>

<tr><td>

Project name/description/archived status

</td>

<td>

```Shell

GET/PUT http://teamcity:8111/app/rest/projects/<projectLocator>/<field_name>
 
```

where `<field_name>` is one of `name`, `description`, `archived`.

Accepts/produces text/plain.

</td></tr>

<tr><td>

Project's parent project

</td>

<td>

```Shell

GET/PUT XML http://teamcity:8111/app/rest/projects/<projectLocator>/parentProject
 
```

</td></tr>


</table>
 
## Project Features
 
Project features (for example, issue trackers, versioned settings, custom charts, shared resources and third-party report tabs) are exposed as entries under the "project" node and via dedicated requests.

<table>

<tr><td width="300"></td><td></td></tr>
<tr><td>

List of project features

</td>

<td>

```Shell

http://teamcity:8111/app/rest/projects/<projectLocator>/projectFeatures
 
```

To filter features, add `?locator=<projectFeaturesLocator>` to the URL, for example to find all issue tracker features of GitHub type, use the locator `type:IssueTracker,property(name:type,value:GithubIssues)`.

</td></tr>

<tr><td>

Create a feature

</td>

<td>

`POST` to `/projects/<projectLocator>/projectFeatures`.

</td></tr>

<tr><td>

Edit features

</td>

<td>

```Shell

GET/DELETE/PUT http://teamcity:8111/app/rest/projects/<projectLocator>/projectFeatures/<featureId>
 
```

</td></tr>


</table>
 
## VCS Roots

<table>

<tr><td width="200"></td><td></td></tr>
<tr><td>

List all VCS roots

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/vcs-roots
 
```

Add the `locator=<vcsRootLocator>` parameter to list only the matched VCS roots.

</td></tr>

<tr><td>

Get details of a VCS root/delete a VCS root

</td>

<td>

```Shell

GET/DELETE http://teamcity:8111/app/rest/vcs-roots/<vcsRootLocator>
 
```

where `<vcsRootLocator>` can be `id:<internal VCS root id>` or other VCS root locator.

</td></tr>

<tr><td>

Create a new VCS root

</td>

<td>

`POST VCS root XML` (similar to the one retrieved by a GET request for VCS root details) to [`http://teamcity:8111/app/rest/vcs-roots`](http://teamcity:8111/app/rest/vcs-roots).

Also supported:
 
* `GET/PUT` [`http://teamcity:8111/app/rest/vcs-roots/<vcsRootLocator>/properties/<property_name>`](http://teamcity:8111/app/rest/vcs-roots/<vcsRootLocator>/properties/<property_name>)
* `GET/PUT` [`http://teamcity:8111/app/rest/vcs-roots/<vcsRootLocator>/<field_name>`](http://teamcity:8111/app/rest/vcs-roots/<vcsRootLocator>/<field_name>)
 
where `<field_name>` is `id`, `name`, `project` (post the project locator to `project` to associate a VCS root with a specific project).

</td></tr>

<tr><td>

List VCS root instances*

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/vcs-root-instances?locator=<vcsRootInstancesLocator>
 
```

</td></tr>



</table>
 
<tip>
 
* A ['VCS root'](vcs-root.md) is the setting configured in the TeamCity UI, a _VCS root instance_ is an internal TeamCity entity which is derived from the _VCS root_ to perform the actual VCS operation. If a VCS root has no %\-references to parameters, a single VCS root corresponds to a single VCS root instance. If a VCS root has %\-reference to a parameter and the reference resolves to a different value when the VCS root is attached to different configurations or when custom builds are run, a single VCS root can generate several VCS root instances.

</tip>
 
__Since TeamCity 10.0__:
 
There are two endpoints dedicated to being used in [commit hooks](configuring-vcs-post-commit-hooks-for-teamcity.md) from the version control repositories: `POST` [`http://teamcity:8111/app/rest/vcs-root-instances/checkingForChangesQueue?locator=<vcsRootInstancesLocator>`](http://teamcity:8111/app/rest/vcs-root-instances/checkingForChangesQueue?locator=<vcsRootInstancesLocator>)
 
It schedules checking for changes for the matched VCS root instances and returns the list of VCS root instances matched (just like `GET` [`http://teamcity:8111/app/rest/vcs-root-instances?locator=<vcsRootInstancesLocator>`](http://teamcity:8111/app/rest/vcs-root-instances?locator=<vcsRootInstancesLocator>)): `POST` [`http://teamcity:8111/app/rest/vcs-root-instances/commitHookNotification?locator=<vcsRootInstancesLocator>`](http://teamcity:8111/app/rest/vcs-root-instances/commitHookNotification?locator=<vcsRootInstancesLocator>)
 
It schedules checking for changes for the matched VCS root instances and returns plain-text human-readable message on the action performed, HTTP response 202 in case of successful operation.
 
Both perform the same action (put the VCS root instances matched by the `<locator>`) to the queue for "checking for changes" process and differ only in responses they produce.
 
Note that since the matched VCS root instances are the same as for `../app/rest/vcs-root-instances?locator=<locator>` request and that means that by default __only the first 100 are matched__ and the rest are ignored. If this limit is reached, consider tweaking the `<locator>` to match fewer instances (recommended) or increase the limit, for example by adding `,count:1000` to the locator.
 
### VCS root instance locator
 
Some supported `<vcsRootInstancesLocator>` from above:
* `type:<VCS root type>` — VCS root instances of the specified version control (for example, `jetbrains.git`, `mercurial`, `svn`).
* `vcsRoot:(<vcsRootLocator>)` — VCS root instances corresponding to the VCS root matched by `<vcsRootLocator>`.
* `buildType:(<buildTypeLocator>)` — VCS root instances attached to the matching build configuration.
* `property:(name:<name>,value:<value>,matchType:<matching>)` — VCS root instances with the property of name `<name>` and value matching condition `<matchType>` (for example, equals, contains) by the value `<value>`.

## Cloud Profiles

TeamCity REST API exposes the same [cloud integration](teamcity-integration-with-cloud-solutions.md) details as those provided in the TeamCity UI.

<table>

<tr><td width="200"></td><td></td></tr>
<tr><td>

List all cloud profiles

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/cloud/profiles
 
```

</td></tr>

<tr><td>

List all cloud images

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/cloud/images
 
```

</td></tr>

<tr><td>

List all cloud instances

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/cloud/instances
 
```

<tip>

To filter the listing results, you can add a [locator](https://www.jetbrains.com/help/teamcity/rest/teamcity-rest-api-documentation.html#Locator) to the request.

</tip>

</td></tr>

<tr><td>

Start a new instance

</td>

<td>

```Shell

POST http://teamcity:8111/app/rest/cloud/instances
 
```

The posted XML/JSON contents are the same as returned by `GET` for one instance.

Example of XML for an instance:

```Shell

<cloudInstance id="profileId:<profileId>,imageId:<imageId>,id:<instanceId>" name="<instanceName>">
	<image id="profileId:<profileId>,id:<imageId>" name="<imageName>"/>
</cloudInstance>

```

</td></tr>

<tr><td>

Stop a running instance

</td>

<td>

```Shell

DELETE http://teamcity:8111/app/rest/cloud/instances/<instanceLocator>
 
```

</td></tr>

</table>

## Build Configuration And Template Settings

<table>

<tr><td width="200"></td><td></td></tr>
<tr><td>

Get build configuration/template details

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/buildTypes/<buildConfigurationLocator>
 
```

Read more on the [build configuration locator](#Build+Configuration+Locator).

Note that there is no transaction, for example support for settings editing in TeamCity, so all the settings modified via REST API are taken into account at once. This can result in half-configured builds triggered and other issues. Make sure you pause a build configuration before changing its settings if this aspect is important for your case.
 
To get aggregated status for several build configurations, see the [Build Status Icon](#Build+Status+Icon) section.

</td></tr>

<tr><td>

Get/set paused build configuration state

</td>

<td>

```Shell

GET/PUT http://teamcity:8111/app/rest/buildTypes/<buildTypeLocator>/paused
 
```

Put `true` or `false` text as text/plain.

</td></tr>

<tr><td>

Build configuration settings

</td>

<td>

```Shell

GET/DELETE/PUT http://teamcity:8111/app/rest/buildTypes/<buildTypeLocator>/settings/<setting_name>
 
```

</td></tr>


<tr><td>

Build configuration parameters

</td>

<td>

```Shell

GET/DELETE/PUT http://teamcity:8111/app/rest/buildTypes/<buildTypeLocator>/parameters/<parameter_name>
 
```

It produces XML, JSON, and plain text depending on the `Accept` header, accepts plain text, XML, and JSON. The `.../parameters/<parameter_name>/name` and `.../parameters/<parameter_name>/value` requests are also supported.

</td></tr>


<tr><td>

Build configuration steps

</td>

<td>

```Shell

GET/DELETE http://teamcity:8111/app/rest/buildTypes/<buildTypeLocator>/steps/<step_id>
 
```

</td></tr>


<tr><td>

Create build configuration step

</td>

<td>

```Shell

POST http://teamcity:8111/app/rest/buildTypes/<buildTypeLocator>/steps
 
```

The XML/JSON posted is the same as retrieved by GET request to `.../steps/<step_id>` except for the secure settings like password: these are not included into responses and should be supplied before POSTing back.

</td></tr>

<tr><td>

Features, triggers, agent requirements, artifact, and snapshot dependencies follow the same pattern as steps (above) with the respective URLs

</td>

<td>

```Shell

http://teamcity:8111/app/rest/buildTypes/<buildTypeLocator>/features/<id>
http://teamcity:8111/app/rest/buildTypes/<buildTypeLocator>/triggers/<id>
http://teamcity:8111/app/rest/buildTypes/<buildTypeLocator>/agent-requirements/
http://teamcity:8111/app/rest/buildTypes/<buildTypeLocator>/artifact-dependencies/<id>
http://teamcity:8111/app/rest/buildTypes/<buildTypeLocator>/snapshot-dependencies/<id>
 
```

__Since TeamCity 10__, it is possible to disable/enable artifact dependencies and agent requirements.

</td></tr>

<tr><td>

Disable/enable an artifact dependency

</td>

<td>

```Shell

PUT http://teamcity:8111/app/rest/buildTypes/<buildTypeLocator>/artifact-dependencies/<id>/disabled
 
```

Put `true` or `false` text as text/plain.

</td></tr>

<tr><td>

Build configuration VCS roots

</td>

<td>

```Shell

GET/DELETE http://teamcity:8111/app/rest/buildTypes/<buildTypeLocator>/vcs-root-entries/<id>
 
```

</td></tr>

<tr><td>

Attach VCS root to a build configuration

</td>

<td>

```Shell

POST http://teamcity:8111/app/rest/buildTypes/<buildTypeLocator>/vcs-root-entries
 
```

The XML/JSON posted is the same as retrieved by GET request to [`http://teamcity:8111/app/rest/buildTypes/<buildTypeLocator>/vcs-root-entries/<id>`](http://teamcity:8111/app/rest/buildTypes/<buildTypeLocator>/vcs-root-entries/<id>) except for the secure settings like password: these are not included into responses and should be supplied before POSTing back.

</td></tr>

<tr><td>

Create a new build configuration with all settings

</td>

<td>

```Shell

POST http://teamcity:8111/app/rest/buildTypes
 
```

The XML/JSON posted is the same as retrieved by GET request. Note that `/app/rest/project/XXX/buildTypes` still uses the previous version notation and accepts another entity.

</td></tr>


<tr><td>

Create a new empty build configuration

</td>

<td>

`POST` plain text (name) to [`http://teamcity:8111/app/rest/projects/<projectLocator>/buildTypes`](http://teamcity:8111/app/rest/projects/<projectLocator>/buildTypes).

</td></tr>

<tr><td>

Copy a build configuration

</td>

<td>

```Shell

POST XML <newBuildTypeDescription name='Conf Name' sourceBuildTypeLocator='id:XXX' copyAllAssociatedSettings='true' shareVCSRoots='false'/>
 
```

to [`http://teamcity:8111/app/rest/projects/<projectLocator>/buildTypes`](http://teamcity:8111/app/rest/projects/<projectLocator>/buildTypes).

</td></tr>


<tr><td>

Read, detach, and attach a build configuration from/to a template

</td>

<td>

__Since TeamCity 2017.2__:

```Shell

GET/DELETE/POST/PUT http://teamcity:8111/app/rest/buildTypes/<buildTypeLocator>/templates
 
```

__Before TeamCity 2017.2__:

```Shell

GET/DELETE/POST/PUT http://teamcity:8111/app/rest/buildTypes/<buildTypeLocator>/template
 
```

Put accepts template locator with the `text/plain` Content-Type).

</td></tr>


<tr><td>

Set build number counter

</td>

<td>

```Shell

curl -v --basic --user <username>:<password> --request PUT http://<teamcity.url>/app/rest/buildTypes/<buildTypeLocator>/settings/buildNumberCounter --data <new number> --header "Content-Type: text/plain"
 
```

</td></tr>


<tr><td>

Set build number format

</td>

<td>

```Shell

curl -v --basic --user <username>:<password> --request PUT http://<teamcity.url>/app/rest/buildTypes/<buildTypeLocator>/settings/buildNumberPattern --data <new format> --header "Content-Type: text/plain"
 
```

</td></tr>

</table>
 
### Build Configuration Locator
 
The most frequently used values for `<buildTypeLocator>` are `id:<buildConfigurationOrTemplate_id>` and `name:<Build%20Configuration%20name>`.
 
__Since TeamCity 2017.2__, the _experimental_ [type](managing-builds.md#Build+Configuration+Types) locator is supported with one of the values: `regular`, `composite`, or `deployment`.
 
Other supported [dimensions](https://www.jetbrains.com/help/teamcity/rest/teamcity-rest-api-documentation.html#Locator) are (these are in _experimental_ state):
* `internalId` — internal ID of the build configuration.
* `project` — `<projectLocator>` to limit the build configurations to those belonging to a single project.
* `affectedProject` — `<projectLocator>` to limit the build configurations under a single project (recursively).
* `template` — `<buildTypeLocator>` of a template to list only build configurations using the template.
* `templateFlag` — boolean value to get only templates or only non-templates.
* `paused` — boolean value to filter paused/not paused build configurations.
 
[//]: # (Internal note. Do not delete. "REST APId269e1377.txt")    
 

## Build Requests

<table>

<tr><td width="200"></td><td></td></tr>
<tr><td>

List builds

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/builds/?locator=<buildLocator>
 
```

</td></tr>

<tr><td>

Get details of a specific build

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/builds/<buildLocator>
 
```

Also supports `DELETE` to delete a build.

</td></tr>

<tr><td>

Get the list of build configurations in a project with the status of the last finished build in each build configuration

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/buildTypes?locator=affectedProject:(id:ProjectId)&fields=buildType(id,name,builds($locator(running:false,canceled:false,count:1),build(number,status,statusText)))
 
```

</td></tr>


</table>
 
### Build Locator
 
Using a [locator](https://www.jetbrains.com/help/teamcity/rest/teamcity-rest-api-documentation.html#Locator) in build-related requests, you can filter the builds to be returned in the build-related requests. It is referred to as _build locator_ in the scope of REST API.
 
For some requests, a default filtering is applied which returns only "normal" builds (finished builds which are not canceled, not failed-to-start, not personal, and on default branch (in branched build configurations)), unless those types of builds are specifically requested via the locator. To turn off this default filter and process all builds, add the `defaultFilter:false` dimension to the build locator. Default filtering varies depending on the specified locator dimensions. For example, when `agent` or `user` dimensions are present, personal, canceled, and failed to start builds are included into the results.
 
Examples of supported build locators:
* `id:<internal build id>` — use [internal build ID](build-results-page.md#Internal+Build+ID) when you need to refer to a specific build.
* `number:<build number>` — to find build by build number, provided build configuration is already specified.
* `<dimension1>:<value1>,<dimension2>:<value2>` — to find builds by multiple criteria.
 
The list of supported build locator dimensions:
* `project:<project locator>` — limit the list to the builds of the specified project (belonging to any build type directly under the project).
* `affectedProject:<project locator>` — limit the list to the builds of the specified project (belonging to any build type directly or indirectly under the project)
* `buildType:(<buildTypeLocator>),defaultFilter:false` — all the builds of the specified build configuration
* `tag:<tag>` — __since TeamCity 10__, get tagged builds. If a list of tags is specified, for example `tag:<tag1>`, `tag:<tag2>`, only the builds containing all the specified tags are returned. The legacy `tags:<tags>` locator is supported for compatibility.
* `status:<SUCCESS/FAILURE/UNKNOWN>` — list builds with the specified status only.
* `user:(<userLocator>)` — limit builds to only those triggered by the user specified.
* `personal:<true/false/any>` — limit builds by the personal flag. By default, personal builds are not included.
* `canceled:<true/false/any>` — limit builds by the canceled flag. By default, canceled builds are not included.
* `failedToStart:<true/false/any>` — limit builds by the failed to start flag. By default, failed to start builds are not included.
* `state:<queued/running/finished>` — limit builds by the specified state.
* `running:<true/false/any>` — limit builds by the running flag. By default, running builds are not included.
* `state:running,hanging:true` — fetch hanging builds (__since TeamCity 10.0__).
* `pinned:<true/false/any>` — limit builds by the pinned flag.
* `branch:<branch locator>` — limit the builds by branch. `<branch locator>` can be the branch name displayed in the UI, or `(name:<name>,default:<true/false/any>,unspecified:<true/false/any>,branched:<true/false/any>)`. By default only builds from the default branch are returned. To retrieve all builds, add the following `locator: branch:default:any`. The whole path will look like this: `/app/rest/builds/?locator=buildType:One_Git,branch:default:any`.
* `revision:<REVISION>` — find builds by revision, for example all builds of the given build configuration with the revision: `/app/rest/builds?locator=revision:(REVISION),buildType:(id:BUILD_TYPE_ID)`. See more information [below](#Revisions).
* `agentName:<name>` — agent name to return only builds ran on the agent with the specified name.
* `sinceBuild:(<buildLocator>)` — limit the list of builds only to those after the one specified
* `sinceDate:<date>` — limit the list of builds only to those started after the date specified. The date should be in the same format as dates returned by REST API (for example, `20130305T170030+0400`).
* `queuedDate/startDate/finishDate:(date:<time-date>,build:<build locator>,condition:<before/after>)` — filter builds based on the time specified by the build locator, for example for the builds finished after November 23, 2017, 20:34:46, GMT\+1 timezone use: `finishDate:(date:20171123T203446%2B0100,condition:after)`.
* `count:<number>` — serve only the specified number of builds.
* `start:<number>` — list the builds from the list starting from the position specified (zero\-based).
* `lookupLimit:<number>` — limit processing to the latest N builds only (the default is 5000). If none of the latest N builds match the other specified criteria of the build locator, 404 response is returned for single build request and empty collection for multiple builds request. See the [related note](https://www.jetbrains.com/help/teamcity/rest/teamcity-rest-api-documentation.html#API+Client+Recommendations).
 
### Queued Builds

<table>

<tr><td width="200"></td><td></td></tr>
<tr><td>

Get a build queue

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/buildQueue
 
```

Supported locators:
* `project:<locator>`
* `buildType:<locator>`

</td></tr>

<tr><td>

Get details of a queued build

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/buildQueue/id:XXX
 
```

For queued builds with snapshot dependencies, the revisions are available in the `revisions` element of the queued build node if a revision is fixed (for regular builds without snapshot dependencies it is not).

</td></tr>

<tr><td>

Get compatible agents for queued builds (useful for builds having "No agents" to run on)

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/buildQueue/id:XXX/compatibleAgents
 
```

</td></tr>

</table>

__Examples__:

<table>

<tr><td width="200"></td><td></td></tr>
<tr><td>

List queued builds per project

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/buildQueue?locator=project:<locator>
 
```

</td></tr>

<tr><td>

List queued builds per build configuration

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/buildQueue?locator=buildType:<locator>
 
```

</td></tr>

</table>
 
### Triggering a Build
 
To start a build, send a `POST` request to [`http://teamcity:8111/app/rest/buildQueue`](http://teamcity:8111/app/rest/buildQueue) with the "build" node (see below) in content — the same node as details of a queued build or finished build. The queued build details will be returned.
 
When the build is started, the request to the queued build (`/app/rest/buildQueue/XXX`) will return running/finished build data. This way, you can monitor the build completeness by querying build details using the `href` attribute of the build details returned on build triggering, until the build has the `state="finished"` attribute.
 
#### Build node example
 
Basic build for a build configuration:
 
```HTML
<build>
    <buildType id="buildConfID"/>
</build>
 
```
 
Build for a branch marked as personal with a fixed agent, comment and a custom parameter:
 
```HTML
<build personal="true" branchName="logicBuildBranch">
    <buildType id="buildConfID"/>
    <agent id="3"/>
    <comment><text>build triggering comment</text></comment>
    <properties>
        <property name="env.myEnv" value="bbb"/>
    </properties>
</build>
 
```
 
Queued build assignment to an agent pool:
 
```HTML
<build>...
  <agent>
    <pool id="N"/>
  </agent>
...
</build>
 
```
 
Build on a change of given revision, forced rebuild of all dependencies and clean sources before the build, moved to the build queue top on triggering. (Note that the change should be already known to TeamCity (displayed in UI for the build configuration, more on the ["lastChanges" element](#Changes)):
 
```HTML
<build>
  <triggeringOptions cleanSources="true" rebuildAllDependencies="true" queueAtTop="true"/>
  <buildType id="buildConfID"/>
   <lastChanges>
    <change locator="version:a286767fc1154b0c2b93d5728dd5bbcdefdfaca,buildType:(id:buildConfID)"/>
  </lastChanges>
</build>
 
```
 
Example command line for the build triggering:
 
```Shell
curl -v -u user:password http://teamcity.server.url:8111/app/rest/buildQueue --request POST --header "Content-Type:application/xml" --data-binary @build.xml
 
```
 
### Build Tags

<table>

<tr><td width="200"></td><td></td></tr>
<tr><td>

Get tags

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/builds/<buildLocator>/tags/
 
```

</td></tr>

<tr><td>

Replace tags

</td>

<td>

```Shell

PUT http://teamcity:8111/app/rest/builds/<buildLocator>/tags/
 
```

Put the same XML or JSON as returned by `GET`.

</td></tr>

<tr><td>

Add tags

</td>

<td>

```Shell

POST http://teamcity:8111/app/rest/builds/<buildLocator>/tags/
 
```

Post the same XML or JSON as returned by `GET` or just a plain-text tag name; `<buildLocator>` here should match a single build only.

Example of XML for two tags:

```Shell

<tags>
    <tag name="<tag1>"/>
    <tag name="<tag2>"/>
</tags>
```

</td></tr>

</table>

 
### Build Pinning

<table>

<tr><td width="200"></td><td></td></tr>
<tr><td>

Get current pin status

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/builds/<buildLocator>/pin/
 
```

</td></tr>

<tr><td>

Pin a build

</td>

<td>

```Shell

PUT http://teamcity:8111/app/rest/builds/<buildLocator>/pin/
 
```

The text in the request data is added as a comment for the action.

</td></tr>

<tr><td>

Unpin a build

</td>

<td>

```Shell

DELETE http://teamcity:8111/app/rest/builds/<buildLocator>/pin/
 
```

The text in the request data is added as a comment for the action; `<buildLocator>` here should match a single build only.

</td></tr>


</table>

 
### Build Canceling/Stopping

<table>

<tr><td width="200"></td><td></td></tr>
<tr><td>

Cancel a queued build

</td>

<td>

POST the `<buildCancelRequest comment='CommentText' readdIntoQueue='false'/>` item to the URL of a queued build. Example:

```Shell

curl -v -u user:password --request POST "http://teamcity:8111/app/rest/buildQueue/<buildLocator>" --data "<buildCancelRequest comment='' readdIntoQueue='false' />" --header "Content-Type: application/xml"
 
```

</td></tr>

<tr><td>

Stop a running build and readd it to the queue

</td>

<td>

POST the `<buildCancelRequest comment='CommentText' readdIntoQueue='true' />` item to the URL of a running build. Example:

```Shell

curl -v -u user:password --request POST "http://teamcity:8111/app/rest/builds/<buildLocator>" --data "<buildCancelRequest comment='' readdIntoQueue='true' />" --header "Content-Type: application/xml"
 
```

Set `readdIntoQueue` to `false` to stop the build without readding it to the queue.

</td></tr>

<tr><td>

See the `canceledInfo` element of the build item

</td>

<td>

Available via `GET http://teamcity:8111/app/rest/builds/<buildLocator>`.

</td></tr>

</table>
 
### Build Artifacts

<table>

<tr><td width="200"></td><td></td></tr>
<tr><td>

Return the content of a build artifact file for a build determined by [`<build_locator>`](#Build+Locator)

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/builds/<build_locator>/artifacts/content/<path>
 
```

`<path>` can be empty for the root of build's artifacts or be a path within the build's artifacts. The path can span into the archive content, for example, `dir/path/archive.zip!/path_within_archive`.
 
* Media-Type: application/octet-stream or a more specific media type (determined from artifact file extension).
* Possible error: 400 if the specified path references a directory.

</td></tr>

<tr><td>

Return information about a build artifact

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/builds/<build_locator>/artifacts/metadata/<path>
 
```

* Media-Type: application/xml or application/json.

</td></tr>

<tr><td>

Return the list of artifact children for directories and archives

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/builds/<build_locator>/artifacts/children/<path>
 
```

* Media-Type: application/xml or application/json.
* Possible error: 400 if the artifact is neither a directory nor an archive.

</td></tr>

<tr><td>

Return an archive containing the list of artifacts under the path specified

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/builds/<build_locator>/artifacts/archived/<path>?locator=pattern:<wildcard>
 
```

The optional `locator` parameter can have file `<wildcard>` to limit the files only to those matching the [wildcard](wildcards.md). `<artifact relative name>` supports referencing files under archives using the `!/` delimiter after the archive name.
 
* Media-Type: application/zip.
* Possible error: 400 if the artifact is neither a directory nor an archive.

</td></tr>

</table>
 
[//]: # (Internal note. Do not delete. "REST APId269e1929.txt")    
 
__Examples__:
 
```Shell
GET http://teamcity:8111/app/rest/builds/id:100/artifacts/children/my-great-tool-0.1.jar!/META-INF
GET http://teamcity:8111/app/rest/builds/buildType:(id:Build_Intallers),status:SUCCESS/artifacts/metadata/my-great-tool-0.1.jar!/META-INF/MANIFEST.MF
GET http://teamcity:8111/app/rest/builds/buildType:(id:Build_Intallers),number:16.7.0.2/artifacts/metadata/my-great-tool-0.1.jar!/lib/commons-logging-1.1.1.jar!/META-INF/MANIFEST.MF
GET http://teamcity:8111/app/rest/builds/buildType:(id:Build_Intallers),tag:release/artifacts/content/my-great-tool-0.1.jar!/lib/commons-logging-1.1.1.jar!/META-INF/MANIFEST.MF
    
```
 
#### Authentication
 
If you download artifacts from within a TeamCity build, consider [using values](https://www.jetbrains.com/help/teamcity/rest/teamcity-rest-api-documentation.html#REST+Authentication) of `teamcity.auth.userId/teamcity.auth.password` system properties as credentials for the download artifacts request: this way TeamCity will have a way to record that one build used artifacts of another and will display it on the build's __Dependencies__ tab.
 
### Other Build Requests
 
#### Changes
 
__`<changes>`__ is meant to represent changes the same way as displayed in the build's [Changes](working-with-build-results.md#Changes) in TeamCity UI. In the most cases these are the commits between the current and previous build. The `<changes>` tag is not included into the build by default, it has the `href` attribute only. If you execute the request specified in `href`, you'll get the required changes.

<table>

<tr><td width="400"></td><td></td></tr>
<tr><td>

Get the list of all changes included into the build

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/changes?locator=build:(id:<buildId>)
 
```

</td></tr>
<tr>

<td>

Get details of an individual change

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/changes/id:changeId
 
```

</td></tr>

<tr><td>

Get information about a changed file action

</td>

<td>

The file's node lists changed files. The information about the changed file action is reported via the `changeType` attribute for the files listed as `added`, `edited`, `removed`, `copied`, or `unchanged`.

</td></tr>

<tr><td>

Filter all changes by a locator

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/changes?locator=<changeLocator>
 
```

<note>

The change ID is the change's internal ID, not the revision. The ID can be seen in the change node listed by the REST API or in the URL of the change details (as `modId`).

</note>

</td></tr>

<tr><td>

Get all changes for a project

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/changes?locator=project:projectId
 
```

</td></tr>

<tr><td>

Get all the changes in a build configuration since a particular change identified by its ID

</td>

<td>

```Shell

http://teamcity:8111/app/rest/changes?locator=buildType:(id:buildConfigurationId),sinceChange:(id:changeId)
 
```

</td></tr>

<tr><td>

Get pending changes for a build configuration

</td>

<td>

```Shell

http://teamcity:8111/app/rest/changes?locator=buildType:(id:BUILD_CONF_ID),pending:true
 
```

</td></tr>


</table>

The __`<lastChanges>`__ tag contains information about the last commit included into the build. When triggering a build, its nested `<change>` element can contain the `locator` field that specifies what change to use for the build triggering.
 
#### Revisions
 
The `<revisions>` tag the same as revisions table on the build's [Changes](working-with-build-results.md#Changes) tab in TeamCity UI: it lists the revisions of all VCS repositories associated with this build that will be checked out by the build on the agent. A revision might or might not correspond to a change known to TeamCity. For example, for a newly created build configuration and a VCS root, a revision will have no corresponding change.

<table>

<tr><td width="200"></td><td></td></tr>
<tr><td>

Get all builds with the specified revision

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/builds?locator=revision(version:XXXX)
 
```

</td></tr>

</table>
 
__Since TeamCity 10__, `<versionedSettingsRevision>` is added to represent revision of the [versioned settings](storing-project-settings-in-version-control.md) of the build.

#### Snapshot dependencies

<table>

<tr><td width="200"></td><td></td></tr>
<tr><td>

Find all the snapshot dependency builds in the build chain upstream for the build with the ID `XXXX`.

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/builds?locator=snapshotDependency:(to:(id:XXXX),includeInitial:true),defaultFilter:false
 
```

</td></tr>

<tr><td>

Find all the snapshot-dependent builds in all build chains downstream for the build with the ID `XXXX`.

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/builds?locator=snapshotDependency:(from:(id:XXXX),includeInitial:true),defaultFilter:false
 
```

</td></tr>

</table>

#### Artifact dependencies
 
__Since TeamCity 10.0.3__, there is an experimental ability to:

<table>

<tr><td width="200"></td><td></td></tr>
<tr><td>

Get all the builds which downloaded artifacts from the build with the given ID (Delivered artifacts in the TeamCity web UI)

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/builds?locator=artifactDependency:(from:(id:<build ID>),recursive:false)
 
```

</td></tr>

<tr><td>

Get all the builds whose artifacts were downloaded by the build with the given ID (Downloaded artifacts in the TeamCity web UI)

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/builds?locator=artifactDependency:(to:(id:<build ID>),recursive:false)
 
```

</td></tr>

</table>

#### VCS Labels


<table>

<tr><td width="200"></td><td></td></tr>
<tr><td>

Get [VCS labels](vcs-labeling.md) of a build

</td>

<td>

by adding the `vcsLabels` field:

```Shell

GET http://teamcity:8111/app/rest/builds?locator=<buildLocator>&fields=build(id,vcsLabels:$long)

```

or via a separate request:

```Shell

GET http://teamcity:8111/app/rest/builds/<buildLocator>/vcsLabels?fields=status,text

```

</td></tr>

<tr><td>

Add a VCS label to a build

</td>

<td>

```Shell

POST http://teamcity:8111/app/rest/builds/<buildLocator>/vcsLabels?locator=<vcsRootInstanceLocator>&fields=build(id,vcsLabels)

```

where `locator` is optional and specifies where to put labels; if not specified, a label will be added to all instances of a VCS root.

</td></tr>

</table>
 
 
#### Build Parameters

<table>

<tr><td width="200"></td><td></td></tr>
<tr><td>

Get the [parameters](predefined-build-parameters.md) of a build

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/builds/id:<build_id>/resulting-properties
 
```

</td></tr>

</table>

#### Build Fields

<table>

<tr><td width="200"></td><td></td></tr>
<tr><td>

Get a single build's field

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/builds/<buildLocator>/<field_name>
 
```

This accepts/produces text/plain where `<field_name>` is one of `number`, `status`, `id`, `branchName`, and other build's bean attributes.

</td></tr>

</table>

#### Statistics

<table>

<tr><td width="200"></td><td></td></tr>
<tr><td>

Get statistics for a single build

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/builds/<buildLocator>/statistics/
 
```

Only standard/bundled statistic values are listed. See also [Custom Charts](custom-chart.md).

</td></tr>

<tr><td>

Get single build statistics value

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/builds/<buildLocator>/statistics/<value_name>
 
```

</td></tr>

<tr><td>

Get statistics for a list of builds

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/builds?locator=BUILDS_LOCATOR&fields=build(id,number,status,buildType(id,name,projectName),statistics(property(name,value)))
 
```

</td></tr>

</table>
 
#### Build Log
 
Downloading build logs via a REST request is not supported, but there is a way to download the log files described [here](build-log.md#Viewing+Build+Log).

## Tests and Build Problems

<table>

<tr><td width="200"></td><td></td></tr>
<tr><td>

List build problems

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/problemOccurrences?locator=build:(BUILD_LOCATOR)
 
```

</td></tr>

<tr><td>

List tests

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/testOccurrences?locator=<locator dimension>:<value>
 
```

</td></tr>

</table>
 
__Supported locators__:
* `build:(<build locator>)` — test run in the build.
* `build:(<build locator>),muted:true` — failed tests which were muted in the build.
* `currentlyFailing:true,affectedProject:<project_locator>` — tests currently failing under the project specified (recursively).
* `currentlyMuted:true,affectedProject:<project_locator>` — tests currently muted under the project specified (recursively). See also the project's __Muted Problems__ tab.
* `includePersonal:true` - include tests from [personal builds](personal-build.md).
 
__Examples__:

<table>

<tr><td width="200"></td><td></td></tr>
<tr><td>

List all build's tests

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/testOccurrences?locator=build:<buildLocator>
 
```

</td></tr>

<tr><td>

Get individual test history

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/testOccurrences?locator=test:<testLocator>
 
```

</td></tr>

<tr><td>

List build's tests which were muted when the build ran

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/testOccurrences?locator=build:(id:XXX),muted:true
 
```

</td></tr>

<tr><td>

List currently muted tests (muted since the failure)

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/testOccurrences?locator=build:(id:XXX),currentlyMuted:true
 
```

</td></tr>

</table>
 
__Supported test locators__:
* `id:<internal test id>` available as a part of the URL on the __Test History__ page
* `name:<full test name>`
 
There is an experimental support for exposing single test invocations / runs:

<table>

<tr><td width="200"></td><td></td></tr>
<tr><td>

Get invocations of a test

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/testOccurrences?locator=build:(id:XXX),test:(id:XXX)&fields=$long,testOccurrence($short,invocations($long))
 
```

</td></tr>

<tr><td>

List all test runs with all the invocations flattened

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/testOccurrences?locator=build:(id:XXX),test:(id:XXX),expandInvocations:true
 
```

</td></tr>


</table>
 
 
### Muted Tests and Build Problems
 
__Since TeamCity 2017.2__

<table>

<tr><td width="200"></td><td></td></tr>
<tr><td>

List all muted tests and build problems

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/mutes
 
```

</td></tr>

<tr><td>

Unmute a test or build problems

</td>

<td>

```Shell

DELETE http://teamcity:81111/app/rest/mutes/id:XXXX
 
```

</td></tr>

<tr><td>

Mute a test or build problems

</td>

<td>

```Shell

POST http://teamcity:8111/app/rest/mutes
 
```

Use the same XML or JSON as returned by `GET`.

Example of XML for muting a test:

```Shell

<mute>
    <scope><project id="<projectID>"/></scope>
    <target><tests><test name="<testName>"/></tests></target>
    <resolution type="whenFixed"/>
</mute>
```

</td></tr>

</table>

## Investigations

<table>

<tr><td width="200"></td><td></td></tr>
<tr><td>

List investigations in the Root project and its subprojects

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/investigations
 
```

__Supported locators:__
* `test: (id:TEST_NAME_ID)`
* `test: (name:FULL_TEST_NAME)`
* `assignee: (<user_locator>)`
* `buildType:(id:XXXX)`

</td></tr>

<tr><td>

Get investigations for a specific test

</td>

<td>

```Shell

http://teamcity:8111/app/rest/investigations?locator=test:(id:TEST_NAME_ID)
http://teamcity:8111/app/rest/investigations?locator=test:(name:FULL_TEST_NAME)
 
```

</td></tr>

<tr><td>

Get investigations assigned to a user

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/investigations?locator=assignee:(<user locator>)
 
```

</td></tr>

<tr><td>

Get investigations for a build configuration

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/investigations?locator=buildType:(id:XXXX)
 
```

</td></tr>

<tr><td>

To assign/replace investigations

</td>

<td>

A single investigation:

`POST/PUT` to [`http://teamcity:8111/app/rest/investigations`](http://teamcity:8111/app/rest/investigations)

Experimental _support for multiple investigations_: `POST/PUT` to [`http://teamcity:8111/app/rest/investigations/multiple`](http://teamcity:8111/app/rest/investigations/multiple) (accepts a list of investigations).

Use the same XML or JSON as returned by `GET`.

Example of XML for assigning an investigation:

```Shell

<investigation state="TAKEN">
    <assignee username="<username>"/>
    <scope><buildType id="<buildTypeID>"/></scope>
    <target anyProblem="true"/>
    <resolution type="whenFixed"/>
</investigation>
```

</td></tr>

</table>
 
## Agents
 
<table>

<tr><td width="200"></td><td></td></tr>
<tr><td>

List agents (only authorized agents are included by default)

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/agents
 
```

</td></tr>
<tr>

<td>

List all connected authorized agents

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/agents?locator=connected:true,authorized:true
 
```

</td></tr>

<tr><td>

List all authorized agents

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/agents?locator=authorized:true
 
```

</td></tr>

<tr><td>

List all enabled authorized agents

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/agents?locator=enabled:true,authorized:true
 
```

</td></tr>

<tr><td>

List all agents (including unauthorized)

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/agents?locator=authorized:any
 
```

The request uses default filtering (depending on the specified locator dimensions, others can have default implied value). To disable this filtering, add `,defaultFilter:false` to the locator.

</td></tr>

<tr><td>

Enable/disable an agent

</td>

<td>

```Shell

PUT http://teamcity:8111/app/rest/agents/<agentLocator>/enabled
 
```

Put `true` or `false` text as text/plain. See an [example](http://devnet.jetbrains.net/message/5462246#5462246).

</td></tr>

<tr><td>

Authorize/unauthorize an agent

</td>

<td>

```Shell

PUT http://teamcity:8111/app/rest/agents/<agentLocator>/authorized
 
```

Put `true` or `false` text as text/plain.

</td></tr>

<tr><td>

Get/put an agent's single field

</td>

<td>

```Shell

GET/PUT http://teamcity:8111/app/rest/agents/<agentLocator>/<field_name>
 
```

</td></tr>

<tr><td>

Delete a build agent

</td>

<td>

```Shell

DELETE http://teamcity:8111/app/rest/agents/<agentLocator>
 
```

</td></tr>

<tr><td>

Add a comment when enabling/disabling and authorizing/unauthorizing an agent

</td>

<td>

Agent enabled/authorized data is exposed in the `enabledInfo` and `authorizedInfo` nodes:
 
```HTML
<agent id="1" name="agentName" typeId="1" connected="true" enabled="true" authorized="true" uptodate="true" ip="..........." href="/app/rest/agents/id:1">
 <enabledInfo status="true">
  <comment>
   <user username="userName" id="1" href="/app/rest/users/id:1"/>
   <timestamp>20160406T175040+0300</timestamp>
   <text>newcomment</text>
  </comment>
 </enabledInfo>
 <authorizedInfo status="true">
  <comment>
   <user username="userName" id="1" href="/app/rest/users/id:1"/>
   <timestamp>20160406T183033+0300</timestamp>
  </comment>
 </authorizedInfo>
....
</agent>
 
```

`GET` and `PUT` requests are supported for the following URLs: [`http://teamcity:8111/app/rest/agents/<agentLocator>/enabledInfo`](http://teamcity:8111/app/rest/agents/<agentLocator>/enabledInfo) and [`http://teamcity:8111/app/rest/agents/<agentLocator>/authorized`](http://teamcity:8111/app/rest/agents/<agentLocator>/authorized).

On `PUT` only status and comment/text sub-items are taken into account. An example of disabling an agent with a comment:
 
```Shell
curl -v -u user:password --request PUT "http://teamcity:8111/app/rest/agents/id:1/enabledInfo" --data "<enabledInfo status='false'><comment><text>commentText</text></comment></enabledInfo>" --header "Content-Type:application/xml
 
```

</td></tr>

</table>
 
### Agent Pools

<table>

<tr><td width="200"></td><td></td></tr>
<tr><td>

List all agent pools

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/agentPools
 
```

</td></tr>

<tr><td>

Get/modify/remove an agent pool with `ID`

</td>

<td>

```Shell

GET/PUT/DELETE http://teamcity:8111/app/rest/agentPools/id:ID
 
```

</td></tr>

<tr><td>

Add an agent pool

</td>

<td>

`POST` the `agentPool name='PoolName'` element to [`http://teamcity:8111/app/rest/agentPools`](http://teamcity:8111/app/rest/agentPools).

</td></tr>

<tr><td>

Move an agent to the pool from the previous pool

</td>

<td>

`POST <agent id='YYY'/>` to the pool's agents [`http://teamcity.url/app/rest/agentPools/id:XXX/agents`](http://teamcity.url/app/rest/agentPools/id:XXX/agents).

</td></tr>


</table>
 
Example:
```Shell
curl -v -u user:password [http://teamcity.url/app/rest/agentPools/id:XXX/agents](http://teamcity.url/app/rest/agentPools/id:XXX/agents) --request POST --header "Content-Type:application/xml" --data "<agent id='1'/>"
 
```
 
### Assigning Projects to Agent Pools

<table>

<tr><td width="200"></td><td></td></tr>
<tr><td>

Add a project to a pool

</td>

<td>

`POST` the `<project>` node to [`http://teamcity.url/app/rest/agentPools/id:XXX/projects`](http://teamcity.url/app/rest/agentPools/id:XXX/projects).

</td></tr>

<tr><td>

Delete a project from a pool

</td>

<td>

```Shell

DELETE http://teamcity.url/app/rest/agentPools/id:XXX/projects/id:YYY
 
```

</td></tr>


</table>

## Users

<table>

<tr><td width="200"></td><td></td></tr>
<tr><td>

List of users

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/users
 
```

</td></tr>

<tr><td>

Get specific user details

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/users/<userLocator>
 
```

</td></tr>

<tr><td>

Create a user

</td>

<td>

```Shell

POST http://teamcity:8111/app/rest/users
 
```

</td></tr>

<tr><td>

Update/remove specific user

</td>

<td>

```Shell

PUT/DELETE http://teamcity:8111/app/rest/users/<userLocator>
 
```

</td></tr>

</table>
 
For the `POST` and `PUT` requests for a user, post data in the form retrieved by the corresponding GET request. Only the following attributes/elements are supported: `name`, `username`, `email`, `password`, `roles`, `groups`, `properties`.

<table>

<tr><td width="200"></td><td></td></tr>
<tr><td>

Work with user roles

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/users/<userLocator>/roles
 
```

`<userLocator>` can be of a form:
* `id:<internal user id>` — to reference the user by internal ID
* `username:<user's username>` — to reference the user by username/login name

</td></tr>

<tr><td>

User's single field

</td>

<td>

```Shell

GET/PUT http://teamcity:8111/app/rest/users/<userLocator>/<field_name>
 
```

</td></tr>


<tr><td>

User's single property

</td>

<td>

```Shell

GET/DELETE/PUT http://teamcity:8111/app/rest/users/<userLocator>/properties/<property_name>
 
```

</td></tr>

</table>
 
## User Groups

<table>

<tr><td width="200"></td><td></td></tr>
<tr><td>

List of groups

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/userGroups
 
```

</td></tr>

<tr><td>

List of users within a group

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/userGroups/key:Group_Key
 
```

</td></tr>

<tr><td>

Create a group

</td>

<td>

```Shell

POST http://teamcity:8111/app/rest/userGroups
 
```

</td></tr>

<tr><td>

Delete a group

</td>

<td>

```Shell

DELETE http://teamcity:8111/app/rest/userGroups/key:Group_Key
 
```

</td></tr>


</table>

## User Access Tokens

<table>

<tr><td width="200"></td><td></td></tr>
<tr><td>

List of access tokens

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/users/<userLocator>/tokens
 
```

</td></tr>

<tr><td>

Create an access token

</td>

<td>

```Shell

POST http://teamcity:8111/app/rest/users/<userLocator>/tokens/<tokenName>
 
```

</td></tr>

<tr><td>

Delete an access token

</td>

<td>

```Shell

DELETE http://teamcity:8111/app/rest/users/<userLocator>/tokens/<tokenName>
 
```

</td></tr>

</table>

## Audit Records

<table>

<tr><td width="200"></td><td></td></tr>
<tr><td>

To access the records of user actions, also available on the __[Audit](tracking-user-actions.md)__ page in TeamCity

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/audit
 
```

</td></tr>

</table>

You can filter records by user, system action, build type, and so on (use `GET` [`http://teamcity:8111/app/rest/audit?locator=$help`](http://teamcity:8111/app/rest/audit?locator=$help) to see all available filters).

## Data Backup
[//]: # (AltHead: Other)

<table>

<tr><td width="200"></td><td></td></tr>
<tr><td>

Start backup

</td>

<td>

```Shell

POST http://teamcity:8111/app/rest/server/backup?includeConfigs=true&includeDatabase=true&includeBuildLogs=true&fileName=
 
```

where `<fileName>` is the prefix of the file to save backup to. The file will be created in the default backup directory (see [more](creating-backup-from-teamcity-web-ui.md)).

</td></tr>

<tr><td>

Get current backup status (idle/running)

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/server/backup
 
```

</td></tr>

</table>

 
## Typed Parameters Specification

<table>

<tr><td width="200"></td><td></td></tr>
<tr><td>

List [typed parameters](typed-parameters.md)

</td>

<td>

For a project:

```Shell

GET http://teamcity:8111/app/rest/projects/<locator>/parameters
 
```

For a build configuration:

```Shell

GET http://teamcity:8111/app/rest/buildTypes/<locator>/parameters
 
```

The information returned is: `parameters count`, `property name`, `value`, and `type`. The `rawValue` of the `type` element is the [parameter specification](typed-parameters.md#Adding+Parameter+Specification) as defined in the UI.

</td></tr>

<tr><td>

Get details of a specific parameter

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/buildTypes/<locator>/parameters/<name>
 
```

Accepts/returns plain-text, XML, JSON. Supply the [relevant Content-Type header](https://www.jetbrains.com/help/teamcity/rest/teamcity-rest-api-documentation.html#Response+Formats) to the request.

</td></tr>

<tr><td>

Create a new parameter

</td>

<td>

`POST` the same XML or JSON or just plain-text as returned by `GET` to [`http://teamcity:8111/app/rest/buildTypes/<locator>/parameters/`](http://teamcity:8111/app/rest/buildTypes/<locator>/parameters/). Note that [secure parameters](typed-parameters.md), for example `type=password`, are listed, but the values not included into response, so the result should be amended before POSTing back.

Example of XML for setting a property:

```Shell

<property name="<parameterName>" value="">
    <type rawValue="password"/>
</property>
```

</td></tr>

</table>

__Since TeamCity 9.1__, partial updates of a parameter are possible (currently in an experimental state):

* Name: `PUT` the same XML or JSON as returned by `GET` to [`http://teamcity:8111/app/rest/buildTypes/<locator>/parameters/NAME`](http://teamcity:8111/app/rest/buildTypes/<locator>/parameters/NAME)
* Type: `GET/PUT` accepting XML and JSON as returned by `GET` to the URL [`http://teamcity:8111/app/rest/buildTypes/<locator>/parameters/NAME/type`](http://teamcity:8111/app/rest/buildTypes/<locator>/parameters/NAME/type)
* Type's rawValue: `GET/PUT` accepting plain text [`http://teamcity:8111/app/rest/buildTypes/<locator>/parameters/NAME/type/rawValue`](http://teamcity:8111/app/rest/buildTypes/<locator>/parameters/NAME/type/rawValue)
 
## Build Status Icon
 
Icon that represents a build status:

<table>

<tr><td width="200"></td><td></td></tr>
<tr><td>

An `.svg` icon (__recommended__)

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/builds/<buildLocator>/statusIcon.svg
 
```

</td></tr>

<tr><td>

A `.png` icon

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/builds/<buildLocator>/statusIcon
 
```

</td></tr>

<tr><td>

Icon that represents build status for several builds (__since TeamCity 10.0__)

</td>

<td>

`GET` request and `strob` build locator dimension.

</td></tr>

</table>
 
 
__Example request__:
 
For project with the `PROJECT_ID` ID:
```Shell
GET http://teamcity:8111/app/rest/builds/aggregated/strob:(buildType:(project:(id:PROJECT_ID)))/statusIcon.svg
 
```
 
For all active branches in a build configuration with the `BUILD_CONF_ID` ID:
```Shell
GET http://teamcity:8111/app/rest/builds/aggregated/strob:(branch:(buildType:(id:BUILD_CONF_ID),policy:active_history_and_active_vcs_branches),locator:(buildType:(id:BUILD_CONF_ID)))/statusIcon.svg
 
```
 
For request `/app/rest/builds/aggregated/<build locator>` the status is calculated by list of the builds: `app/rest/builds?locator=<build locator>`. This allows embedding a build status icon into any HTML page with a simple `img` tag:
 
For build configuration with the `BUILD_CONF_ID` ID:
 
Status of the last build:
 
```HTML
<img src="http://teamcity:8111/app/rest/builds/buildType:(id:BUILD_CONF_ID)/statusIcon.svg"/>
 
```
 
Status of the last build tagged with the `myTag` tag:
```HTML
<img src="http://teamcity:8111/app/rest/builds/buildType:(id:BUILD_CONF_ID),tag:myTag/statusIcon.svg"/>
 
```
 
![BuildSuccessfulStatus.svg](BuildSuccessfulStatus.svg)
 
All other [build locators](#Build+Locator) are supported.
 
For example, you can use the following markdown markup to add the build status for GitHub repository for the build configuration with ID `TeamCityPluginsByJetBrains_TeamcityGoogleTagManagerPlugin_Build` and server [`https://teamcity.jetbrains.com`](https://teamcity.jetbrains.com) with guest authentication enabled:
 
```HTML
[![Build status](https://teamcity.jetbrains.com/guestAuth/app/rest/builds/buildType:(id:TeamCityPluginsByJetBrains_TeamcityGoogleTagManagerPlugin_Build)/statusIcon.svg](https://teamcity.jetbrains.com/viewType.html?buildTypeId=TeamCityPluginsByJetBrains_TeamcityGoogleTagManagerPlugin_Build)
 
```
 
If the returned image contains "no permission to get data" text (![no-permission-to-get-data.svg](no-permission-to-get-data.svg)), ensure that one of the following is true:
* The server has the [guest user access](guest-user.md) enabled, and the guest user has permissions to access the build configuration referenced, or
* The build configuration referenced has the "enable status widget" [option](configuring-general-settings.md#Enable+Status+Widget) __ON__.
* You are logged in to the TeamCity server in the same browser and you have permissions to view the build configuration referenced. Note that this will not help for embedded images in GitHub pages as GitHub retrieves the images from the server side.
 
## TeamCity Licensing Information Requests
 
__Since TeamCity 10__:

<table>

<tr><td width="200"></td><td></td></tr>
<tr><td>

Licensing information

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/server/licensingData
 
```

</td></tr>

<tr><td>

List of license keys

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/server/licensingData/licenseKeys
 
```

</td></tr>

<tr><td>

License key details

</td>

<td>

```Shell

GET http://teamcity:8111/app/rest/server/licensingData/licenseKeys/<license_key>
 
```

</td></tr>

<tr><td>

Add license key(s)

</td>

<td>

`POST` text/plain newline-delimited keys to [`http://teamcity:8111/app/rest/server/licensingData/licenseKeys`](http://teamcity:8111/app/rest/server/licensingData/licenseKeys).

</td></tr>

<tr><td>

Delete a license key

</td>

<td>

```Shell

DELETE http://teamcity:8111/app/rest/server/licensingData/licenseKeys/<license_key>
 
```

</td></tr>

</table>
 
## CCTray
 
CCTray-compatible XML is available via [`http://teamcity:8111/app/rest/cctray/projects.xml`](http://teamcity:8111/app/rest/cctray/projects.xml).
 
Without authentication (only build configurations available for guest user): [`http://teamcity:8111/guestAuth/app/rest/cctray/projects.xml`](http://teamcity:8111/guestAuth/app/rest/cctray/projects.xml).
 
The CCTray-format XML does not include paused build configurations by default. The URL accepts the `locator` parameter instead with standard [build configuration locator](#Build+Configuration+Locator).
 
## Request Examples

[//]: # (AltHead: Request Sending Tool)

You can use [curl](https://en.wikipedia.org/wiki/CURL) command line tool to interact with the TeamCity REST API.
 
Example command:
```Shell
curl -v --basic --user USERNAME:PASSWORD --request POST "http://teamcity:8111/app/rest/users/" --data @data.xml --header "Content-Type: application/xml"
 
```
where `USERNAME`, `PASSWORD`, `teamcity:8111` are to be substituted with real values, and `data.xml` file contains the data to send to the server.

<anchor name="exampleNewProjectCurl"/>
 
### Creating a new project
 
Using curl tool:
 
```Shell
curl -v -u USER:PASSWORD http://teamcity:8111/app/rest/projects --header "Content-Type: application/xml" --data-binary
"<newProjectDescription name='New Project Name' id='newProjectId'><parentProject locator='id:project1'/></newProjectDescription>"
 
```
 
### Making user a system administrator
 
1. Get [super user](super-user.md) token.
2. Issue the request.
3. Get [curl](https://en.wikipedia.org/wiki/CURL) command line tool and use a command line:
 
```Shell
curl -v -u :SUPERUSER_TOKEN --request PUT http://teamcity:8111/app/rest/users/username:USERNAME/roles/SYSTEM_ADMIN/g/
 
```
where `SUPERUSER_TOKEN` is the super user token unique for each server start; `teamcity:8111` — the TeamCity server URL; `USERNAME` — the username of the user to be made the system administrator.
 
<tip>
 
More examples (for TeamCity 8.0) are available in this [external posting](https://gist.github.com/carlspring/6762356).
</tip>
 
[//]: # (Internal note. Do not delete. "REST APId269e3252.txt")  
