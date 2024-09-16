[//]: # (title: Amazon S3 and S3-compatible Storages)
[//]: # (auxiliary-id: Storing Build Artifacts in Amazon S3)

TeamCity comes bundled with the Amazon S3 Artifact Storage plugin which allows storing build artifacts in Amazon S3 buckets, as well as S3-compatible buckets such as [MinIO](https://min.io/product/s3-compatibility), [Backblaze B2](https://www.backblaze.com/cloud-storage), and others. S3-compatible storages can be hosted in both AWS and non-AWS environments.


## Create and Set Up a New AWS S3 Storage



1. <snippet id="settings_art_storage_page">Navigate to the <b>Administration | &lt;Your_Project&gt;</b> page and switch to the <b>Artifacts Storage</b> tab.

   * Open settings of a &lt;Root project&gt; if you want your new storage to be available for all TeamCity projects.
   * Edit one specific project if your new storage should be available only for this project and its sub-projects.

    </snippet>
   
2. <snippet id="settings_add_new_storage">The built-in TeamCity artifacts storage is displayed by default and marked as active. Click <b>Add new storage</b> button to create a new storage.</snippet>

3. <snippet id="settings_name_id">Specify the custom storage name and, if needed, its internally used <a href="identifier.md">ID</a>.</snippet>

4. Set the **Type** field to "AWS S3".

5. Choose an existing [AWS Connection](configuring-connections.md#AmazonWebServices) that TeamCity should use to access your Amazon resources. If no suitable AWS connection exists, click the "+" icon to add one.
    
    > You cannot use AWS Connections that belong to a parent project and have their **Available for sub-projects** settings disabled.
    > 
    > However, if a parent project has an S3 storage configured using a non-shared connection, this storage is still available to child projects.
    > 
    {style="tip"}

    <anchor name="permissions"/>

    A user whose credentials the selected AWS Connection uses (or an IAM Role it assumes) to access the S3 buckets should have the following permissions:

   * `ListAllMyBuckets`
   * `GetBucketLocation`
   * `GetObject`
   * `ListBucket`
   * `PutObject`
   * `DeleteObject`
   * `GetAccelerateConfiguration` (if [Transfer Acceleration](#TransferAcceleration) is enabled)
    
    > <anchor name="transferToConnection"/>In previous TeamCity versions, the **Artifacts Storage** dialog allowed you to explicitly specify connection settings: access key credentials, user IAM role, and whether TeamCity should look for credentials in the default AWS locations (the **Default provider chain** setting).
    > 
    > Starting with version 2023.11, these settings are exclusive to [AWS Connections](configuring-connections.md#AmazonWebServices). If you're migrating from an older version of TeamCity and your existing AWS S3 storages used any of these settings, click the **Convert to AWS Connection** link. This action transfers AWS-related settings to a new AWS Connection, and selects this new connection as the source connection of your storage.
    >
    {style="tip"}
    
6. TeamCity uses the selected AWS Connection to retrieve the list of available S3 buckets. Open the **Bucket** drop-down menu to choose a specific item from the list.

<anchor name="pathPrefix"/>

7. <snippet id="settings_path_prefix">(Optional) Specify the <a href="https://docs.aws.amazon.com/AmazonS3/latest/user-guide/using-folders.html">path prefix</a> if you want to use the same S3 bucket for all TeamCity projects and configure prefix-based permissions.</snippet>



8. Amazon S3 buckets support two options to speed up file uploads and downloads:

   * [AWS CloudFront](https://aws.amazon.com/cloudfront/) — a content delivery network (CDN) that allows TeamCity to transfer arifacts using low-latency CloudFront servers nearby.
   * <anchor name="TransferAcceleration"/>[Transfer Acceleration](https://aws.amazon.com/s3/transfer-acceleration/) — a bucket-level feature designed to optimize transfer speeds from across the world into centralized S3 buckets. It enables fast, easy, and secure transfers of files over long distances between your client and an S3 bucket.
    
    If your bucket is configured to use Transfer Acceleration or CloudFront, choose the corresponding option under the **Transfer speed-up** section. Otherwise, if you wish TeamCity to transfer files in the regular mode, choose the **None** type.

    <img src="dk-s3-speedUpMode.png" width="706" alt="S3 Transfer SpeedUp Mode"/>
    
    > Choosing the **AWS CloudFront** option requires setting up additional fields. See this section for more information: [](#CloudFrontSettings).
    > 
    {style="note"}

    > Using Transfer Acceleration applies the following restrictions:
    >
    > * The [virtual host addressing](#forceVirtualHostAddressing) is always on. Path-style requests are not supported.
    > * Transfer Acceleration must be enabled on your S3 bucket.
    > * Buckets [cannot have dots (.) in their names](https://docs.aws.amazon.com/AmazonS3/latest/userguide/bucketnamingrules.html).
   > * User credentials or IAM role the AWS Connection uses to access your bucket must include the `GetAccelerateConfiguration` permission.
    >
    {style="note"}

<anchor name="multipartUpload"/>

9. <snippet id="settings_mulipart_upload">To optimize the <a href="https://aws.amazon.com/premiumsupport/knowledge-center/s3-upload-large-files/">upload of large files</a> to the storage, you can enable the <a href="https://docs.aws.amazon.com/AmazonS3/latest/userguide/mpuoverview.html">multipart upload</a>. To do this, tick the <b>Customize threshold and part size</b> setting and set the multipart upload threshold. The minimum allowed value is <code>5MB</code>. Supported suffixes: <code>KB</code>, <code>MB</code>, <code>GB</code>, <code>TB</code>. If you leave this field empty, multipart upload will be initiated automatically for all files larger than 8 MB (<code>8MB</code> is the default value).

    <img src="dk-s3-multipart.png" width="706" alt="Multipart upload"/>

    Additionally, you can configure the maximum allowed size of each uploaded file part. The minimum value is <code>5MB</code>. If left empty, TeamCity will use <code>8MB</code> as the default value.
    </snippet>
    
    > We recommend that you configure a [bucket lifecycle policy](https://docs.aws.amazon.com/AmazonS3/latest/userguide/mpu-abort-incomplete-mpu-lifecycle-config.html) to prevent incomplete multipart uploads.
    >
    {style="tip"}

<anchor name="forceVirtualHostAddressing"/>

10. <snippet id="settings_virtual_host">Uncheck the <b>Force virtual host addressing</b> option to turn off the <a href="https://docs.aws.amazon.com/AmazonS3/latest/userguide/VirtualHosting.html">corresponding feature</a> (enabled by default). Currently, both hosted-style and path-style requests are supported by TeamCity. Note that <a href="https://docs.aws.amazon.com/AmazonS3/latest/dev/VirtualHosting.html#path-style-access">Amazon stopped supporting path-style access</a> for new buckets since September 2020.</snippet>

    > This setting cannot be disabled if your storage uses [Transfer Acceleration](#TransferAcceleration).
    >
    {style="note"}

11. <snippet id="settings_verify_integrity">Tick <b>Verify file integrity after upload</b> to allow TeamCity to perform an additional <a href="https://docs.aws.amazon.com/AmazonS3/latest/userguide/checking-object-integrity.html#checking-object-integrity-md5">check-up on uploaded files</a>. If the integrity verification fails, TeamCity writes a corresponding message to the build log.</snippet>

12. <snippet id="settings_save_and_exit">Click <b>Save</b> to save your new storage and return to the list of available storages.</snippet>

<snippet id="make_storage_active">

> If your build agent is deployed behind a proxy, you must also configure the proxy settings on the build agent.
> See [](configuring-proxy-server.md#Use+Proxy+for+Outgoing+Build+Agent+Connections).
>
{style="note"}

When viewing a list of storages available for a project, click <b>Make Active</b> to start using the corresponding storage for all new builds of this project. The <b>has N usages</b> link allows you to view which builds used this storage to upload their artifacts.


<img src="dk-s3-makeActive.png" width="706" alt="Make storage active"/>

</snippet>



## Create and Set Up a New S3-Compatible Storage


1. <include from="storing-build-artifacts-in-amazon-s3.md" element-id="settings_art_storage_page"/>

2. <include from="storing-build-artifacts-in-amazon-s3.md" element-id="settings_add_new_storage"/>

3. <include from="storing-build-artifacts-in-amazon-s3.md" element-id="settings_name_id"/>

4. Set the **Type** field to "Custom S3".

5. Specify the **Access key ID** and **Secret access key** values. See documentation for your S3-compatible storage vendor for the information on how to issue access keys.

6. Specify the storage endpoint TeamCity should use to access your bucket.

7. <include from="storing-build-artifacts-in-amazon-s3.md" element-id="settings_path_prefix"/>

8. <include from="storing-build-artifacts-in-amazon-s3.md" element-id="settings_mulipart_upload"/>

9. <include from="storing-build-artifacts-in-amazon-s3.md" element-id="settings_virtual_host"/>

10. <include from="storing-build-artifacts-in-amazon-s3.md" element-id="settings_verify_integrity"/>

11. <include from="storing-build-artifacts-in-amazon-s3.md" element-id="settings_save_and_exit"/>


<include from="storing-build-artifacts-in-amazon-s3.md" element-id="make_storage_active"/>


## S3 Storage Classes

[Amazon S3 Storage Classes](https://aws.amazon.com/s3/storage-classes/) allow you to fine-tune your storage based on its desired performance, as well as availability and resilience of its data.

There are two ways to enable the required storage class:

* **On TeamCity side**. When uploading artifacts to an S3 bucket, TeamCity adds the `x-amz-storage-class` header to its `PUT` requests. The header value depends on the corresponding storage setting in TeamCity (for example, `x-amz-storage-class: INTELLIGENT_TIERING`). This mode does not require any additional setup on the AWS side.

    Although this approach is not currently supported, we hope to implement this functionality in our future release cycles. Upvote and comment on this YouTrack ticket to support the feature and share your feedback: [TW-79992](https://youtrack.jetbrains.com/issue/TW-79992).

* **On AWS side**. In this mode, TeamCity uploads artifacts in a regular manner and the required storage class is applied by a pre-configured lifecycle rule after the artifacts were uploaded. To set up this rule, do the following:

    1. Open the required S3 storage and switch to the **Management** tab.
    2. Click **Create lifecycle rule**.
    3. Check **Move current versions of objects between storage classes** under the **Lifecycle rule actions** section.
    4. Choose the required storage class and the delay between the upload and transition dates. Set the **Days after object creation** to "0" to transition your artifacts as soon as TeamCity uploads them.
        > TeamCity does not support archive storage classes since their files are not immediately available and require additional unpack/warmup actions before they can be fetched.
        >
        {style="tip"}
    5. Enable additional rules for stored artifacts. For example, you can check **Expire current versions of objects** to label previously uploaded artifacts as expired, and **Permanently delete noncurrent versions of objects** to periodically clean your storage.
    6. Specify the rule scope to choose whether it should apply to the entire storage or only those artifacts that match the required filter.
    7. Review your rule at the bottom of the page. It may look like the following:
        <img src="dk-s3-lifecycle-rule.png" width="706" alt="S3 lifecycle rule"/>
    8. Click **Create rule** to save your lifecycle rule.

See the following AWS help article for more information: [Using Amazon S3 storage classes](https://docs.aws.amazon.com/AmazonS3/latest/userguide/storage-class-intro.html).



## Transferring Artifacts via CloudFront
{id="CloudFrontSettings" auxiliary-id="CloudFrontSettings"}

[Amazon CloudFront](https://aws.amazon.com/cloudfront/) is a content delivery network that offers low latency and high transfer speeds. Enabling its support for an S3 storage will allow TeamCity to transfer artifacts through the closest CloudFront server. If your S3 bucket is located in a different region than your TeamCity infrastructure, this could significantly speed up the artifacts' upload/download and reduce expenses.

>If you use [EC2 build agents](setting-up-teamcity-for-amazon-ec2.md) located in the same region as the target S3 bucket, these agents will communicate with the bucket directly, omitting CloudFront.

### Prerequisites

TeamCity can [set up CloudFront integration for you](#Automatic+CloudFront+Setup), or you can set up all the settings [manually](#Manual+CloudFront+Setup).

The CloudFront integration requires configuring:
* an [OAI](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-restricting-access-to-s3.html) user
* two [CloudFront distributions](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/distribution-working-with.html), one for uploading and downloading artifacts.
* a [trusted key group](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-trusted-signers.html#choosing-key-groups-or-AWS-accounts)
* an SSH-2 RSA [key pair](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-trusted-signers.html#private-content-creating-cloudfront-key-pairs) (public key + private key) in PEM format

### CloudFront Settings

When you switch the storage type to **AWS CloudFront**, four new settings appear.

* Use the **Download distribution** and **Upload distribution** drop-down menus to choose [manually created](#Manual+CloudFront+Setup) distributions.

* The **Public key** field and **Upload private key...** button allow you to specify [corresponding keys](#Manual+CloudFront+Setup).

Alternatively, you can click the <img src="magic-wand.png" alt="Switch to the Sakura UI" height="20" width="20"/> icon to let TeamCity [configure all four settings automatically](#Automatic+CloudFront+Setup).

For Cloudfront settings to work properly, TeamCity needs the following permissions:

* cloudfront:ListDistributions
* cloudfront:ListKeyGroups
* cloudfront:ListPublicKeys

#### Automatic CloudFront Setup

TeamCity can configure the settings automatically. This involves:
* Generating a key pair and uploading a public key to CloudFront.
* Creating a new key group in CloudFront.
* Creating two new distributions with:
  * the __Use all edge locations__ price class.
  * a new [Origin Access Identity](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-restricting-access-to-s3.html) that can access the current bucket.
  * the default behaviour defining
      * allowed HTTP methods for uploading artifacts: `GET`, `HEAD`, `OPTIONS`, `PUT`, `POST`, `PATCH`, `DELETE`, for downloading: `GET`, `HEAD`, `OPTIONS`;
      * a new [key group](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-trusted-signers.html#private-content-adding-trusted-signers) with the viewer access;
      * a custom [cache policy](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/using-managed-cache-policies.html) that allows passing all query strings.
* Adding a new policy to an S3 bucket to allow the new distributions access to it. See the [policy example](#S3+Policy+Example).

Automatic setup requires giving Teamcity additional permissions:

* cloudfront:CreateDistribution
* cloudfront:CreateKeyGroup
* cloudfront:CreatePublicKey
* cloudfront:CreateOriginRequestPolicy
* cloudfront:CreateCloudFrontOriginAccessIdentity
* cloudfront:CreateCachePolicy
* cloudfront:DeleteKeyGroup
* cloudfront:DeletePublicKey
* cloudfront:ListCloudFrontOriginAccessIdentities
* cloudfront:ListCachePolicies
* cloudfront:ListOriginRequestPolicies
* cloudfront:GetDistribution
* cloudfront:GetPublicKey
* s3:GetBucketPolicy
* s3:PutBucketPolicy

Example policy providing all necessary permissions:

```Plain Text
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "1",
            "Effect": "Allow",
            "Action": [
                "cloudfront:CreatePublicKey",
                "cloudfront:CreateOriginRequestPolicy",
                "cloudfront:ListCloudFrontOriginAccessIdentities",
                "cloudfront:DeleteKeyGroup",
                "cloudfront:GetPublicKey",
                "cloudfront:ListCachePolicies",
                "cloudfront:CreateDistribution",
                "cloudfront:ListOriginRequestPolicies",
                "cloudfront:DeletePublicKey",
                "cloudfront:CreateCloudFrontOriginAccessIdentity",
                "cloudfront:CreateKeyGroup",
                "cloudfront:CreateCachePolicy",
                "cloudfront:GetDistribution",
                "cloudfront:ListPublicKeys",
                "s3:ListAllMyBuckets",
                "cloudfront:ListKeyGroups",
                "cloudfront:ListDistributions"
            ],
            "Resource": "*"
        },
        {
            "Sid": "2",
            "Effect": "Allow",
            "Action": [
                "s3:PutBucketPolicy",
                "s3:GetBucketPolicy"
            ],
            "Resource": "arn:aws:s3:::<YOUR_BUCKET_NAME>"
        }
    ]
}

```

#### Manual CloudFront Setup

For security reasons, we recommend configuring two separate distributions for uploading and downloading artifacts. For each distribution:
1. Generate a key pair in [SSH-2 RSA key format](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-trusted-signers.html#private-content-creating-cloudfront-key-pairs).
2. Upload the public key from the pair to CloudFront.
3. [Add a new key group](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-trusted-signers.html#private-content-adding-trusted-signers) in CloudFront and add the created public key to this group.
4. Create a new cache policy with __Cache key settings | Query strings__ set to _All_.
5. If you use a private bucket, create a new [OAI](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-restricting-access-to-s3.html) user.
6. [Create a distribution](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/distribution-web-creating.html) and attach your key group to it:
   * Make sure to choose the same S3 bucket as specified in TeamCity.
   * __Allowed HTTP methods__ for uploading artifacts `GET`, `HEAD`, `OPTIONS`, `PUT`, `POST`, `PATCH`, `DELETE`; for downloading `GET`, `HEAD`, `OPTIONS` 
   * __Restrict viewer access__: _yes_
   * __Trusted authorization type__: _trusted key groups_
   * __Cache key and origin requests__: _Cache policy and origin request policy_
   * For private buckets, enable the _use OAI_ option and configure OAI with the following setting:
     * __Bucket policy__: _No, I will update the bucket policy_
   * For public buckets, disable the __Block public access__ option.
7. [Add a new policy](https://docs.aws.amazon.com/AmazonS3/latest/userguide/add-bucket-policy.html) to your S3 bucket. See the [policy example](#S3+Policy+Example).

When configured, the distributions should automatically appear in the _Download distribution_  and _Upload distribution_ drop-down menus.
1. Select the target CloudFront distribution.
2. In _Public key_, select the public key associated with this distribution.
3. Click the _Upload private key..._ button upload the private key from the pair.
4. Save the storage settings.

#### S3 Policy Example

For accessing a private bucket with OAI:

```Plain Text
{
"Sid": "1",
"Effect": "Allow",
"Principal": {
      "AWS": "arn:aws:iam::cloudfront:user/CloudFront Origin Access Identity <OAI ID"
  },
  "Action": [
      "s3:GetObject",
      "s3:PutObject",
      "s3:DeleteObject"
  ],
  "Resource": "arn:aws:s3:::<S3 bucket name/*"
}
```

For accessing a public bucket:

```Plain Text
{
"Sid": "PublicRead",
"Effect": "Allow",
"Principal": "*",
  "Action": [
        "s3:GetObject",
        "s3:GetObjectVersion"
    ],
  "Resource": "arn:aws:s3::<BUCKET_NAME>/*"
}
```

## Kotlin DSL

The following sample illustrates how to add an S3 bucket as custom storage for project artifacts, and set this storage as primary (default).

```Kotlin
import jetbrains.buildServer.configs.kotlin.*
import jetbrains.buildServer.configs.kotlin.projectFeatures.activeStorage
import jetbrains.buildServer.configs.kotlin.projectFeatures.s3Storage

project {
    // ...
    features {
        activeStorage {
            id = "PROJECT_EXT_37"
            activeStorageID = "PROJECT_EXT_4"
        }
        s3Storage {
            id = "PROJECT_EXT_4"
            awsEnvironment = default {
                awsRegionName = "eu-west-1"
            }
            connectionId = "AwsPrimary"
            storageName = "S3 Transfer Acceleration"
            bucketName = "dk-s3ta"
            enableTransferAcceleration = true
            forceVirtualHostAddressing = true
            verifyIntegrityAfterUpload = true
        }
    }
    // ...
}
```

> To quickly get an ID of an [AWS Connection](configuring-connections.md#AmazonWebServices) that should be used to retrieve a required bucket, navigate to the required **Administration | &lt;Your_Project&gt; | Connections** page.
> 
> <img src="dk-copy-connection-id.png" alt="Copy connection ID" width="706"/>
> 
{style="tip"}

## Migrating Artifacts To a Different Storage

<include from="configuring-artifacts-storage.md" element-id="artifactMigrationToS3"/>