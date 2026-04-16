[//]: # (title: What's New in TeamCity Cloud 2026.1)


## Build 1408, 2 June 2027

If you use [Bitbucket Cloud connections](configuring-connections.md#Bitbucket+Cloud), this update has your name on it. Atlassian recently [shuffled a few endpoints](https://developer.atlassian.com/cloud/bitbucket/changelog/#CHANGE-3139), which stirred up authorization and repository retrieval on the TeamCity side. The good news: this build clears it all up, so you can get back to building and shipping great products.


<deflist collapsible="true">
    <def title="Performance issues" default-state="collapsed">
        <ul>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99747"><b>TW-99747</b></a> — Potentially unnecessary Git ls-remote operation is performed for every checking for changes</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99549"><b>TW-99549</b></a> — Too much memory is consumed by overview controller if VCS problem is reported</li>
        </ul>
    </def>

</deflist>

## Build 1337, 28 May 2027

TeamCity 2026.1 adds a new way to work with your TeamCity instances: TeamCity CLI. Alongside the browser-based UI and the extensive [REST API](https://www.jetbrains.com/help/teamcity/rest/teamcity-rest-api-documentation.html), you can now use a command-line tool to interact with TeamCity directly from the terminal.

<img src="showcase.gif" alt="TeamCity CLI in action" xmlns="" border-effect="rounded"/>


Install the CLI on any machine to check build statuses, start new builds, investigate failures, and handle many other routine tasks without leaving the command line.





<deflist collapsible="true">
    <def title="Bug fixes" default-state="collapsed">
        <ul>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99956"><b>TW-99956</b></a> — Unable to log into TeamCity and list repositories using BitBucket Cloud connection</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99775"><b>TW-99775</b></a> — Adding build type to favorites fails</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-96789"><b>TW-96789</b></a> — Agent can refuse running a new command from the server because of incorrect command id assumptions</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99407"><b>TW-99407</b></a> — Exception on attempt to finish a build if there are many unresolved output parameters</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-98876"><b>TW-98876</b></a> — Don't hide loader after end of the streaming request</li>
        </ul>
    </def>
    <def title="Minor improvements" default-state="collapsed">
        <ul>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99956"><b>TW-99956</b></a> — Unable to log into TeamCity and list repositories using BitBucket Cloud connection</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99775"><b>TW-99775</b></a> — Adding build type to favorites fails</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-96789"><b>TW-96789</b></a> — Agent can refuse running a new command from the server because of incorrect command id assumptions</li>
        </ul>
    </def>
    <def title="Performance issues" default-state="collapsed">
        <ul>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99956"><b>TW-99956</b></a> — Unable to log into TeamCity and list repositories using BitBucket Cloud connection</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99775"><b>TW-99775</b></a> — Adding build type to favorites fails</li>
        </ul>
    </def>
    <def title="Security" default-state="collapsed">
        Two security problems have been fixed. To learn more about fixed vulnerabilities directly related to TeamCity, check out our <a href="https://www.jetbrains.com/privacy-security/issues-fixed/?product=TeamCity&amp;version=2025.11.1">Security Bulletin</a>.
        <note>Security bulletins are typically published a few days after the release date.</note>
    </def>

</deflist>




## Build 1253, 13 April 2027

We are burning midnight oil working on amazing features, but nothing too major to highlight this time around.

It's like a duck on a pond: under the water it paddles furiously, but on the top you all that can be seen is the bird gently gliding. This release, TeamCity is the duck.


<deflist collapsible="true">
    <def title="Bug fixes" default-state="collapsed">
        <ul>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99956"><b>TW-99956</b></a> — Unable to log into TeamCity and list repositories using BitBucket Cloud connection</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99775"><b>TW-99775</b></a> — Adding build type to favorites fails</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-96789"><b>TW-96789</b></a> — Agent can refuse running a new command from the server because of incorrect command id assumptions</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99407"><b>TW-99407</b></a> — Exception on attempt to finish a build if there are many unresolved output parameters</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-98876"><b>TW-98876</b></a> — Don't hide loader after end of the streaming request</li>
        </ul>
    </def>
    <def title="Performance issues" default-state="collapsed">
        <ul>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99956"><b>TW-99956</b></a> — Unable to log into TeamCity and list repositories using BitBucket Cloud connection</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99775"><b>TW-99775</b></a> — Adding build type to favorites fails</li>
        </ul>
    </def>
</deflist>



## Build 1221, 6 April 2027

This update is not a huge one, but let that not convince you it’s unimportant. They can’t all be red-letter days (or updates), but each day we’re here is a good one. This update is a good one too.


<deflist collapsible="true">
    <def title="Bug fixes" default-state="collapsed">
        <ul>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99956"><b>TW-99956</b></a> — Unable to log into TeamCity and list repositories using BitBucket Cloud connection</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99775"><b>TW-99775</b></a> — Adding build type to favorites fails</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-96789"><b>TW-96789</b></a> — Agent can refuse running a new command from the server because of incorrect command id assumptions</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99407"><b>TW-99407</b></a> — Exception on attempt to finish a build if there are many unresolved output parameters</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-98876"><b>TW-98876</b></a> — Don't hide loader after end of the streaming request</li>
        </ul>
    </def>
</deflist>



## Build 1183, 22 March 2027

### Advanced build and test actions

Starting with version 2025.11, pipelines support some of advanced features that was previously available only in build configurations. Users can now process build and test failures: [assign investigations](investigating-and-muting-build-failures.md#Investigations), [mute irrelevant failures](investigating-and-muting-build-failures.md#Mutes), and manually label as fixed issues that are expected to be resolved in future builds.

<img src="dk-pipelines-investigations.png" width="706" alt="Investigations and mutes in pipelines"/>

In addition, the run actions menu now includes options to [pin, tag, and comment](build-actions.md) individual pipeline runs.

<img src="dk-build-actions-pipelines.png" width="706" alt="Pin, tag, and comment actions in pipelines"/>

Learn more: [](investigating-and-muting-build-failures.md), [](build-actions.md)


### Reimagined Creation Flows

Every new TeamCity journey starts with "New Project", "New Build Configuration", and "New Connection" pages (unless you are a [Kotlin DSL](kotlin-dsl.md) or [REST API](https://www.jetbrains.com/help/teamcity/rest/teamcity-rest-api-documentation.html) expert!). In version 2025.11, we are rethinking these pages for a faster, more intuitive creation process. Whether you are reusing an existing connection, sharing a VCS root, or creating a build configuration without a repository, everything you need is right at your fingertips.

<img src="build-configuraiton-creation-options.png" width="706" alt="All build config creation options"/>

Learn more: [Creating projects](creating-and-editing-projects.md), [Creating build configurations](creating-and-editing-build-configurations.md).


<deflist collapsible="true">
    <def title="Bug fixes" default-state="collapsed">
        <ul>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99956"><b>TW-99956</b></a> — Unable to log into TeamCity and list repositories using BitBucket Cloud connection</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99775"><b>TW-99775</b></a> — Adding build type to favorites fails</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-96789"><b>TW-96789</b></a> — Agent can refuse running a new command from the server because of incorrect command id assumptions</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99407"><b>TW-99407</b></a> — Exception on attempt to finish a build if there are many unresolved output parameters</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-98876"><b>TW-98876</b></a> — Don't hide loader after end of the streaming request</li>
        </ul>
    </def>
    <def title="Performance issues" default-state="collapsed">
        <ul>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99956"><b>TW-99956</b></a> — Unable to log into TeamCity and list repositories using BitBucket Cloud connection</li>
            <li><a href="https://youtrack.jetbrains.com/issue/TW-99775"><b>TW-99775</b></a> — Adding build type to favorites fails</li>
        </ul>
    </def>
</deflist>