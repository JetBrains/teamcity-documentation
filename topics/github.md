[//]: # (title: Integrating TeamCity with GitHub)
[//]: # (auxiliary-id: Integrating TeamCity with GitHub;GitHub)

TeamCity integration with the GitHub issue tracker can be set up separately or as a part of TeamCity integration with the [GitHub source code hosting service](integrating-teamcity-with-vcs-hosting-services.md).

When setting up integration with GitHub (see general information [here](integrating-teamcity-with-issue-tracker.md#Enabling+Issue+Tracker+Integration)), in addition to the repository URL and other general settings, you need to configure authentication and specify the issue ID pattern.

## Authentication

TeamCity allows you to select the preferred GitHub or GitHub Enterprise authentication mode in the VCS root settings:

* anonymous connection;
* via a login/PAT (personal access token) pair;
* via refreshable access token (if a [GitHub App connection](configuring-connections.md#GitHub) is used);
* via uploaded [SSH keys](ssh-keys-management.md) (when a VCS root is configured to fetch repository data through an SSH link).

<seealso>
        <category ref="concepts">
            <a href="supported-platforms-and-environments.md">Supported Platforms and Environments</a>
        </category>
        <category ref="admin-guide">
            <a href="integrating-teamcity-with-issue-tracker.md">Integrating TeamCity with Issue Tracker</a>
            <a href="integrating-teamcity-with-vcs-hosting-services.md">Integrating TeamCity with VCS Hosting Services</a>
        </category>
</seealso>
