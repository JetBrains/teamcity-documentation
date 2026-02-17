# AI Build Analyzer

<snippet id="ai-build-analyzer">

Add the **AI Build Analyzer** feature to allow TeamCity to automatically inspect failed builds of this configuration. Once finished, the Analyzer will publish an investigation report on the build results page.

<img src="ai-analyzer-report.png" width="706" alt="AI Analyzer Report"/>

Every time the Analyzer reviews a failed build, it charges 125 [build credits](teamcity-cloud-subscription-and-licensing.md#Using+Build+Credits). Subsequent investigations are charged individually, regardless of whether you made any changes since the last failure report. You can disable the build feature at any moment to avoid any additional costs.

> The AI Build Analyzer can operate only if the core AI Assistant functionality is enabled. If you disable the Assistant in your **Administration | AI Assistant** section, the Analyzer will be disabled as well.
>
{style="note"}

</snippet>

The Analyzer can identify build failures caused by different issues. Based on the issue, it can suggest various fixes, from changing a specific TeamCity setting to commiting a patch in the remote repository.

* TeamCity misconfigurations — errors in branch filters, VCS root settings, token scopes, and other settings that affect only TeamCity. For example, the report below explains how to fix the error caused by the incorrect **Fetch URL** value of a corresponding [VCS root](configuring-vcs-roots.md).

    <img src="ai-analyzer-vcs-root.png" width="706" alt="Faulty VCS root"/>

* Agent-related issues — build failures caused by incorrectly set up build agents. For example, the following report appears after running the Unreal Engine `RunUAT` command with `-platform=Win64` argument on an Ubuntu TeamCity agent without the corresponding tool installed.

    <img src="ai-analyzer-uat.png" width="706" alt="RunUAT error report"/>

* Underlying code issues — failures that are not related to the way TeamCity configuration, project, or agent are configured, and caused by issues in the code sources themselves. For example, incorrect values passed to the `Assert` tests.

    <img src="ai-analyzer-assert.png" width="706" alt="Assert incorrect values"/>