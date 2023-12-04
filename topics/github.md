[//]: # (title: Integrating TeamCity with GitHub Issues)
[//]: # (auxiliary-id: Integrating TeamCity with GitHub Issues;Integrating TeamCity with GitHub;GitHub)

TeamCity integration with GitHub issue tracker enables you to create connections to issues associated with GitHub projects. You need to create one issue tracker connection for each GitHub repository. After the connection is created, TeamCity displays links to GitHub issues in the TeamCity UI (for example, when these issues are mentioned in a commit message).

If your GitHub repository is public, you can configure the connection with Anonymous authentication. More generally, for private repositories, you need to ensure that the provided credentials have sufficient permission to access the connected repository.

If you already have a GitHub connection or a GitHub App connection configured in your TeamCity project (under **Project Settings | Connections**), you can use these connections to simplify the creation of a new GitHub Issues connection.


## Creating a GitHub Issues Connection

Before creating a GitHub Issues connection:

* Ensure that the [GitHub issues feature](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/enabling-features-for-your-repository) is enabled in the corresponding repository and that there is at least one issue in the repository, so that you can test the connection.
* If you want to leverage a GitHub connection or a GitHub App connection to create the issue tracker connection, make sure that a connection of this type already exists in **Project Settings | Connections**.

To create a new issue tracker connection for GitHub Issues:

1. Go to **Project Settings | Issue Trackers** and click **Create new connection**.
2. In the **Create New Issue Tracker Connection** dialog, select GitHub as the **Connection Type**.
3. In the **Display Name** field, enter a symbolic name that will be used to identify this issue tracker connection in the TeamCity UI.
4. In the **Repository URL** field, enter the URL of the GitHub repository's main page (not the URL for cloning).

   <procedure title="Configure using an existing GitHub connection" initial-collapse-state="true">
      <p>If you have a GitHub connection or a GitHub App connection configured in the current project, you can use them to initialize this connection.</p>
      <step>
          <p>Next to the <b>Repository URL</b> field click the icon corresponding either to the GitHub connection or the GitHub App connection.</p>
      </step>
      <step><p>If this is the first time you use the connection, TeamCity prompts you to <b>Sign in to GitHub</b> or <b>Sign in to GitHub App</b>. When you click the button, you are redirected to a pop-up window to authorize access to your GitHub account.</p></step>
      <step><p>TeamCity loads a list of accessible repositories and displays the list with a filter box. Enter some text in the box to find the repository you want and then select it from the list.</p></step>
      <step><p>TeamCity automatically fills in the authentication and authorization fields in the dialog.</p></step>
   </procedure>

5. Choose one of the following **Authentication** options from the dropdown list:
   * Anonymous
   * Access Token — enter your GitHub [personal access token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens) in the **Access token** field
   * Username / Password — _deprecated_ consider using an access token instead
   * GitHub App access token — select a GitHub App access token from the dropdown list (requires at least one [GitHub App connection](configuring-connections.md#GitHub) to be configured in this project)
6. In the **Issue ID Pattern** field, specify a regular expression pattern to filter the issues that belong to this project. You can usually leave this at the default setting, `#(\d+)`. See [Converting Strings into Links to Issues](https://www.jetbrains.com/help/teamcity/integrating-teamcity-with-issue-tracker.html#Converting+Strings+into+Links+to+Issues).
7. Click **Test Connection** and follow the dialog instructions to test the issue tracker connection.
8. Click **Create**.

<seealso>
        <category ref="concepts">
            <a href="supported-platforms-and-environments.md">Supported Platforms and Environments</a>
        </category>
        <category ref="admin-guide">
            <a href="integrating-teamcity-with-issue-tracker.md">Integrating TeamCity with Issue Tracker</a>
            <a href="integrating-teamcity-with-vcs-hosting-services.md#Integrating+TeamCity+with+GitHub">Integrating TeamCity with VCS Hosting Services</a>
            <a href="configuring-connections.md#GitHub">Configuring Connections</a>
            <a href="configuring-authentication-settings.md#GitHub">Configuring Authentication Settings</a>
            <a href="commit-status-publisher.md#GitHub">Commit Status Publisher</a>
            <a href="pull-requests.md#GitHub+Pull+Requests">Pull Requests</a>
            <a href="configuring-vcs-post-commit-hooks-for-teamcity.md#GitHub+and+GitHub+Enterprise">Configuring VCS Post-Commit Hooks for TeamCity</a>
        </category>
</seealso>
