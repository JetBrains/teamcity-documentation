[//]: # (title: Artifacts Migration Tool)
[//]: # (auxiliary-id: Artifacts Migration Tool)

The **artifacts migration tool** is a command-line tool that allows you to transfer [build artifacts](build-artifact.md) from one storage to another.

<img src="dk-baMigrationTool-overview.png" width="708" alt="TeamCity Artifacts Migration Tool"/>

Currently, the tool accepts only Amazon S3 as a migration target.


## Download the Artifacts Migration Tool

You can download this tool from the **Project Settings | Artifacts Storage** page.

<img src="dk-downloadAMTool.png" width="708" alt="Download artifacts migration tool"/>

Note that you need this tool to be on the same machine where the TeamCity server is installed.

## Configuration File

Before you can run the artifacts migration tool, you need to specify the following settings in the `config/application.properties` file.

* `teamcity.storage.migration.host` — the TeamCity server's address (protocol, host, and port).

* `teamcity.storage.migration.artifact.directories` — the absolute path to the local TeamCity artifact storage. If your TeamCity server uses multiple directories as artifacts storages, use a semicolon character (;) as a separator.

* `teamcity.storage.migration.access.token` — the TeamCity [authentication token](configuring-your-user-profile.md#Managing+Access+Tokens). Tokens must have permissions sufficient to access artifact storages. Navigate to **Your Profile | Access Tokens** to create a new token.

The server URL and default artifact storage paths can be found on the **Administration | Global Settings** page.

<img src="dk-artifactstoragepaths.png" width="708" alt="Obtain server URL and storage paths"/>

The snippet below demonstrates the contents of a sample `application.properties` file.

```Plain Text
teamcity.storage.migration.access.token=aBcEfgHIjkLMnoPQRsTUVwxyz
teamcity.storage.migration.artifact.directories=C:\\ProgramData\\JetBrains\\TeamCity\\system\\artifacts
teamcity.storage.migration.host=http://localhost:8111
```

## AWS-Specific Settings

To migrate artifacts to or from [Amazon S3 buckets](storing-build-artifacts-in-amazon-s3.md), the artifacts migration tool needs to use AWS credentials stored on the server machine. See this documentation article for more information: [Set up AWS Credentials and Region for Development](https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/setup-credentials.html).

If a project has multiple S3-compatible storages that need to be migrated and require different credentials, use [Custom AWS profiles](https://docs.aws.amazon.com/sdk-for-php/v3/developer-guide/guide_credentials_profiles.html) to provide these credentials. To associate a specific profile in the credentials file with the particular storage, add the following property to the tool's `config/application.properties` file:

```Plain Text
teamcity.storage.migration.s3.custom.profile.<FEATURE_ID>=<PROFILE_NAME>
```

* `<FEATURE_ID>` is the storage ID from [the storage settings](storing-build-artifacts-in-amazon-s3.md#Create+and+Set+Up+a+New+AWS+S3+Storage) page.
* `<PROFILE_NAME>` is the profile name from the AWS credentials file.


## Target Storage Settings

The migration tool copies or moves artifacts to the currently active storage. Before you run the migration process, go to the **Project Settings | Artifacts Storage** page of a project whose artifacts you want to migrate, and activate the target storage.

<img src="dk-makeArtifactsStorageActive.png" width="708" alt="Activate target project storage"/>

## Run the Migration Tool

Open the terminal at the _&lt;artifacts_migration_tool&gt;/bin_ folder and run the "migrate" (Linux, macOS) or "migrate.bat" (Windows) file with the `--project` (`-p`) parameter. This parameter accepts [external project IDs](identifier.md#External+IDs) as values.

<tabs>

<tab title="Linux / macOS">

```Shell
./migrate --project="NetFrameworkProject3"
``` 
</tab>

<tab title="Windows">

```Shell
migrate.bat --project="NetFrameworkProject3"
``` 
</tab>

</tabs>




You can add the `--recursive` (`-r`) parameter to specify whether the tool should recursively process all subprojects of the selected project.

> If the migration tool cannot access a subproject's storage, it will skip this project. Add the `failWhenCannotAccessStorageSettings=true` property to the tool's [configuration file](#Configuration+File) to force the tool to fail in these scenarios.

If you need to migrate artifacts from cloud storage rather than a local directory, specify the additional `--source` (`-s`) parameter and pass a storage ID as a value.

<img src="dk-getCloudStorageID.png" width="708" alt="Obtain cloud storage ID"/>


<tabs>

<tab title="Linux / macOS">

```Shell
./migrate -p "NetFrameworkProject3" --source="PROJECT_EXT_2"
``` 
</tab>

<tab title="Windows">

```Shell
migrate.bat -p "NetFrameworkProject3" --source="PROJECT_EXT_2"
``` 
</tab>

</tabs>



> Currently, the tool accepts only Amazon S3 bucket IDs as the "source" parameter values.

The first time you run the migration tool in this mode, it detects artifacts that should be copied, saves the migration plan, and asks you for the next step.

* Update the migration plan.
* Show the migration plan.
* Copy artifacts from the source storage to the target storage.
* Revert the migration. This option removes copied artifacts from the target storage. You can select this option in case of an incomplete or interrupted migration.
* Delete artifacts from the source storage.
* Forget the migration plan. Use this option to discard the saved migration plan if you specified incorrect migration parameters.

To migrate artifacts in one go (without the tool asking you for confirmation or input), specify the required migration steps by adding the following commands.

* `--create-migration-plan` — create or update the migration plan.
* `--show-migration-plan` — show the migration plan.
* `--start-migration` — copy artifacts from the source storage to the target storage.
* `--revert-migration` — remove copied artifacts from the target storage in case of an incomplete or interrupted migration.
* `--remove-artifacts-in-source` — delete copied artifacts from the source storage. Artifacts that were not copied will not be removed.
* `--reset-migration-plan` — discard the current migration plan.

For example, the following command moves artifacts from the given Amazon S3 storage to a currently active storage.

<tabs>

<tab title="Linux / macOS">

```Shell
./migrate --project="SampleProject" --source="PROJECT_EXT_2" --start-migration --remove-artifacts-in-source
``` 
</tab>

<tab title="Windows">

```Shell
migrate.bat --project="SampleProject" --source="PROJECT_EXT_2" --start-migration --remove-artifacts-in-source
``` 
</tab>

</tabs>


> The migration process must conclude with either the `--remove-artifacts-in-source` or `--revert-migration` command. Simply copying artifacts to new storage using the `--start-migration` command alone is considered incomplete, and may lead to issues like duplicated artifacts in the TeamCity UI or migration plan stuck in the same state.
> 
{style="warning"}



## Additional Configuration Properties

You can add the following properties to the [configuration file](#Configuration+File).

* `teamcity.storage.migration.processing.threadCount` — the number of threads that the tool should use for processing. The default value is **4**.
* `teamcity.storage.migration.failWhenCannotAccessStorageSettings` — controls whether the migration should fail if the tool cannot fetch the storage settings from the TeamCity server. It may happen due to the lack of permissions. The default value is **false**.
* `teamcity.storage.migration.s3.threadCount` — the number of threads that the tool should use to upload data to S3. The default value is **4**.
* `teamcity.storage.migration.s3.forceVirtualHostAddressing` — specifies whether the tool should use the virtual hosted style of S3 URL addresses instead of the deprecated path style. The default value is **true**.
* `teamcity.storage.migration.s3.upload.numberOfRetries` — the number of attempts the tool makes when uploading data to S3 if it encounters errors. The default value is **5**.
* `teamcity.storage.migration.s3.upload.retryDelayMs` — the initial delay between attempts in milliseconds. The default value is **1000**.