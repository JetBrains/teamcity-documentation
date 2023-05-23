[//]: # (title: Upgrade Notes)
[//]: # (auxiliary-id: Upgrade Notes)



## Changes from 2022.10.3 to 2023.05

### Planned deprecation of Java 8 in TeamCity Server

TeamCity 2023.05 supports Java versions 8, 11 and 17, but __Java 8 support will be discontinued in one the next TeamCity versions__. If you use a non-bundled version of Java 8, we highly recommend that you migrate your server to Java 11 or 17.


### Bundled Tools Updates
{id="bundled-tools-updates-2023-5"}



* The bundled Kotlin compiler (used in [TeamCity DSL](kotlin-dsl.md)) and Dokka (the documentation engine for Kotlin) were updated to version 1.7.10.
* The bundled Tomcat was updated to version 9.0.75.
* Amazon Corretto Java bundled with TeamCity Windows installer and TeamCity Docker images was updated to version 17.0.7.7.1. In addition, our Docker images no longer utilize Java 8. If you relied on this version, use the `:2022.10.3` tag to pull the previous Docker image version.

### REST API Update

The Web Application Description Language (WADL) generator is now removed. See the [initial announcement](#Upcoming+REST+API+Updates) for more information.

### Multinode Setup Updates

* The "Processing user requests to modify data" responsibility was renamed to "Handling UI actions and load balancing user requests".
* The `[data_directory](teamcity-data-directory.md)/config/nodes-config.xml` file listed only "MAIN_NODE" responsibility for main nodes. In version 2023.05, this configuration file lists all responsibilities enabled on a main node.

See the What's New page for more responsibility-related changes: [](what-s-new-in-teamcity.md#Multinode+Setup+Enhancements).

### Podman Support

Due to the implementation of [](what-s-new-in-teamcity.md#Podman+Support), the following changes were made:

* The "Docker Wrapper" extension was renamed to [](container-wrapper.md).
* The "Docker Info" tab on the [](build-results-page.md) was renamed to "Container Info".
* Adding the [](container-wrapper.md) build feature to a build configuration no longer applies the `docker.server.version exists` agent requirement. Instead, TeamCity now defines the `docker.server.osType exists` condition. This property is synchronized with `podman.osType` so that agents with Podman installed instead of Docker are compatible with this new requirement.

### Miscellaneous Updates

* Users with the "Project Developer" [role](managing-roles-and-permissions.md) can now download and view the `.teamcity/settings/buildSettings.xml` [hidden artifact](build-artifact.md#Hidden+Artifacts). Previously, this action required the "Edit project" permission that is enabled for "Project Administrator" and higher roles.
* Agent pages no longer display the **Open SSM Terminal** action link. This functionality was deprecated in favor of more generic **Open Terminal** button. See [](what-s-new-in-teamcity.md#Interactive+Agent+Terminals) for more details.




## Changes from 2022.10.2 to 2022.10.3

### Bundled Tools Updates
{id="bundled-tools-updates-2022-10-3"}

* The bundled Git was updated to version 2.40 in both Server and Agent Docker images.
* The bundled Tomcat was updated to version 9.0.71.
* The Perforce Helix Core client (p4) was updated to version 2022.2-2407422 in Agent and Server Docker images.

### Known Issues
{id="known-issues-2022-10-3"}

Using the "Default Credentials Provider" as a principal AWS connection may cause the "Default Credentials Provider Chain: The security token included in the request is expired" error when the session expires. This issue is already fixed in the latest AWS Core plugin that will be bundled with TeamCity in the upcoming 2022.10.4 bugfix version. To manually install this plugin version, download the corresponding attachment from the [TW-80253](https://youtrack.jetbrains.com/issue/TW-80253/) ticket.

## Changes from 2022.10.1 to 2022.10.2

### Bundled Tools Updates
{id="bundled-tools-updates-2022-10-2"}

* The bundled Git was updated to version 2.39.1 in both Server and Agent Docker images.
* The Perforce Helix Core client (p4) was updated to version 2022.2-2369846 in Agent Docker images.
* The bundled Apache Tomcat was updated to version 8.5.84.


### Upcoming REST API Updates

The Web Application Description Language (WADL) generator will be removed in version 2023.05 since we now utilize Swagger to generate documentation [REST API](teamcity-rest-api.md) and client code.

If you rely on this generator tool, [contact us](feedback.md) to share your business requirements.

## Changes from 2022.10 to 2022.10.1

### AWS Connection

#### Default Provider Chain credentials type is disabled by default

The **[Default Provider Chain](configuring-connections.md#AmazonWebServices)** credentials type in AWS connections is now disabled by default to prevent [associated security risks](upgrade-notes.md#known-issues-202210).
To enable this option, set [the internal property](server-startup-properties.md#TeamCity+Internal+Properties) `teamcity.internal.aws.connection.defaultCredentialsProviderEnabled=true` (The default value is `false`.)
No server restart is required after the property is set.

#### Custom STS endpoint is disabled by default

Only [the global or regional AWS STS endpoints](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_enable-regions.html)
can be used as STS Endpoints in [the AWS connection configuration](configuring-connections.md#AmazonWebServices).
To use a custom endpoint for Amazon alternatives like [MinIO](https://min.io/), [contact the TeamCity support team](https://teamcity-support.jetbrains.com/hc/en-us/requests/new?).



<include src="upgrade-notes-older-versions.md" include-id="older-upgrade-notes" />