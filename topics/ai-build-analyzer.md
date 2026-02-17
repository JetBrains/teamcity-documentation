# AI Build Analyzer

<snippet id="ai-build-analyzer">

Add the **AI Build Analyzer** feature to allow TeamCity to automatically inspect failed builds of this configuration. Once finished, the Analyzer will publish an investigation report on the build results page.

<img src="ai-analyzer-report.png" width="706" alt="AI Analyzer Report"/>

Every time the Analyzer reviews a failed build, it charges 125 [build credits](teamcity-cloud-subscription-and-licensing.md#Using+Build+Credits). Subsequent investigations are charged individually, regardless of whether you made any changes since the last failure report. You can disable the build feature at any moment to avoid any additional costs.

> The AI Build Analyzer can operate only if the core AI Assistant functionality is enabled. If you disable the Assistant in your **Administration | AI Assistant** section, the Analyzer will be disabled as well.
> 
{style="note"}

</snippet>