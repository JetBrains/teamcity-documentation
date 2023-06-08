[//]: # (title: TeamCity 2022.10.4 Release Notes)
[//]: # (auxiliary-id: TeamCity 2022.10.4 Release Notes)

__Build: 117134__

__8 June 2023__

<!--Project: TeamCity Fix versions: {2022.10.4 (117134)}  visible to: {All Users} #Fixed -{Trunk issue}-->


### Feature

**[TW-75239](https://youtrack.jetbrains.com/issue/TW-75239/Prefer-IMDSv2-in-EC2-plugin)** — Prefer IMDSv2 in EC2 plugin


### Bug

**[TW-77316](https://youtrack.jetbrains.com/issue/TW-77316/Provide-more-details-about-error-in-test-connection-for-OAuth-credentials-in-Jira-Cloud)** — Provide more details about error in test connection for OAuth credentials in Jira Cloud

**[TW-81680](https://youtrack.jetbrains.com/issue/TW-81680/Cleanup-rules-page-Disk-Usage-shows-irrelevant-data)** — Cleanup rules page: Disk Usage shows irrelevant data

**[TW-77540](https://youtrack.jetbrains.com/issue/TW-77540/Restore-from-backup-may-hang-if-database-connection-is-lost)** — Restore from backup may hang if database connection is lost

**[TW-80419](https://youtrack.jetbrains.com/issue/TW-80419/Dependencies-chains-crashed-for-chains-with-70-builds)** — Dependencies chains crashed for chains with 70+ builds

**[TW-79602](https://youtrack.jetbrains.com/issue/TW-79602/Perforce-shelve-trigger-may-start-permanently-trigger-builds-for-a-shelved-changelist)** — Perforce shelve trigger may start permanently trigger builds for a shelved changelist

**[TW-80987](https://youtrack.jetbrains.com/issue/TW-80987/teamcity-vcs.log-files-are-no-longer-available-on-the-build-agents-probably-since-migration-to-log4j2)** — teamcity-vcs.log files are no longer available on the build agents (probably since migration to log4j2)

**[TW-80253](https://youtrack.jetbrains.com/issue/TW-80253/Default-Credentials-Provider-Chain-The-security-token-included-in-the-request-is-expired)** — Default Credentials Provider Chain: The security token included in the request is expired

**[TW-80508](https://youtrack.jetbrains.com/issue/TW-80508/Cloud-agents-are-terminated-despite-Maintenance-mode-after-idle-time-10-minutes)** — Cloud agents are terminated despite Maintenance mode after idle time + 10 minutes

**[TW-76018](https://youtrack.jetbrains.com/issue/TW-76018/Git-Diagnostic-page-improve-message-for-cases-when-git-isnt-installed)** — Git Diagnostic page: improve message for cases when git isn't installed

**[TW-80069](https://youtrack.jetbrains.com/issue/TW-80069/Build-configuration-filter-unavailable-on-the-Build-History-tab-of-the-agent-page)** — Build configuration filter unavailable on the Build History tab of the agent page

**[TW-77777](https://youtrack.jetbrains.com/issue/TW-77777/Kotlin-DSL-transitive-maven-dependencies-arent-resolved-during-server-execution)** — Kotlin DSL transitive maven dependencies aren't resolved during server execution

**[TW-80297](https://youtrack.jetbrains.com/issue/TW-80297/SQL-error-in-RunningBuildsCollection-on-the-PostgreSQL-database)** — SQL error in RunningBuildsCollection on the PostgreSQL database


### Performance Problem

**[TW-78441](https://youtrack.jetbrains.com/issue/TW-78441/Very-slow-overviewstatusestrue-after-the-server-start-because-of-active-branches-calculation)** — Very slow "'/overview?statuses=true" after the server start because of active branches calculation

**[TW-80963](https://youtrack.jetbrains.com/issue/TW-80963/Build-status-recalculation-queue-can-become-overflown-if-many-builds-are-finishing-in-the-same-configuration)** — Build status recalculation queue can become overflown if many builds are finishing in the same configuration

**[TW-80602](https://youtrack.jetbrains.com/issue/TW-80602/Speedup-calculation-of-revision-affected-by-checkout-rules-if-checkout-rules-do-not-filter-files-inside-submodule-mountpoints)** — Speedup calculation of revision affected by checkout rules if checkout rules do not filter files inside submodule mountpoints


### Security

Fixed 5 security problems.