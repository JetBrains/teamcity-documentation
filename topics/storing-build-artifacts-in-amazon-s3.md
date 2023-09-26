[//]: # (title: Storing Build Artifacts in Amazon S3)
[//]: # (auxiliary-id: Storing Build Artifacts in Amazon S3)

TeamCity comes bundled with the [Amazon S3 Artifact Storage](https://plugins.jetbrains.com/plugin/9623-aws-s3-artifact-storage) plugin which allows storing build artifacts in an Amazon S3 bucket.

It is possible to replace the TeamCity built-in artifacts' storage with [Amazon S3](https://aws.amazon.com/s3/) at a project level. When an S3 artifacts storage is configured, it:
* allows uploading to, downloading, and removing artifacts from S3;
* handles resolution of artifact dependencies as well as clean-up of artifacts;
* displays artifacts located externally in the TeamCity UI.


## Create and Set Up a New AWS S3 Storage

1. Navigate to the **Administration | &lt;Your_Project&gt;** page and switch to the **Artifacts Storage** tab.

   * Open settings of a &lt;Root project&gt; if you want your new storage to be available for all TeamCity projects.
   * Edit one specific project if your new storage should be available only for this project and its sub-projects.

2. The built-in TeamCity artifacts storage is displayed by default and marked as active. Click **Add new storage** button to create a new storage.

3. Specify the custom storage name and, if needed, its internal [ID](identifier.md).

4. Set the **Type** field to "AWS S3".

5. Choose an existing [AWS Connection](configuring-connections.md#AmazonWebServices) that TeamCity should use to access your Amazon resources.<anchor name="permissions"/> A user whose credentials the selected AWS Connection uses (or an IAM Role it assumes) to access the S3 buckets should have the following permissions:

   * `ListAllMyBuckets`
   * `GetBucketLocation`
   * `GetObject`
   * `ListBucket`
   * `PutObject`
   * `DeleteObject`
   * `GetAccelerateConfiguration` (if [Transfer Acceleration](#TransferAcceleration) is enabled)
    
    > <anchor name="transferToConnection"/>In previous TeamCity versions, the **Artifacts Storage** dialog allowed you to explicitly specify connection settings: access key credentials, user IAM role, and whether TeamCity should look for credentails in the default AWS locations (the **Default provider chain** setting).
    > 
    > Starting with version 2023.11, these settings are exclusive to [AWS Connections](configuring-connections.md#AmazonWebServices). If you're migrating from an older version of TeamCity and your existing storages used any of these settings, click the **Convert to AWS Connection** link. This action transfers AWS-related settings to a new AWS Connection, and selects this new connection as the source connection of your storage.
    >
    {type="tip"}
    
6. TeamCity uses the selected AWS Connection to retrieve the list of available S3 buckets. Open the **Bucket** drop-down menu to choose a specific item from the list.

7. <anchor name="pathPrefix"/>(Optional) Specify the [path prefix](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/using-folders.html) if you want to use the same S3 bucket for all TeamCity projects and configure prefix-based permissions.

8. Amazon S3 buckets support two options to speed up file uploads and downloads:

   * [AWS CloudFront](https://aws.amazon.com/cloudfront/) — a content delivery network (CDN) that allows TeamCity to transfer arifacts using low-latency CloudFront servers nearby.
   * <anchor name="TransferAcceleration"/>[Transfer Acceleration](https://aws.amazon.com/s3/transfer-acceleration/) — a bucket-level feature designed to optimize transfer speeds from across the world into centralized S3 buckets. It enables fast, easy, and secure transfers of files over long distances between your client and an S3 bucket.
    
    If your bucket is configured to use Transfer Acceleration or CloudFront, choose the corresponding option under the **Transfer speed-up** section. Otherwise, if you wish TeamCity to transfer files in the regular mode, choose the **None** type.

    <img src="dk-s3-speedUpMode.png" width="706" alt="S3 Transfer SpeedUp Mode"/>
    
    > Choosing the **AWS CloudFront** option requires setting up additional fields. See this section for more information: [](#CloudFrontSettings).
    > 
    {type="note"}

    > Using Transfer Acceleration applies the following restrictions:
    >
    > * The [virtual host addressing](#forceVirtualHostAddressing) is always on. Path-style requests are not supported.
    > * Transfer Acceleration must be enabled on your S3 bucket.
    > * The bucket name must be DNS-compliant (must not contain periods `.`).
    > * User credentials or IAM role the AWS Connection uses to access your bucket must include the `GetAccelerateConfiguration` permission.
    >
    {type="note"}

9. <anchor name="multipartUpload"/>To optimize the [upload of large files](https://aws.amazon.com/premiumsupport/knowledge-center/s3-upload-large-files/) to [Amazon S3](storing-build-artifacts-in-amazon-s3.md), you can enable the [multipart upload](https://docs.aws.amazon.com/AmazonS3/latest/userguide/mpuoverview.html). To do this, tick the **Customize threshold and part size** setting and set the multipart upload threshold. The minimum allowed value is `5MB`. Supported suffixes: `KB`, `MB`, `GB`, `TB`. If you leave this field empty, multipart upload will be initiated automatically for all files larger than 8 MB (`8MB` is the default value).

    <img src="dk-s3-multipart.png" width="706" alt="Multipart upload"/>

    Additionally, you can configure the maximum allowed size of each uploaded file part. The minimum value is `5MB`. If left empty, TeamCity will use `8MB` as the default value.
    
    > We recommend that you configure a [bucket lifecycle policy](https://docs.aws.amazon.com/AmazonS3/latest/userguide/mpu-abort-incomplete-mpu-lifecycle-config.html) to prevent incomplete multipart uploads.
    >
    {type="tip"}

10. <anchor name="forceVirtualHostAddressing"/>Check the **Force virtual host addressing** option to enable the [corresponding feature](https://docs.aws.amazon.com/AmazonS3/latest/userguide/VirtualHosting.html). Currently, both hosted-style and path-style requests are supported by TeamCity. Note that Amazon [stopped supporting path-style access](https://docs.aws.amazon.com/AmazonS3/latest/dev/VirtualHosting.html#path-style-access) for new buckets since September 2020.

    > This setting cannot be disabled if your storage uses [Transfer Acceleration](#TransferAcceleration).
    >
    {type="note"}

11. Tick **Verify file integrity after upload** to allow TeamCity to perform an additional [check-up on uploaded files](https://docs.aws.amazon.com/AmazonS3/latest/userguide/checking-object-integrity.html#checking-object-integrity-md5). If the integrity verification fails, TeamCity writes a corresponding message to the build log.

12. Click **Save** to save your new storage and return to the list of available storages.

To choose which storage should be used by a project, click **Make Active** next to a required storage. The **has N usages** link allows you to view which builds used this storage to upload their artifacts.


<img src="dk-s3-makeActive.png" width="706" alt="Make storage active"/>



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

To enable the CloudFront support for the current S3 bucket, activate the _Use CloudFront to transport artifacts_ option in the storage settings.

In each of the dropdowns, _Distribution for uploads_ and _Distribution for downloads_, choose an available distribution 
if it was [manually created](#Manual+CloudFront+Setup) in your Amazon profile, 
or click <img src="magic-wand.png" alt="Switch to the Sakura UI" height="20" width="20"/> to _[configure settings automatically](#Automatic+CloudFront+Setup)_.

For Cloudfront settings to work properly TeamCity needs new permissions:

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

When configured, the distributions should automatically appear in the _Distribution for uploads_  and _Distribution for downloads_ drop-down menus.
1. Select the target CloudFront distribution.
2. In _CloudFront public key_, select the public key associated with this distribution.
3. In _Private SSH key_, upload the private key from the pair.
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

## Migrating Artifacts To a Different Storage

<include src="configuring-artifacts-storage.md" include-id="artifactMigrationToS3"/>