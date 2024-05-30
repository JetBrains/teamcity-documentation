[//]: # (title: TeamCity 2022.10.6 Release Notes)
[//]: # (auxiliary-id: TeamCity 2022.10.6 Release Notes)

__Build: 117311__

__30 May 2024__

<!--Project: TeamCity Fix versions: 2022.10.5  visible to: {All Users} #Fixed #Testing -{Trunk issue}-->


### Bug

**[TW-87765](https://youtrack.jetbrains.com/issue/TW-87765/Subproject-administrator-cannot-view-NuGet-feed-and-Artifact-storage-settings-of-a-parent-project)** — Subproject administrator cannot view NuGet feed and Artifact storage settings of a parent project

**[TW-87750](https://youtrack.jetbrains.com/issue/TW-87750/The-user-with-the-Project-Administrator-role-cant-see-the-NuGet-feed)** — The user with the Project Administrator role can't see the NuGet feed

<!--Project: TeamCity Fix versions: {2022.10.5 (117305)}  #{Security Problem} #Fixed #Testing -{Trunk issue} -bulletin-exclude-->


### Security

23 security issues were fixed. To protect customers who have not yet updated their servers, we typically withhold details about these fixes. Instead, we encourage you to review our [Security Bulletin](https://www.jetbrains.com/privacy-security/issues-fixed/?product=TeamCity) a few days after each bugfix release for more information.

In our effort to enhance transparency and due to potential delays in publishing new security bulletins (stemming from the simultaneous release of the 2022.04.6, 2022.10.5, 2023.05.5, 2023.11.5, and 2024.03.2 bug-fix updates), we have decided to provide a summary of both new and backported fixes.

#### Backported Fixes

These issues were resolved in newer major TeamCity versions and backported to this bug-fix update. You can find more info about them in our [Security Bulletin](https://www.jetbrains.com/privacy-security/issues-fixed/?product=TeamCity).

* Path traversal allowed reading data within JAR archives. Reported by Sndav Bai and Crispr Xiang from TianShu Dubhe Team (TW-86017)
* Stored XSS during restore from backup was possible (TW-82309)
* Authentication bypass allowing to perform admin actions was possible. Reported by Rapid7 team (TW-86500)
* Authentication bypass leading to RCE was possible. Reported by Sndav Bai and Crispr Xiang from TianShu Dubhe Team (TW-86005)
* Path traversal allowing to perform limited admin actions was possible. Reported by Rapid7 team (TW-86502)
* XXE was possible in the Maven build steps detector (TW-86300)
* Authenticated users without administrative permissions could register other users when self-registration was disabled (TW-87046)
* Server administrators could remove arbitrary files from the server by installing tools (TW-86039)
* Presigned URL generation requests in S3 Artifact Storage plugin were authorized improperly (TW-85562)
* Stored XSS was possible during nodes configuration (TW-83216)
* Open redirect was possible on the login page (TW-87062)
* Limited directory traversal was possible in the Kotlin DSL documentation (TW-85585)
* Authentication bypass leading to RCE on TeamCity Server was possible. Reported by Stefan Schiller from Sonar (TW-83545)

#### Recently Resolved Issues

Fixes for the following security issues are not immediately available in our Security Bulletin: newly discovered and fixed problems, issues from upstream libraries outside the TeamCity codebase, specific cases of previously reported vulnerabilities, and others.

You can expect details on most of these issues to be published in our [Security Bulletin](https://www.jetbrains.com/privacy-security/issues-fixed/?product=TeamCity) a few days after the official release.

* Path traversal allowing to read files from server was possible
* TeamCity server could be accessed without authorization during specific brief moments of its lifecycle
* Several Stored XSS in code inspection reports
* Improper access control in Pull Requests and Commit status publisher build features
* A third-party agent could impersonate a cloud agent
* An XSS could be executed via certain report grouping and filtering operations
* Stored XSS via third-party reports was possible
* Reflected XSS via OAuth provider configuration was possible
* Stored XSS via issue tracker integration was possible
* Stored XSS via OAuth connection settings was possible
