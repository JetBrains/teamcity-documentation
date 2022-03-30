[//]: # (title: Artifacts Migration Tool)
[//]: # (auxiliary-id: Artifacts Migration Tool)

TeamCity provides a command-line tool dedicated to automatic migration of [build artifacts](build-artifact.md) from one storage to another. Currently, it supports migration from a local storage to Amazon S3.

The tool is a work in progress. The download link will be available on its release.

## Prerequisites

* The service has to be installed on the same machine where the TeamCity server is installed.
* The address of the TeamCity server, TeamCity authentication token, and list of artifact directories must be provided in the configuration file.  
  By default, the service expects it to be located in `config/application.properties`.
* The ID of the source storage must be provided to the service. It can be found in the URL of the storage settings page in TeamCity, in the `storageSettingsId` parameter.
* If either a source or target storage is an S3 storage, the tool expects to find AWS credentials on the machine.  
  AWS credentials can be supplied using [any supported method](https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/setup-credentials.html).

## Command Line Options

The service supports the following command line arguments:
* `--source` (`-s`) — (required) the settings ID of the source storage (the `storageSettingsId` parameter in the URL of the artifact storage settings page in TeamCity).
* `--project` (`-p`) — (required) the [external ID](https://www.jetbrains.com/help/teamcity/identifier.html#External+IDs) of the project that needs to be migrated.
* `--recursive` (`-r`) — (optional) indicates whether the service should recursively process all subprojects of the selected project.

## Interactive Mode

By default, the service runs in the interactive mode. During the first run, the service creates a migration plan listing all artifacts in the source storage. Then, the service asks to select the next step:
* Update the migration plan.
* Show the migration plan.
* Copy artifacts from the source storage to the target storage.
* Revert the migration — in case of incomplete or interrupted migration, remove the copied artifacts from the target storage.
* Remove artifacts from the source storage.
* Reset the migration plan — discard the current migration plan.

If the service fails, you can choose to "update the migration plan" to continue the interrupted migration.

If the service finds an existing migration plan, it will request to select the next step right away.

## Non-Interactive Mode

Alternatively to the interactive mode, migration steps can be specified as additional command line arguments:
* `--create-migration-plan` — creating or updating the migration plan.
* `--show-migration-plan` — showing the migration plan.
* `--start-migration` — copying artifacts from the source storage to the target storage.
* `--revert-migration` — in case of incomplete or interrupted migration, removing copied artifacts from the target storage.
* `--remove-artifacts-in-source` — deleting artifacts from the source storage.
  >*Note:* `--remove-artifacts-in-source` will not remove artifacts that have not been copied to the target storage.
* `--reset-migration-plan` — discard the current migration plan.

If one or more of these steps are provided, the service goes through them without asking for confirmation or input.

## Required Properties

* `teamcity.storage.migration.host` — the address (protocol, host, and port) of the TeamCity server.
* `teamcity.storage.migration.access.token` — the TeamCity [authentication token](https://www.jetbrains.com/help/teamcity/configuring-your-user-profile.html#Managing+Access+Tokens).
* `teamcity.storage.migration.artifact.directories` — the list of TeamCity artifact directories separated with `;`.

## Additional Properties

* `teamcity.storage.migration.processing.threadCount` — the number of threads that the service should use for processing (by default, **4** threads).
* `teamcity.storage.migration.s3.threadCount` — the number of threads that the service should use for uploading data to S3 (by default, **4** threads).
* `teamcity.storage.migration.s3.forceVirtualHostAddressing` — use the virtual hosted style of S3 URL addresses instead of deprecated path style (by default, **true**).
* `teamcity.storage.migration.s3.upload.numberOfRetries` — the number of attempts the service does when uploading data to S3 if it encounters errors (by default, **5**).
* `teamcity.storage.migration.s3.upload.retryDelayMs` — the initial delay between attempts in milliseconds (by default, **1000**).

## Build

This project uses Gradle as the build system.