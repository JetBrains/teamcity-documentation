[//]: # (title: AWS Credentials)
[//]: # (auxiliary-id: AWS Credentials;AWS credentials)

The AWS credentials [build feature](https://www.jetbrains.com/help/teamcity/adding-build-features.html) makes AWS connection credentials accessible from within a build on an agent. To use this feature, you need at least one [AWS connection](configuring-connections.md#AmazonWebServices) configured in your project.

The feature can be useful if you need to provide credentials to the builds that use the AWS CLI to upload files into S3 buckets, to run AWS ECS tasks or custom code based on the AWS SDK. We recommend using temporary credentials in these cases.

After you start a build, the AWS credentials for the build will be stored in a separate file. It is created automatically, and the file path is stored to the `AWS_SHARED_CREDENTIALS_FILE` environment variable set by TeamCity. The file is only available during the current build and will be deleted automatically after the build has finished.


## Settings

<table><tr>

<td>

**Option**

</td>

<td>

**Description**

</td></tr><tr>

<td>

AWS Connection

</td>

<td>

Select an AWS connection from the drop-down menu list.

You can choose only those connections whose **Available for builds** setting is enabled. If the target connection is owned not by a project whose build configuration you set up, but rather its parent project, ensure the **Available for sub-projects** checkbox is also ticked.

<img src="dk-shareAwsConnections.png" width="706" alt="Share AWS connections"/>


</td></tr><tr>

<td>

Session duration

</td>

<td>

This setting affects **connections with temporary credentials only** and defines how long these credentials will be valid after a build is started.

TeamCity automatically fills in this field with the default of 60 minutes.

Check the required duration. You can modify the default value if you want to increase the validity period of your temporary credentials. It can be useful for long builds.

New temporary credentials will be generated for each build with this build feature.


</td></tr></table>

## Kotlin DSL


```Kotlin
import jetbrains.buildServer.configs.kotlin.*
import jetbrains.buildServer.configs.kotlin.buildFeatures.provideAwsCredentials


object MyBuildConfig : BuildType({
    name = "Build"

    features {
        provideAwsCredentials {
            awsConnectionId = "AwsPrimary"
        }
    }
})
```

> To quickly get an ID of a target [AWS Connection](configuring-connections.md#AmazonWebServices), navigate to the required **Administration | &lt;Your_Project&gt; | Connections** page.
>
> <img src="dk-copy-connection-id.png" alt="Copy connection ID" width="706"/>
>
{style="tip"}