[//]: # (title: Integrating TeamCity with Bitbucket Cloud)
[//]: # (auxiliary-id: Integrating TeamCity with Bitbucket Cloud;Bitbucket Cloud)

The TeamCity integration with the Bitbucket Cloud issue tracker can be set up separately, or as a part of TeamCity integration with [Bitbucket Cloud](integrating-teamcity-with-vcs-hosting-services.md#Integrating+TeamCity+with+Bitbucket+Cloud) as a source code hosting service.

When setting up integration with the Bitbucket issue tracker (see general information [here](integrating-teamcity-with-issue-tracker.md#Enabling+Issue+Tracker+Integration)), in addition to the repository URL and other general settings, you need to configure authentication and specify the issue ID pattern.

## Authentication

TeamCity allows you to select whether you want to connect to the Bitbucket issue tracker anonymously or to authenticate via username/password.

>For Bitbucket Cloud team accounts, it is possible to use the team name as the username and the [API key](https://developer.atlassian.com/bitbucket/api/2/reference/meta/authentication#api-key) as the password.

<seealso>
        <category ref="concepts">
            <a href="supported-platforms-and-environments.md">Supported Platforms and Environments</a>
        </category>
        <category ref="admin-guide">
            <a href="integrating-teamcity-with-issue-tracker.md">Integrating TeamCity with Issue Tracker</a>
            <a href="integrating-teamcity-with-vcs-hosting-services.md">Integrating TeamCity with VCS Hosting Services</a>
        </category>
</seealso>