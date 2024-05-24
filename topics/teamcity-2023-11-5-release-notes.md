[//]: # (title: TeamCity 2023.11.5 Release Notes)
[//]: # (auxiliary-id: TeamCity 2023.11.5 Release Notes)


**Build 147631, 24 May 2024**


<!--Project: TeamCity Fix versions: 2023.11.5  -{2023.11.4 (147586)}  visible to: {All Users} #Fixed #Testing -{Trunk issue}-->


### Bug

**[TW-87765](https://youtrack.jetbrains.com/issue/TW-87765/Subproject-administrator-cannot-view-NuGet-feed-and-Artifact-storage-settings-of-a-parent-project)** — Subproject administrator cannot view NuGet feed and Artifact storage settings of a parent project

**[TW-87750](https://youtrack.jetbrains.com/issue/TW-87750/The-user-with-the-Project-Administrator-role-cant-see-the-NuGet-feed)** — The user with the Project Administrator role can't see the NuGet feed

**[TW-82293](https://youtrack.jetbrains.com/issue/TW-82293/bean-currentUser-not-found-within-scope-error-in-the-logs-when-an-unauthorized-user-opens-a-non-existing-page)** — "bean currentUser not found within scope" error in the logs when an unauthorized user opens a non existing page



<!--Project: TeamCity Fix versions: {2023.11.5 (147631)} #{Security Problem}  #Fixed #Testing -{Trunk issue} -bulletin-exclude-->

### Security

10 security issues were fixed. To protect customers who have not yet updated their servers, we typically withhold details about these fixes. Instead, we encourage you to review our [Security Bulletin](https://www.jetbrains.com/privacy-security/issues-fixed/?product=TeamCity) a few days after each bugfix release for more information.

In our effort to enhance transparency and due to potential delays in publishing new security bulletins (stemming from the simultaneous release of the 2022.04.6, 2022.10.5, 2023.05.5, 2023.11.5, and 2024.03.2 bug-fix updates), we have decided to provide a summary of both new and backported fixes.

#### Backported Fixes

These issues were resolved in newer major TeamCity versions and backported to this bug-fix update. You can find more info about them in our [Security Bulletin](https://www.jetbrains.com/privacy-security/issues-fixed/?product=TeamCity).

* XXE was possible in the Maven build steps detector (TW-86300)
* Authenticated users without administrative permissions could register other users when self-registration was disabled (TW-87046)
* Server administrators could remove arbitrary files from the server by installing tools (TW-86039)
* XSS was possible via Agent Distribution settings (TW-86535)
* Reflected XSS was possible via Space connection configuration (TW-86832)
* Open redirect was possible on the login page (TW-87062)
* 2FA could be bypassed by providing a special URL parameter (TW-86989)


#### Recently Resolved Issues

Fixes for the following security issues are not immediately available in our Security Bulletin: newly discovered and fixed problems, issues from upstream libraries outside the TeamCity codebase, specific cases of previously reported vulnerabilities, and others.

You can expect details on most of these issues to be published in our [Security Bulletin](https://www.jetbrains.com/privacy-security/issues-fixed/?product=TeamCity) a few days after the official release.

* Path traversal allowing to read files from server was possible
* TeamCity server could be accessed without authorization during specific brief moments of its lifecycle
* A third-party agent could impersonate a cloud agent

