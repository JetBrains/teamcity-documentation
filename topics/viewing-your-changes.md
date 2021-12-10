[//]: # (title: Viewing Your Changes)
[//]: # (auxiliary-id: Viewing Your Changes)

Monitoring the quality of the codebase is essential for a development team: a project developer needs to see whether his/her commit brought a build failure or not; for a project leader it is important to detect the code at fault for a build failure to be able to have the situation rectified early, so other members of the team are not inconvenienced.

The __Changes__ page of the TeamCity web UI allows you to review the commits made by all TeamCity users and see how they have affected builds. You can filter the results with the user selector on the page.

<note>

Changes made by a user are displayed correctly only when appropriate [VCS usernames](creating-and-managing-users.md#VCS+Usernames) are defined.
</note>

By default, the page does not show the commits to the build configurations hidden by the current user on the Projects dashboard. To remove this filter and view all build configurations, deselect the __Hide configurations excluded from my Projects__ box.

Each change now has a new pie-chart icon with pie slices showing the relative size of pending, successful, as well as old and new problematic builds affected by the change. Hovering over/clicking the pie-chart icon gives a visual representation of how the user commit has affected different builds. Builds with new/critical problems are listed by default. Expanding the change or clicking the _See builds_ link lists all builds with the change.

From this page you can:
* View all commits and changes included into personal builds
* View how changes have affected the builds
* See whether there are new failed tests caused by changes
* Navigate to the issue tracker, if the [issue tracker integration is configured](integrating-teamcity-with-issue-tracker.md)
* Open possibly problematic files in your IDE: the option is available if the plugin for this [IDE is installed](installing-tools.md) and you are logged in to TeamCity from within this IDE
* Navigate to change details
* Modify the change comment in TeamCity (available to project administrators by default). Changing the comment in the VCS as well is recommended for consistency.
* View detailed data for each change on the dedicated tabs. To switch between tabs for the currently selected change, use `Tab/Shift+Tab` or mnemonics: `T` for tests, `B` for builds, `F` for files.
* View builds with any of the changes. By default, successful and pending build configurations are hidden from display. Uncheck the corresponding box to view all builds with the change.

Note that problems which have an investigator/responsible are not considered critical (unless you are the investigator).

 <seealso>
        <category ref="concepts">
            <a href="build-state.md">Build State</a>
            <a href="change.md">Change</a>
        </category>
</seealso>
