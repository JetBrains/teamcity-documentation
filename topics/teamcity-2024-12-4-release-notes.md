[//]: # (title: TeamCity 2024.12.4 Release Notes)
[//]: # (auxiliary-id: TeamCity 2024.12.4 Release Notes)


**Build 0, 24 March 2025**

### Bug

* [**TW-86451**](https://youtrack.jetbrains.com/issue/TW-86451) — Shared resource with distinct custom values sometimes doles out the same value to two same time running build configs
* [**TW-74523**](https://youtrack.jetbrains.com/issue/TW-74523) — Warnings in triggers log - \"lastProcessedModId has become unreachable, will reset it\"
* [**TW-92182**](https://youtrack.jetbrains.com/issue/TW-92182) — Artifact dependency change details cannot be displayed with eternal \"Loading...\"
* [**TW-92498**](https://youtrack.jetbrains.com/issue/TW-92498) — BuildAgent may hang on a faulty artifact publishing command
* [**TW-91928**](https://youtrack.jetbrains.com/issue/TW-91928) — Build steps configured to execute always are terminated if build was stopped due to execution timeout
* [**TW-92226**](https://youtrack.jetbrains.com/issue/TW-92226) — New Edit Mode: lags after scrolling on Cloud Profiles page 
* [**TW-92052**](https://youtrack.jetbrains.com/issue/TW-92052) — Pause is visible only after page reload

### Performance Problem

* [**TW-89953**](https://youtrack.jetbrains.com/issue/TW-89953) — The content on the project page appears and disappears when loaded

### Security

-1 security problems have been fixed. This number includes both native TeamCity issues and vulnerabilities found in 3rd-party libraries TeamCity depends on. Upstream library issues usually make up the majority of this total number, and are promptly resolved by updating these libraries to their newest versions.

To learn more about fixed vulnerabilities directly related to TeamCity, check out our [Security Bulletin](https://www.jetbrains.com/privacy-security/issues-fixed/?product=TeamCity&version=2024.12.4). Security bulletins for new versions are typically published within the next few days after the release date.