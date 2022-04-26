[//]: # (title: Configuring Artifacts Storage)
[//]: # (auxiliary-id: Configuring Artifacts Storage)

The __Project Settings | Artifacts Storage__ tab displays artifact storages configured in this project as well as the storages inherited from parents. 

By default, the built-in TeamCity artifacts storage is displayed and marked as active. You can activate a different storage using the corresponding link.

## Built-in Artifacts Storage

TeamCity stores [artifacts](build-artifact.md) produced by builds on the file system accessible by the TeamCity server. The default artifacts directory location is `<[TeamCity Data Directory](teamcity-data-directory.md)>/system/artifacts` but it can be [redefined](teamcity-configuration-and-maintenance.md).

## External Artifacts Storage 

TeamCity provides a pluggable API to enable external storage for TeamCity build artifacts. Support for different storages can be implemented as an external plugin to TeamCity: the details are provided in the [external storage implementation guide](https://plugins.jetbrains.com/docs/teamcity/external-storage-implementation-guide.html).

Note that when an external storage for artifacts is enabled, the TeamCity [internal](build-artifact.md#Hidden+Artifacts) artifacts (including build logs) will still be published to the TeamCity server and stored in the TeamCity Data Directory in the built-in artifacts' storage.

The same applies to the metadata about artifacts mappings, which will be published to the [artifacts directory](teamcity-configuration-and-maintenance.md) of the TeamCity Data Directory. When restoring from a backup, make sure they are restored for the external artifact plugin to work properly.

### Amazon S3 Support
<anchor name="Amazon+S3+Support"/>

TeamCity can store build artifacts in an Amazon S3 bucket. Read more details in [this article](storing-build-artifacts-in-amazon-s3.md).

### Azure Artifact Storage

[Azure Artifact Storage](https://plugins.jetbrains.com/plugin/9617-azure-artifact-storage) is an experimental plugin by JetBrains which allows replacing the TeamCity built-in artifacts' storage by Azure Blob storage. 

### Google Cloud Artifact Storage

Google Cloud Artifact Storage is implemented as a [plugin](https://plugins.jetbrains.com/plugin/9634-google-artifact-storage) by JetBrains.



<chunk include-id="artifactMigrationToS3">

## Migrating Artifacts To a Different Storage
<anchor name="migratingArtifactsToS3"/>

TeamCity provides [a command-line tool](artifacts-migration-tool.md) dedicated to automatic migration of build artifacts from one storage to another.
Currently, it supports migration from the built-in storage to [Amazon S3](configuring-artifacts-storage.md#Amazon+S3+Support). We're working on supporting other cloud storage options as well.

</chunk>


<chunk id="S3multipartUpload">

## Multipart Upload
<anchor name="multipartUpload"/>


To optimize the [upload of large files](https://aws.amazon.com/premiumsupport/knowledge-center/s3-upload-large-files/) to [Amazon S3](storing-build-artifacts-in-amazon-s3.md), you can initiate [multipart upload](https://docs.aws.amazon.com/AmazonS3/latest/userguide/mpuoverview.html) instead of regular upload. To do this, set the multipart upload threshold in the _Connection Settings_ block. The minimum allowed value is `5MB`. Supported suffixes: `KB`, `MB`, `GB`, `TB`. If you leave this field empty, multipart upload will be initiated automatically for all files larger than 8 MB (`8MB` is the default value).

Additionally, you can configure the maximum allowed size of each uploaded file part. The minimum value is `5MB`. If left empty, TeamCity will use `8MB` as the default value.

>We recommend that you configure a [bucket lifecycle policy](https://docs.aws.amazon.com/AmazonS3/latest/userguide/mpu-abort-incomplete-mpu-lifecycle-config.html) to prevent incomplete multipart uploads.

</chunk>


