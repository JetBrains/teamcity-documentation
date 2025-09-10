# InstallationToken Endpoint: How to Issue Multi-Repository Auth Tokens via Existing GitHub App Connections

TeamCity [GitHub App](configuring-connections.md#github-app) connections can issue auth tokens that allow TeamCity to access a single repository. While it covers the majority of cases where a build configuration targets a specific repository, some scenarios require tokens with a multi-repository access scope.

To issue a token that valid for accessing a limited set of repositories, send `POST` requests to the `<TeamCity_Server_URL/app/oauth/githubapp/installationToken` endpoint.

<deflist type="narrow">

<def title="Endpoint">/app/oauth/githubapp/installationToken</def>

<def title="Request type">POST</def>

<def title="Headers">

* **Content-Type:** `application/x-www-form-urlencoded`
* **Accept:** `application/json`
* (optional) **X-TC-GitHub-PAT:** a US-ASCII string that is your personal GitHub access token (issued on the GitHub user settings page). If omitted, TeamCity will try to identify a user and their permissions by checking existing tokens stored in its internal storage. See the [](#Special+Notes) section for more information.

</def>

<def title="Body">

An x-www-form-urlencoded string with the following keys:


* `projectId` — the internal ID of a project that will own a newly issued auth token.

    <tip>

    The project ID can be copied from the project settings page in TeamCity UI, or retrieved via the `app/rest/projects/name:Public%20Name?fields=id` [REST API endpoint](https://www.jetbrains.com/help/teamcity/rest/project.html).

    </tip>


* `connectionId` — the internal ID of a GitHub App connection that will be used to issue a new token. By default, has the `PROJECT_EXT_INT32` format.

    <tip>

    The connection ID can be copied from the **Connections** tab of parent project settings page, or retrieved via the `/app/rest/projects/id:ProjectID/projectFeatures?locator=type:OAuthProvider` [REST API endpoint](https://www.jetbrains.com/help/teamcity/rest/projectfeature.html)

    </tip>

* `repositoryName` — the name of a repository that the newly issued token will be able to access. The username should be omitted ("my-repo-name" instead of "johndoe/my-repo-name"). Allows you to list multiple repositories in the `repositoryName=name1&repositoryName=name2&..." format.

  Only accepts names of repositories owned by organizations or users that installed the corresponding GitHub App.

</def>

<def title="Response">

If the request finishes with HTTP code 200 "OK", the response includes a JSON object with the following fields:

* `tokenId` — the full ID of the newly issued token. 
* `permissions` — the list of permission-value pairs that describe access permissions of the issued token.
* `repositories` — the array of repository names that can be accessed using this token.
* `personalAccessTokenSource` — returns either "USER_PROVIDED" or "TOKEN_STORAGE_LOOKUP" depending on whether the user's personal access token was explicitly provided via the `X-TC-GitHub-PAT` header, or retrieved by TeamCity.

</def>

</deflist>

The cURL sample below illustrates how to use the GitHub App connection with the "PROJECT_EXT_31" ID to issue a token that has access to the "sample-java-app-maven" and "HelpLinkGenerator" repositories. The token will be owned by a project with the "GitHubMavenJavaApp" ID.

```cURL
curl --location 'http://your-server-URL/app/oauth/githubapp/installationToken' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--header 'Accept: application/json' \
--header 'X-TC-GitHub-PAT: ghp_foOBar1337' \
--header 'Authorization: Bearer 12345' \
--data 'projectId=GitHubMavenJavaApp&connectionId=PROJECT_EXT_31&repositoryName=sample-java-app-maven&repositoryName=HelpLinkGenerator'
```

If successful, the request returns the following payload:

```JSON
{
    "tokenId": "tc_token_id:CID_3183460fk032c31a4d92650323o03492:-1:1af54425-9b25-4f1e-8c63-66f062309abf",
    "permissions": {
        "CONTENTS": "WRITE",
        "METADATA": "READ_ONLY",
        "PULL_REQUESTS": "READ_ONLY",
        "ISSUES": "WRITE",
        "STATUSES": "WRITE",
        "CHECKS": "WRITE"
    },
    "repositories": [
        "sample-java-app-maven",
        "HelpLinkGenerator"
    ],
    "personalAccessTokenSource": "USER_PROVIDED"
}
```

The issued token is visible on the [VCS Auth Tokens](manage-access-tokens.md) tab of parent project settings.

<img src="dk-tokens-generated-in-rest.png" width="706" alt="Tokens generated via the endpoint shown on the project VCS Auth Tokens page"/>

## Special Notes

* The user sending a `POST` request must be a [project administrator](managing-roles-and-permissions.md) of the referenced TeamCity project. Only this project (and its child subprojects) will be able to use the newly issued token.

* The issued token has access permissions only for repositories whose names were passed in the request body. It is not possible to issue an unrestricted token that can access any user/organization repository.

* TeamCity limits an installation token’s permissions to the lowest level the user has across the selected GitHub repositories. Therefore, the user must have previously signed in to TeamCity through the corresponding GitHub App connection, such as by logging in or listing repositories on the "New Project" page. Alternatively, use the `X-TC-GitHub-PAT` to explicitly specify the user's personal access token (PAT).