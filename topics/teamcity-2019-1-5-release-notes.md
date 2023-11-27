[//]: # (title: TeamCity 2019.1.5 Release Notes)
[//]: # (auxiliary-id: TeamCity 2019.1.5 Release Notes)


__Build: 66605__

### Security Problem

3 security problems have been fixed:

* 1 major problem with potential exposure of some server-stored passwords to users without due permissions.
* 2 problems concerned the possibility of a [reverse tabnabbing](https://www.owasp.org/index.php/Reverse_Tabnabbing) in several TeamCity plugins.

### Feature

* [TW-61510](https://youtrack.jetbrains.com/issue/TW-61510) — Use uploaded trusted SSL certificated for email notifications sending

### Usability Problem

* [TW-61488](https://youtrack.jetbrains.com/issue/TW-61488) — Experimental UI shows time since the build started instead of finished

### Bug

* [TW-52924](https://youtrack.jetbrains.com/issue/TW-52924) — \[S3] Retry uploading to S3
* [TW-55003](https://youtrack.jetbrains.com/issue/TW-55003) — ProjectManagerImpl.getProjectIds: Comparison method violates its general contract!
* [TW-59527](https://youtrack.jetbrains.com/issue/TW-59527) — Corrupted "Configure Build Agent Properties" Java window layout during agent installation under Windows 10 (bad bundled Java)
* [TW-60258](https://youtrack.jetbrains.com/issue/TW-60258) — More cloud agents can be started than there are available agent licenses
* [TW-61317](https://youtrack.jetbrains.com/issue/TW-61317) — Performing a POST request using Bearer token returns 403 error of missing Origin header, same request with user+pass works fine.
* [TW-61883](https://youtrack.jetbrains.com/issue/TW-61883) — Infinite loop (and CPU load) calculating build committers for a notification
* [TW-61893](https://youtrack.jetbrains.com/issue/TW-61893) — Filter by branches does not work in all Project tabs except for Overview tab
* [TW-62306](https://youtrack.jetbrains.com/issue/TW-62306) — Chrome tab crashes with "Not enough memory to open this page" when leaved open on build step's page with many running builds
* [TW-62444](https://youtrack.jetbrains.com/issue/TW-62444) — Can no longer use default credential chain in ECS plugin
* [TW-62489](https://youtrack.jetbrains.com/issue/TW-62489) — Broken path to build.gradle file for autodetected build steps
* [TW-62519](https://youtrack.jetbrains.com/issue/TW-62519) — Secondary nodes stops processing events after error 'Error while processing multi-node event: buildStarted'
* [TW-62545](https://youtrack.jetbrains.com/issue/TW-62545) — Certain package doesn't get published to the nuget feed since version 2019.1.4
* [TW-62546](https://youtrack.jetbrains.com/issue/TW-62546) — Server can get slow on build metrics retrieval (many different statistic values caused by custom build start precondition)
* [TW-62595](https://youtrack.jetbrains.com/issue/TW-62595) — Amazon ECR Connection: "Default Credential Provider Chain" option is no longer available after update to 2019.1.4
* [TW-62609](https://youtrack.jetbrains.com/issue/TW-62609) — Build doesn't stop: "Data too long for column 'task_identity' at row 1" error
* [TW-62611](https://youtrack.jetbrains.com/issue/TW-62611) — dotnet CLI agents part is not loading under Java 1.6
* [TW-62668](https://youtrack.jetbrains.com/issue/TW-62668) — Browser auto-fill can suggest previously used passwords in "password" field for a certain setting
* [TW-62669](https://youtrack.jetbrains.com/issue/TW-62669) — Build ends up in the top of the queue when stopped with "re-add to the queue" option
* [TW-62704](https://youtrack.jetbrains.com/issue/TW-62704) — Empty password for run agent is displayed as not empty in Agent Push preset window
* [TW-62710](https://youtrack.jetbrains.com/issue/TW-62710) — cloud instances stuck in "will be stopped soon" status
* [TW-62765](https://youtrack.jetbrains.com/issue/TW-62765) — Unnecessary build configuration ids group can be created in database if there is a hash collision of this group with another one

### Performance Problem

* [TW-62483](https://youtrack.jetbrains.com/issue/TW-62483) — Speedup agent pool expanding in case when there are many projects in the pool (17k active, 12k archived)
* [TW-62568](https://youtrack.jetbrains.com/issue/TW-62568) — Investigations auto assigner long running background threads

### Task

* [TW-62466](https://youtrack.jetbrains.com/issue/TW-62466) — Need to upgrade docker in teamcity-agent image to fix issue with unexpected docker stop

### Cosmetics

* [TW-62604](https://youtrack.jetbrains.com/issue/TW-62604) — Ambiguous message when waiting for to ‘Run build on the same agent’  