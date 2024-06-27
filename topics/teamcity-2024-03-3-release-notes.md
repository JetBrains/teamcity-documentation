[//]: # (title: TeamCity 2024.03.3 Release Notes)
[//]: # (auxiliary-id: TeamCity 2024.03.3 Release Notes)


**Build 156364, 27 June 2024**


<!--Project: TeamCity Fix versions: 2024.03.3 visible to: {All Users} #Fixed #Testing -{Trunk issue}-->

### Bug

**[TW-82176](https://youtrack.jetbrains.com/issue/TW-82176/EC2-Cloud-Profile-IMDSv1-metadata-format-doesnt-work-after-upgrade-to-2023.05)** — EC2 Cloud Profile: IMDSv1 metadata format doesn't work after upgrade to 2023.05

**[TW-87124](https://youtrack.jetbrains.com/issue/TW-87124/Agents-running-from-Windows-2024.03-nanoserver-2022-docker-images-become-incompatible-with-some-runners-after-restart)** — Agents running from Windows 2024.03-nanoserver-2022 docker images become incompatible with some runners after restart

**[TW-87892](https://youtrack.jetbrains.com/issue/TW-87892/Converter-DownloadedArtifactsIndexesConverter-fails-during-the-update-from-TeamCity-7.x-to-2024.03.x)** — Converter DownloadedArtifactsIndexesConverter fails during the update from TeamCity 7.x to 2024.03.x

**[TW-88068](https://youtrack.jetbrains.com/issue/TW-88068/AWS-EC2-instance-agents-can-no-longer-authorize-on-start)** — AWS EC2 instance agents can no longer authorize on start

**[TW-88441](https://youtrack.jetbrains.com/issue/TW-88441/BuildLog-is-not-displayed-correctly-for-some-of-the-builds-produced-by-older-servers)** — BuildLog is not displayed correctly for some of the builds produced by older servers

**[TW-87489](https://youtrack.jetbrains.com/issue/TW-87489/Build-settings-have-not-been-finalized-for-hours)** — "Build settings have not been finalized" for hours

**[TW-87693](https://youtrack.jetbrains.com/issue/TW-87693/Agent-service-under-Windows-does-not-use-bundled-jre-and-fails-to-start-if-JAVAHOME-is-not-defined)** — Agent service under Windows does not use bundled jre, and fails to start if JAVA_HOME is not defined

**[TW-84271](https://youtrack.jetbrains.com/issue/TW-84271/Copy-to-clipboard-in-custom-report)** — Copy to clipboard in custom report

**[TW-88200](https://youtrack.jetbrains.com/issue/TW-88200/Artifact-dependency-for-filenames-with-symbol)** — Artifact dependency for filenames with '%' symbol

**[TW-87956](https://youtrack.jetbrains.com/issue/TW-87956/Agents-can-be-shown-as-compatible-but-cant-run-builds-after-removing-a-tool-used-in-the-build)** — Agents can be shown as compatible but can't run builds after removing a tool used in the build

**[TW-88208](https://youtrack.jetbrains.com/issue/TW-88208/Agent-side-NuGet-Cache-cleanup-is-interrupted-if-the-process-cannot-clean-it-in-under-1.5-minutes)** — Agent-side NuGet Cache cleanup is interrupted if the process cannot clean it in under 1.5 minutes

**[TW-80350](https://youtrack.jetbrains.com/issue/TW-80350/Gradle-Runner-in-TC-doesnt-find-tests-with-quotation-marks-in-filter.)** — Gradle Runner in TC doesn't find tests with quotation marks in filter.

**[TW-87930](https://youtrack.jetbrains.com/issue/TW-87930/ArtifactsStorage-page-is-not-available-for-super-user-in-some-cases)** — ArtifactsStorage page is not available for super user in some cases

### Performance Problem

**[TW-88123](https://youtrack.jetbrains.com/issue/TW-88123/Inefficient-DBVcsModificationHistory.getModificationsInVersionsRange-slows-down-REST-API-call)** — Inefficient DBVcsModificationHistory.getModificationsInVersionsRange() slows down REST API call


<!--Project: TeamCity Fix versions: {2024.03.2 (156319)}  #{Security Problem}  #Fixed #Testing -{Trunk issue} -bulletin-exclude -->

### Security

Security issues were fixed. To protect customers who have not yet updated their servers, we typically withhold details about these fixes. Instead, we encourage you to review our [Security Bulletin](https://www.jetbrains.com/privacy-security/issues-fixed/?product=TeamCity) a few days after each bugfix release for more information.

