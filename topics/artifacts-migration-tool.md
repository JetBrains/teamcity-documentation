[//]: # (title: Artifacts Migration Tool)
[//]: # (auxiliary-id: Artifacts Migration Tool)

TeamCity provides a command-line tool dedicated to automatic migration of [build artifacts](build-artifact.md) from one storage to another. 
Currently, the tool supports migration from a local storage to Amazon S3.

To get the tool, go to __Project Settings | Artifacts Storage__ and use the _Download artifacts migration tool_ link in the top right corner of the page.

## Prerequisites

* The tool has to be installed on the same machine where the TeamCity server is installed.
* The address of the TeamCity server, TeamCity authentication token, and the list of artifact directories must be provided in the configuration file.  
  By default, the tool expects them to be located in `config/application.properties`.
* To use S3 storage as the target, the tool needs AWS credentials on the machine.  
  The AWS credentials can be supplied using [any supported method](https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/setup-credentials.html).

## Command Line Options

The tool supports the following command line arguments:
* `--project` (`-p`) — (required) the [external ID](https://www.jetbrains.com/help/teamcity/identifier.html#External+IDs) of the project that needs to be migrated.
* `--recursive` (`-r`) — (optional) indicates whether the tool should recursively process all subprojects of the selected project.

## Interactive Mode

By default, the tool runs in the interactive mode. During the first run, the tool creates a migration plan listing all artifacts in the source storage. Then, the tool asks you to select the next step:
* Update the migration plan.
* Show the migration plan.
* Copy artifacts from the source storage to the target storage.
> If the source project has a subproject with a custom artifact storage that the tool cannot access,
> it will skip the subproject by default and continue migrating the remaining projects. You can configure the migration to fail in this case using the corresponding [additional property](artifacts-migration-tool.md#additional-properties).

* Revert the migration — in case of incomplete or interrupted migration, the tool will remove the copied artifacts from the target storage.
* Delete artifacts from the source storage.
* Forget the migration plan — discard the current migration plan.

If the tool fails, you can choose to update the migration plan to continue the interrupted migration.

If the tool finds an existing migration plan, it will ask you to select the next step right away.

## Non-Interactive Mode

Alternatively to the interactive mode, migration steps can be specified as additional command line arguments:
* `--create-migration-plan` — creating or updating the migration plan.
* `--show-migration-plan` — showing the migration plan.
* `--start-migration` — copying artifacts from the source storage to the target storage.
* `--revert-migration` — in case of incomplete or interrupted migration, removing copied artifacts from the target storage.
* `--remove-artifacts-in-source` — deleting artifacts from the source storage.
  >*Note:* `--remove-artifacts-in-source` will not remove artifacts that have not been copied to the target storage.
* `--reset-migration-plan` — discard the current migration plan.

If one or more of these steps are provided, the tool goes through them without asking for confirmation or input.

## Required Properties

* `teamcity.storage.migration.host` — the address (protocol, host, and port) of the TeamCity server.
* `teamcity.storage.migration.access.token` — the TeamCity [authentication token](https://www.jetbrains.com/help/teamcity/configuring-your-user-profile.html#Managing+Access+Tokens).
* `teamcity.storage.migration.artifact.directories` — the list of TeamCity artifact directories separated with `;`.

## Additional Properties
{id="additional-properties"}

* `teamcity.storage.migration.processing.threadCount` — the number of threads that the tool should use for processing (by default, **4** threads).
* `teamcity.storage.migration.failWhenCannotAccessStorageSettings` — controls whether the migration should fail if the tool cannot fetch the storage settings from the TeamCity server. It may happen due to the lack of permissions. (by default **false**). 
* `teamcity.storage.migration.s3.threadCount` — the number of threads that the tool should use for uploading data to S3 (by default, **4** threads).
* `teamcity.storage.migration.s3.forceVirtualHostAddressing` — use the virtual hosted style of S3 URL addresses instead of deprecated path style (by default, **true**).
* `teamcity.storage.migration.s3.upload.numberOfRetries` — the number of attempts the tool does when uploading data to S3 if it encounters errors (by default, **5**).
* `teamcity.storage.migration.s3.upload.retryDelayMs` — the initial delay between attempts in milliseconds (by default, **1000**).

### Custom credentials for S3 storage

If a project has multiple S3-compatible storages that need to be migrated, and they require different credentials, 
these credentials can be provided via [Custom AWS profiles](https://docs.aws.amazon.com/sdk-for-php/v3/developer-guide/guide_credentials_profiles.html)

To associate a specific profile in the credentials file with a specific storage, use the following property:

`teamcity.storage.migration.s3.custom.profile.<FEATURE_ID>=<PROFILE_NAME>`, where
`<FEATURE_ID>` should be replaced with the storage ID available in the URL of the storage settings page as the value of the **storageSettingsId** parameter.
`<PROFILE_NAME>` should be replaced with the profile name from the AWS credentials file.

>The target storage needs to be made active in TeamCity.