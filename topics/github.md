[//]: # (title: GitHub)
[//]: # (auxiliary-id: GitHub)

TeamCity integration with GitHub issue tracker can be set up separately or as a part of TeamCity integration with the [GitHub source code hosting service](integrating-teamcity-with-vcs-hosting-services.md#Connecting+to+GitHub).

When setting up integration with GitHub (see general information [here](integrating-teamcity-with-issue-tracker.md#Enabling+Issue+Tracker+Integration)), in addition to the repository URL and other general settings, you need to configure authentication and specify the issue ID pattern.

## Authentication

TeamCity allows you to select whether you want to connect to GitHub anonymously or to be authenticated via a personal access token (PAT).

>Authentication via login/password is [deprecated](https://developer.github.com/changes/2020-02-14-deprecating-password-auth/) by GitHub.
>We highly recommend that you authenticate with access tokens instead.
> 
{type="warning"}

<seealso>
        <category ref="concepts">
            <a href="supported-platforms-and-environments.md">Supported Platforms and Environments</a>
        </category>
        <category ref="admin-guide">
            <a href="integrating-teamcity-with-issue-tracker.md">Integrating TeamCity with Issue Tracker</a>
            <a href="integrating-teamcity-with-vcs-hosting-services.md">Integrating TeamCity with VCS Hosting Services</a>
        </category>
</seealso>
