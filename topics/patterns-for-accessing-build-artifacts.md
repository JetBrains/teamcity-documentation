[//]: # (title: Patterns for Accessing Build Artifacts)
[//]: # (auxiliary-id: Patterns for Accessing Build Artifacts)

<warning>

It is recommended to use the TeamCity [REST API](https://www.jetbrains.com/help/teamcity/rest/manage-builds.html#Build+Artifacts) for accessing artifacts from scripts, as the REST API provides build selection facilities and allows listing artifacts.
</warning>

This section is preserved for __backward compatibility__ with the previous TeamCity versions and for some specific functionality.

Check the following information as well:
* If you need to access the artifacts in your builds, consider using the TeamCity's built-in [Artifact Dependency](dependent-build.md#Artifact+Dependency) feature.
* You can also download artifacts from TeamCity using the [Ivy](artifact-dependencies.md#Configuring+Artifact+Dependencies+Using+Ant+Build+Script) dependency manager.
* For artifact downloads from outside TeamCity builds, consider using [REST API](https://www.jetbrains.com/help/teamcity/rest/teamcity-rest-api-documentation.html).
* See also [Accessing Server by HTTP](accessing-server-by-http.md) on basic rules covering an HTTP access from scripts.

## Obtaining Artifacts

__To download artifacts of the latest builds (last finished, successful or pinned)__, use the following paths:


```Shell
/repository/download/BUILD_TYPE_EXT_ID/.lastFinished/ARTIFACT_PATH
/repository/download/BUILD_TYPE_EXT_ID/.lastSuccessful/ARTIFACT_PATH
/repository/download/BUILD_TYPE_EXT_ID/.lastPinned/ARTIFACT_PATH

```

__To download artifacts by the [build id](working-with-build-results.md#Internal+Build+ID)__, use:

```Shell
/repository/download/BUILD_TYPE_EXT_ID/BUILD_ID:id/ARTIFACT_PATH

```

__To download artifacts by the build number__, use:

```Shell

/repository/download/BUILD_TYPE_EXT_ID/BUILD_NUMBER/ARTIFACT_PATH

```

__To download artifacts from the latest build with a specific tag__, use:

```Shell
/repository/download/BUILD_TYPE_EXT_ID/BUILD_TAG.tcbuildtag/ARTIFACT_PATH

```

__To download all artifacts in a .zip archive__, use:

```Shell
/repository/downloadAll/BUILD_TYPE_EXT_ID/BUILD_SPECIFICATION

```

where
* `BUILD_TYPE_EXT_ID` is a [build configuration ID](configuring-general-settings.md).
* `BUILD_SPECIFICATION` can be `.lastFinished`, `.lastSuccessful` or `.lastPinned`, specific `buildNumber` or [build id](working-with-build-results.md#Internal+Build+ID) in format `BUILD_ID:id`.
* `ARTIFACT_PATH` is a path to the artifact on the TeamCity server. This path can contain a `{build.number}` pattern (`%7Bbuild.number%7D`) which will be replaced by TeamCity with the build number of the build whose artifact is retrieved. By default, the archive with all artifacts does not include [hidden artifact](build-artifact.md#Hidden+Artifacts). To include them, add `?showAll=true` at the end of the corresponding URL.   
To download artifact from the last finished, last successful, last pinned or tagged build in a specific branch, add the `?branch=<branch_name>` parameter at the end of the corresponding URL.
  
Since 2020.1.4, this endpoint allows downloading archives larger than 4 Gb only if the `teamcity.internal.artifacts.useZip64=false` [configuration parameter](configuring-build-parameters.md) is provided, either on a build configuration or project level.

## Obtaining Artifacts from an Archive

TeamCity allows obtaining a file from an archive from the build artifacts directory by means of the following URL patterns:

```Shell
/repository/download/BUILD_TYPE_EXT_ID/BUILD_SPECIFICATION/<archive>!/PATH_WITHIN_ARCHIVE

```

* __`BUILD_TYPE_EXT_ID`__ is a [build configuration ID](configuring-general-settings.md).
* __`BUILD_SPECIFICATION`__ can be `.lastFinished`, `.lastPinned`, `.lastSuccessful`, specific `buildNumber` or [build id](working-with-build-results.md#Internal+Build+ID) in format `BUILD_ID:id`.
* __`PATH_WITHIN_ARCHIVE`__ is a path to a file within a zip/7\-zip/jar/tar.gz archive on TeamCity server.
 Following archive types are supported (case-insensitive):
* .zip
* .7z
* .jar
* .war
* .ear
* .nupkg
* .sit
* .apk
* .tar.gz
* .tgz
* .tar.gzip
* .tar

[//]: # (Internal note. Do not delete. "Patterns For Accessing Build Artifactsd243e253.txt")

## Obtaining Artifacts from a Build Script

It is often required to download artifacts of some build configuration by tools like __wget__ or another downloader which does not support HTML login page. TeamCity asks for authentication if you access the artifacts' repository.

__To authenticate correctly using token-based auth from a build script__, you have to pass your personal [access token](managing-your-user-account.md#Managing+Access+Tokens) in the HTTP header `Authorization: Bearer <token-value>`.

__To authenticate correctly using basic auth from a build script__, you have to change URLs (add the `/httpAuth/` prefix to the URL):

```Shell
/httpAuth/repository/download/BUILD_TYPE_EXT_ID/.lastFinished/ARTIFACT_PATH

```

Basic authentication is required for accessing artifacts by this URLs with the `/httpAuth/` prefix.

You can use existing TeamCity username and password in basic authentication settings, but consider using `teamcity.auth.userId`/`teamcity.auth.password` system properties as credentials for the download artifacts request: this way TeamCity will have a way to record that one build used artifacts of another and will display that on build's Dependencies tab.

__To enable downloading an artifact with guest user login__, you can use either of the following methods:

*  Use old URLs without the `/httpAuth/` prefix, but with added `guest=1` parameter. For example:

    ```Shell
    /repository/download/BUILD_TYPE_EXT_ID/.lastFinished/ARTIFACT_PATH?guest=1

    ```

* Add the `/guestAuth` prefix to the URLs, instead of using `guest=1` parameter. For example:

    ```Shell
    /guestAuth/repository/download/BUILD_TYPE_EXT_ID/.lastFinished/ARTIFACT_PATH

    ```

In this case you will not be asked for authentication. The list of the artifacts can be found in `/repository/download/BUILD_TYPE_EXT_ID/.lastFinished/teamcity-ivy.xml`.

## Links to the Artifacts Containing the TeamCity Build Number

You can use `{build.number}` (%7Bbuild.number%7D in URL) as a shortcut to current build number in the artifact file name. For example:

```Shell
http://teamcity.yourdomain.com/repository/download/MyConfExtId/.lastFinished/TeamCity-%7Bbuild.number%7D.exe

```

[//]: # (Internal note. Do not delete. "Patterns For Accessing Build Artifactsd243e349.txt")    

<seealso>
        <category ref="extending_tc">
            <a href="accessing-server-by-http.md">Accessing Server by HTTP</a>
        </category>
        <category ref="concepts">
            <a href="build-artifact.md">Build Artifact</a>
            <a href="authentication-modules.md">Authentication Modules</a>
        </category>
        <category ref="admin-guide">
            <a href="configuring-dependencies.md">Retrieving artifacts in builds</a>
        </category>
</seealso>