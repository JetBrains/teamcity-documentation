[//]: # (title: Customizing Notifications)
[//]: # (auxiliary-id: Customizing Notifications)

TeamCity users can [select the events to be notified about](subscribing-to-notifications.md). The default notification messages can be customized globally on a per-server basis. Customizing notifications received via Atom/RSS syndication feeds requires a [special approach](#TeamCity+Notification+Properties) since the feeds use the "pull" model for receiving notifications instead of "push".

Project Administrators with the enabled "_Change user / group notification rules in project_" permission can edit notification rules for users and user groups assigned to their projects.

## Notifications Lifecycle

TeamCity supports a set of events that can generate user notifications (such as build failures, investigation state changes, and so on). On an event occurrence, for each notifier type, TeamCity processes notification settings for all the users to determine which users to notify.

When the set of users is determined, TeamCity fills the notification model (the objects relevant to the notification, such as "build", investigation data, and so on) and evaluates a notification template that corresponds to the notification event.   
The template uses the data model objects to generate the output values (for example, notification message text). The output values are then used by the notifier to send the message. Each notifier supports a specific set of output values.

Note that the template is evaluated once for an event which means that notification properties cannot be adjusted on a per-user basis.

The output values defined by the template are then used by the notifier to send alerts to the selected users.

## Customizing Notifications Templates

### Notification Templates Location

Each of the bundled [notifiers](notifier.md) has a directory in \<[TeamCity Data Directory](teamcity-data-directory.md)\>\/config\/_notifications/, which stores [FreeMarker](http://freemarker.sourceforge.net/) (`.ftl`) templates. There are also [`.dist`](teamcity-data-directory.md#.dist+Template+Configuration+Files) files that store the default templates. Each notification type evaluates a template file with a corresponding name. The template files can be modified while the server is running.

By default, the server checks for changes in the files every 60 seconds, but this can be changed by setting the `teamcity.notification.template.update.interval` [internal property](configuring-teamcity-server-startup-properties.md#TeamCity+internal+properties) to the desired number of seconds.

If an error occurs during the template evaluation, TeamCity logs the error details to `teamcity-notifications.log`. There can be non\-critical errors that result in ignoring part of the template or critical errors that result in the inability to send notification at all. Whenever you make changes to the notification templates ensure the notification can still be sent.

This document doesn't describe the FreeMarker template language, so if you need guidance on the FreeMarker syntax, refer to the corresponding template manual at [http://freemarker.org/docs/dgui.html](http://freemarker.org/docs/dgui.html).

### Supported Output Values

TeamCity notifiers use templates to evaluate output values (global template variables) which are then retrieved by name. The following output values are supported:

__Email Notifier__

* __subject__ – subject of the email message to send
* __body__ – plain text of the email message to send
* __bodyHtml__ – (optional) HTML text of the email message to send. It will be included together with plain text part of the message. However, if it is present in the template, it should be customized as well.
* __headers__ – (optional) raw list of additional headers to include into an email. One header per line. For example:


```XML
<#global headers>
  X-Priority: 1 (Highest)
  Importance: High
</#global>

```

__Jabber__

* __message__ – plain text of the message to send
__IDE Notifications__ and __Windows Tray Notifications__
* __message__ – plain text of the message to send
* __link__ – URL of the TeamCity page that contains detailed information about the event
(i)The Atom/RSS feeds template differs from the others. Refer to the [dedicated section](#TeamCity+Notification+Properties) for details.

### Customization Examples

This section provides Freemaker code snippets that can be used for customization of notifications:

#### Including ERRORS from the log


```XML
<#list build.buildLog.messages[1..] as message><#-- skipping the first message (it is a root node)-->
  <#if message.status == "ERROR" || message.status == "FAILURE" >
    ${message.text}
  </#if>
</#list>

```



The example below shows the snippet included into the `build_failed.ftl` template: the errors will be listed in both the plain text and the html part of the e\-mail:


```XML
<#-- Uses FreeMarker template syntax, template guide can be found at http://freemarker.org/docs/dgui.html -->
 
<#import "common.ftl" as common>
 
<#global subject>[<@common.subjMarker/> FAILED] ${project.name}:${buildType.name} - Build: ${build.buildNumber}</#global>
 
<#global body>TeamCity build: ${project.name}:${buildType.name} - Build: ${build.buildNumber} failed ${var.buildShortStatusDescription}.
Agent: ${agentName}
Build results: ${link.buildResultsLink}
 
${var.buildCompilationErrors}${var.buildFailedTestsErrors}${var.buildChanges}
 
 <#list build.buildLog.messages[1..] as message><#-- skipping the first message (it is a root node)-->
   <#if message.status == "ERROR" || message.status == "FAILURE" >
     ${message.text}
   </#if>
 </#list>
 
<@common.footer/></#global>
 
<#global bodyHtml>
<div>
  <div>
    TeamCity build: <i>${project.name}:${buildType.name} - <a href='${link.buildResultsLink}'>Build: ${build.buildNumber}</a></i> failed
    ${var.buildShortStatusDescription}
  </div>
  <@common.build_agent build/>
  <@common.build_comment build/>
  <br>
  <@common.build_changes var.changesBean/>
  <@common.compilation_errors var.compilationBean/>
  <@common.test_errors var.failedTestsBean/>
 
    <#list build.buildLog.messages[1..] as message><#-- skipping the first message (it is a root node)-->
   <#if message.status == "ERROR" || message.status == "FAILURE" >
     ${message.text}
   </#if>
 </#list>
 
  <@common.footerHtml/>
</div>
</#global>

```


#### Listing build artifacts

There is no default way of listing build artifacts in an email template at the moment. See a [related issue](https://youtrack.jetbrains.com/issue/TW-50431) with a simple plugin allowing you to list artifacts via a relevant API.

__Current workaround__

This assumes direct access to disk from the template and ignores external artifacts storage (like S3) as well as artifacts browsing policies (not showing internal artifacts). Also, this way is likely to become deprecated in the future TeamCity versions.


```XML
<p>Build artifacts:</p>
  <#list build.artifactsDirectory.listFiles() as file>
    <a href="${webLinks.getDownloadArtefactUrl(build.buildTypeExternalId, build.buildId, file.name)}">${file.name}</a> (${file.length()}B)<br/>
  </#list>

```


This will list only the root artifacts and include the `.teamcity` directory, which can be changed by modifications to the code.

#### Listing build parameters


```XML
<#list build.parametersProvider.all?keys as param>
  ${param} - ${build.parametersProvider.all[param]}
  <br>
</#list>

```



This will list the parameters that are passed to the build from the server.

### Default Data Model

For the template evaluation, TeamCity provides the default data model that can be used inside the template. The objects exposed in the model are instances of the corresponding classes from [TeamCity server-side open API](https://plugins.jetbrains.com/docs/teamcity/server-side-object-model.html).   
The set of available objects model differs for different events.You can also add your own objects into the model via a plugin. See [Extending Notification Templates Model](https://plugins.jetbrains.com/docs/teamcity/extending-notification-templates-model.html)) for details.

Here is an example description of the model (the code can be used in IntelliJ IDEA to edit the template with completion). Note that not all entities listed below will be available in any template, for example, all test responsibilities are assigned in the project scope and notifications on test responsibilities can be configured to point to a project where an investigation was assigned (not to a build configuration). 


```XML
<#-- @ftlvariable name="project" type="jetbrains.buildServer.serverSide.SProject" -->
<#-- @ftlvariable name="buildType" type="jetbrains.buildServer.serverSide.SBuildType" -->
<#-- @ftlvariable name="build" type="jetbrains.buildServer.serverSide.SBuild" -->
<#-- @ftlvariable name="agentName" type="java.lang.String" -->
<#-- @ftlvariable name="buildServer" type="jetbrains.buildServer.serverSide.SBuildServer" -->
<#-- @ftlvariable name="webLinks" type="jetbrains.buildServer.serverSide.WebLinks" -->
 
<#-- @ftlvariable name="var.buildFailedTestsErrors" type="java.lang.String" -->
<#-- @ftlvariable name="var.buildShortStatusDescription" type="java.lang.String" -->
<#-- @ftlvariable name="var.buildChanges" type="java.lang.String" -->
<#-- @ftlvariable name="var.buildCompilationErrors" type="java.lang.String" -->
 
<#-- @ftlvariable name="link.editNotificationsLink" type="java.lang.String" -->
<#-- @ftlvariable name="link.buildResultsLink" type="java.lang.String" -->
<#-- @ftlvariable name="link.buildChangesLink" type="java.lang.String" -->
<#-- @ftlvariable name="responsibility" type="jetbrains.buildServer.responsibility.ResponsibilityEntry" -->
<#-- @ftlvariable name="oldResponsibility" type="jetbrains.buildServer.responsibility.ResponsibilityEntry" -->

```

### TeamCity Notification Properties

The following [properties](configuring-teamcity-server-startup-properties.md) can be useful to customize the notifications behavior:

* `teamcity.notification.template.update.interval` – how often the templates are reread by the system (integer, in seconds, default 60)
* `teamcity.notification.includeDebugInfo` – include debugging information into the message in case of template processing errors (boolean, default false)
* `teamcity.notification.maxChangesNum` – max number of changes to list in an email message (integer, default 10)
* `teamcity.notification.maxCompilationDataSize` – max size (in bytes) of compilation error data to include in an email message (integer, default 20480)
* `teamcity.notification.maxFailedTestNum` – max number of failed tests to list in an email message (integer, default 50)
* `teamcity.notification.maxFailedTestStacktraces` – max number of test stacktraces in an email message (integer, default 5)
* `teamcity.notification.maxFailedTestDataSize` – max size (in bytes) of failed test output data to include in a single email message (integer, default 10240)

### Syndication Feed Template

The template uses a configuration approach different from other notification engines.

The default template is stored in the file: `<TeamCity Data Directory>/config/default-feed-item-template.ftl`. This file should never be edited: it is overwritten on every server startup with the default copy. To specify a new template to use, copy the file named `feed-item-template.ftl` into the same directory. This file can be edited and will not be overwritten. It will be used by the engine if present.

The template is a [FreeMarker](http://freemarker.org/docs/dgui.html) template and can be freely edited.

You can use several templates on a single server. The template name can be passed as a [URL parameter](subscribing-to-notifications.md#Additional+Supported+URL+Parameters) of the feed URL.

During feed rendering, the template is evaluated to get the feed content. The resultant content is defined by the global variables defined in the template.

See the default template for an example of available input variables and output variables.

<seealso>
        <category ref="user-guide">
            <a href="subscribing-to-notifications.md">Subscribing to Notifications</a>
        </category>
        <category ref="admin-guide">
            <a href="notifications.md">Notifications</a>
        </category>
</seealso>