# Build-scoped Token

The **Build-scoped Token** feature lets builds automatically obtain a short-lived VCS access token. This token is issued through an existing [VCS connection](configuring-connections.md) and stored in a specified parameter. Build steps can then use this token to access and modify resources in the VCS.

At the moment, tokens can only be issued via a [GitHub App connection](configuring-connections.md#github-app).

> You can also generate tokens on the [](manage-access-tokens.md) page in the TeamCity UI or via the [InstallationToken endpoint](github-app-installationToken-endpoint.md).
>
{style="tip"}

## Token Parameters

<deflist type="medium">

<def title="Repository scope">

Tokens are fine-grained and grant access only to repositories explicitly listed in the [build feature settings](#Feature+Settings). Creating a global token with access to all repositories of a user or organization is not supported.

</def>

<def title="Time to live">

A token is valid for up to 60 minutes and automatically expires when the parent build (or build chain) completes. If a build runs longer than one hour, the token may expire before being used. Currently, tokens cannot be refreshed during a build.

</def>

<def title="Permissions">

Token permissions are defined by the issuing App. For example, a [GitHub App connection](configuring-connections.md#github-app) created in **Automatic** mode uses an App with the following permissions:

<ul>
<li>contents: write</li>
<li>metadata: read</li>
<li>pull_requests: read</li>
<li>issues: read</li>
<li>members: read</li>
<li>emails: read</li>
<li>statuses: write</li>
<li>checks: write</li>
</ul>

</def>

</deflist>


## Feature Settings

To configure the **Build-scoped token** feature, specify the following properties:

<img src="dk-build-scoped-token-settings.png" width="706" alt="Main settings"/>

<deflist type="medium">

<def title="Type">

The token type. Currently, only GitHub App tokens are supported.

</def>

<def title="Parameter name">

The name of the parameter that stores the issued token. TeamCity clears this value after the build finishes and masks it to prevent exposure (for example, in build logs).

</def>

<def title="Connection">

The [TeamCity VCS connection](configuring-connections.md) used to issue the token. **GitHub App installation tokens** can only be created using a [GitHub App connection](configuring-connections.md#github-app) with **Enable build-scoped tokens** enabled.

</def>

<def title="Accessible repositories">

A newline-separated list of repositories that the token can access.

</def>

</deflist>

The following example shows how to configure this feature using [Kotlin DSL](kotlin-dsl.md):


```Kotlin
object MyConfig : BuildType({
    name = "My Build Config"

    params {
        param("GhaToken", "unknown")
    }

    steps {
        // ...
    }
    features {
        gitHubAppBuildScopedToken {
            parameterName = "GhaToken"
            connectionId = "PROJECT_EXT_79"
            targetRepositories = """
                teamcity-samples-core-concepts
                HelpLinkGenerator
            """.trimIndent()
        }
    }})
```

## Example

This example demonstrates how to use the **Build-scoped token** feature to send an authorized [GitHub REST API](https://docs.github.com/en/rest/pulls/) request that creates a pull request.


1. Create a project with an [unbound build configuration](creating-and-editing-build-configurations.md#Configuration+Without+a+Repository). Without a configured [VCS root](configuring-vcs-roots.md), it cannot access repositories directly, so all interactions must use a manually issued token.
2. <include from="common-templates.md" element-id="open-project-settings-tab"><var name="tab-name" value="Connections"/></include>
3. Create a new [GitHub App connection](configuring-connections.md#github-app) (ensure **Enable build-scoped tokens** is selected).
4. By default, the App created in step #3 has the `pull_requests: read` permission. Since tokens inherit the App’s permissions, you must extend them to create pull requests.
   * Go to [https://github.com/settings/apps](https://github.com/settings/apps) and click **Edit** next to the App linked to your TeamCity connection.
   * Open the **Permissions and events** section and expand **Repository permissions**.
   * Set **Pull requests** to "Read and write".
   * Click **Save changes**.
   * Confirm the updated permissions in your account or organization settings: go to [https://github.com/settings/installations](https://github.com/settings/installations), click **Review request**, and then **Accept new permissions**.
5. <include from="common-templates.md" element-id="open-configuration-settings-tab"><var name="configuration-tab-name" value="Parameters"/></include>
6. Create a parameter to store the issued token.
7. Open the configuration’s **Build Features** tab and add the **Build-scoped token** feature.
    * Parameter name: type the name of the parameter created in step #6.
    * Connection: choose a connection created in step #3.
    * Accessible repositories: specify the repository where the pull request will be created.
8. Add the [Command-line build step](command-line.md) that sends a `POST` request to the [`/repos/{owner}/{repo}/pulls`](https://docs.github.com/en/rest/pulls/pulls?apiVersion=2026-03-10#create-a-pull-request) endpoint.
9. Run a build. If configured correctly, the build log will include the response describing the created pull request.

```Plain Text
18:18:10     "url": "https://api.github.com/repos/OWNER/REPO/pulls/2",
18:18:10     "id": 34314330272,
18:18:10     "node_id": "PR_kwDOafdszPG887Mv-2g",
18:18:10     "html_url": "https://github.com/OWNER/REPO/pull/2",
18:18:10     "diff_url": "https://github.com/OWNER/REPO/pull/2.diff",
18:18:10     "patch_url": "https://github.com/OWNER/REPO/pull/2.patch",
18:18:10     "issue_url": "https://api.github.com/repos/OWNER/REPO/issues/2",
18:18:10     "number": 2,
18:18:10     "state": "open",
18:18:10     "locked": false,
18:18:10     "title": "Amazing new feature",
...
```

The following [Kotlin DSL sample](kotlin-dsl.md) shows the complete setup:


```Kotlin
version = "2026.1"

project {
    buildType(Build)

    features {
        githubAppConnection {
            id = "PROJECT_EXT_79"
            displayName = "GHA with tokens"
            appId = "APP_ID"
            clientId = "CLIENT_ID"
            clientSecret = "CLIENT_SECRET"
            privateKey = "PRIVATE_KEY"
            ownerUrl = "https://github.com/OWNER"
            useUniqueCallback = true
            allowBuildScopedTokens = true
        }
    }
    
object Build : BuildType({
    name = "Build"

    params { param("GhaToken", "n/a") }

    steps {
        script {
            id = "simpleRunner_1"
            scriptContent = """
                curl -L \
                  -X POST \
                  -H "Accept: application/vnd.github+json" \
                  -H "Authorization: Bearer %GhaToken%" \
                  -H "X-GitHub-Api-Version: 2026-03-10" \
                  https://api.github.com/repos/OWNER/REPO/pulls \
                  -d '{"title":"Amazing new feature","body":"Please pull these awesome changes in!","head":"new-feature","base":"main"}'
            """.trimIndent()
        }
    }

    features {
        gitHubAppBuildScopedToken {
            parameterName = "GhaToken"
            connectionId = "PROJECT_EXT_79"
            targetRepositories = "REPO"
        }
    }
})
```