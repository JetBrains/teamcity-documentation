[//]: # (title: TeamCity 2023.05.3 Release Notes)
[//]: # (auxiliary-id: TeamCity 2023.05.3 Release Notes)

__Build: 129390__

__24 August 2023__


<!--project: TeamCity Fix versions: {2023.05.3 (129390)} #Fixed visible to: {All Users} -{Trunk issue}-->


### Bug

**[TW-82982](https://youtrack.jetbrains.com/issue/TW-82982/Listing-repositories-may-fail-on-create-from-Azure-DevOps-OAuth-connection-form-due-to-an-organization-not-providing-access-to)** — Listing repositories may fail on create from Azure DevOps OAuth connection form due to an organization not providing access to third-party OAuth applications

**[TW-82940](https://youtrack.jetbrains.com/issue/TW-82940/GitHub-App-connection-prepares-a-list-of-repositories-for-too-long)** — GitHub App connection prepares a list of repositories for too long

**[TW-83121](https://youtrack.jetbrains.com/issue/TW-83121/Unexpected-exception-appears-within-agent-log-after-each-closing-of-Agent-Terminal)** — Unexpected exception appears within agent log after each closing of Agent Terminal

**[TW-82979](https://youtrack.jetbrains.com/issue/TW-82979/Use-custom-SSL-certificate-when-error-setting-certificate-file-occurred)** — Use custom SSL certificate when "error setting certificate file" occurred

**[TW-82938](https://youtrack.jetbrains.com/issue/TW-82938/Commit-Status-Publisher-incorrectly-parses-some-Azure-DevOps-URLs-in-visualstudio.com-domain)** — Commit Status Publisher incorrectly parses some Azure DevOps URLs in visualstudio.com domain

**[TW-80374](https://youtrack.jetbrains.com/issue/TW-80374/IdeaRunner-unable-to-use-JRT-protocol-with-JDK-Jar-Files-Patterns)** — IdeaRunner: unable to use JRT protocol with "JDK Jar Files Patterns"

**[TW-83027](https://youtrack.jetbrains.com/issue/TW-83027/Hanging-builds-during-backup-cleanup)** — Hanging builds during backup + cleanup

**[TW-81292](https://youtrack.jetbrains.com/issue/TW-81292/GitHub-App-Improve-error-messages-in-features-when-there-are-not-enough-permissions-in-app)** — GitHub App: Improve error messages in features when there are not enough permissions in app

**[TW-82864](https://youtrack.jetbrains.com/issue/TW-82864/Symbolic-link-folders-are-absent-in-build-artifact-paths-on-Windows-machines)** — Symbolic link folders are absent in build artifact paths on Windows machines

**[TW-81178](https://youtrack.jetbrains.com/issue/TW-81178/GitHub-App-Improve-error-messages-appearing-during-usage-of-the-GitHub-App-Connection.-Part2)** — GitHub App: Improve error messages appearing during usage of the GitHub App Connection. Part2

**[TW-44069](https://youtrack.jetbrains.com/issue/TW-44069/Invalid-hint-for-the-field-Attribute-Filters-for-dotCover-in-the-.net-runners)** — Invalid hint for the field "Attribute Filters:" for dotCover in the .net runners

**[TW-82983](https://youtrack.jetbrains.com/issue/TW-82983/Failed-to-resolve-artifact-dependency-in-multinode-setup-with-external-artifact-storage-if-dependency-and-dependent-builds-are)** — Failed to resolve artifact dependency in multinode setup with external artifact storage if dependency and dependent builds are assigned to different nodes

**[TW-81917](https://youtrack.jetbrains.com/issue/TW-81917/Unusually-high-anchor-number-in-build-log)** — Unusually high anchor number in build log

**[TW-81869](https://youtrack.jetbrains.com/issue/TW-81869/S3-Storage-S3-artifact-publishing-requires-https-after-upgrading-to-2023.05)** — [S3 Storage] S3 artifact publishing requires https after upgrading to 2023.05

**[TW-75391](https://youtrack.jetbrains.com/issue/TW-75391/400-on-editing-a-long-parameter-spec)** — 400 on editing a long parameter spec

**[TW-81957](https://youtrack.jetbrains.com/issue/TW-81957/S3-Artifact-Storage-Uploads-failing-with-org.apache.http.NoHttpResponseException)** — [S3 Artifact Storage] Uploads failing with org.apache.http.NoHttpResponseException

**[TW-82766](https://youtrack.jetbrains.com/issue/TW-82766/Custom-AMI-field-is-absent-when-editing-EC2-Cloud-Image-with-Public-Shared-AMI)** — Custom AMI field is absent when editing EC2 Cloud Image with Public/Shared AMI

**[TW-82811](https://youtrack.jetbrains.com/issue/TW-82811/Allow-to-disable-automatic-generation-of-P4HOST-P4USER-P4CLIENT-environment-variables-for-builds-which-use-Perforce-VCS-Root)** — Allow to disable automatic generation of P4HOST, P4USER, P4CLIENT environment variables for builds which use Perforce VCS Root


### Performance Problem

**[TW-82813](https://youtrack.jetbrains.com/issue/TW-82813/Speedup-persisting-of-the-changed-files-associated-with-a-VCS-commit)** — Speedup persisting of the changed files associated with a VCS commit

**[TW-82835](https://youtrack.jetbrains.com/issue/TW-82835/Enabling-of-VCS-polling-on-multiple-VCS-nodes-can-slow-down-checking-for-changes)** — Enabling of VCS polling on multiple VCS nodes can slow down checking for changes


<!--project: TeamCity Fix versions: {2023.05.3 (129390)} #Fixed #{Security Problem}  -{Trunk issue}-->

### Security

5 security problems have been fixed.

> We do not share the details of security-related issues to avoid compromising clients that keep using previous bugfix and/or major versions of TeamCity. Check out our [Security Bulletin](https://www.jetbrains.com/privacy-security/issues-fixed/?product=TeamCity&version=2023.05.3) for the list of disclosed vulnerability fixes.
>
{style="note"}

