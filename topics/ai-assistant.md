# AI Assistant

<primary-label ref="eap"/>

The TeamCity AI Assistant is your 24/7 companion for debugging failed workflows, suggesting optimal configurations, and guiding you through TeamCity’s capabilities.

## Key Takeaways

**What can TeamCity AI Assistant do?**<br/>
AI Assistant is equipped with a set of tools for retrieving information about the current TeamCity installation and its builds. It offers general guidance (for example, “How do I configure the TeamCity NuGet feed?” or “How do I set up pull requests?”) and troubleshooting help for specific configurations and builds (for instance, “Why did build #17 in my SampleApp configuration fail?”).

**Is it free?**<br/>
This feature may become a paid option in future releases but will remain free throughout the Early Access Program. The AI Assistant requires an active TeamCity Enterprise license. It is not available for Professional licenses or Enterprise licenses with the expired [maintenance period](licensing-policy.md#Valid+TeamCity+Versions). See the [](#Limitations+and+Special+Notes) section for more information.<br/>

**Are there any additional requirements?**<br/>
AI Assistant requires access to `https://auth.grazie.ai`, `https://api.jetbrains.ai`, and other resources to obtain authorization tokens, send user prompts and receive responses, publish usage statistics (if enabled), and more.

**What data does the Assistant collect and share?**<br/>
See the [](#Privacy+Policy) section.


**What model does TeamCity AI Assistant use?**<br/>
At the 2025.11 version launch, the AI Assistant uses `gpt-4.1` to process user requests. As we evaluate its accuracy and performance, and carefully study your feedback, we may switch to another model available through [JetBrains AI](https://www.jetbrains.com/ai/). We also plan to add an option to manually select a model via the TeamCity UI in the next release cycles.


## Initial Setup

AI Assistant ships with all TeamCity 2025.11 installations in a disabled state. To make sure you do not miss this exciting addition to TeamCity, AI Assistant keeps showing its menu item in TeamCity header even when disabled.

<img src="ai-assistant-promo-disabled.png" width="706" alt="Disabled AI Assistant"/>

Server administrators can completely hide this element in the **Admin | AI Assistant** section.

<img src="ai-assistant-admin-settings.png" width="706" alt="Server AI Assistant settings"/>

The **Allow detailed data collection** checkbox allows TeamCity to privately share AI Assistant chat history with JetBrains. We never share this data with anyone and use it solely to improve the quality of the Assistant's responses.

## 'Analyze it' Button

For AI queries to deliver relevant results, you need to provide context. A question like “Why did this build fail?” may be unclear without specifying which build you mean.

To save you from typing long queries with project and configuration details, TeamCity adds the **Analyze it** button in the top-right corner of build configuration and failed build pages. Clicking it automatically supplies the necessary context, allowing the AI Assistant to analyze failing or misbehaving workflows immediately.

<img src="aia-whats-new.png" width="706" thumbnail="true" alt="AI Assistant"/>



## Limitations and Special Notes

* We continually work to improve the AI Assistant’s response quality and provide the most accurate context for its underlying model. Still, the inherent limitations of today’s generative AI models mean that occasional inaccuracies or information hallucinations are possible. If a response seems uncertain or inconsistent, consider rephrasing your question or checking it against a reliable source. Please treat the AI Assistant as a helpful aid, not a sole source of truth, and verify its suggestions when handling mission-critical tasks.
{help-id="ai-errors-warning"}

* The Assistant does not currently support [pipelines](create-and-edit-pipelines.md) and can only analyze and troubleshoot classic [build configurations](creating-and-editing-build-configurations.md).

* Past conversations with the AI Assistant are not saved. When you start a new chat (via the "**+**" button in the chat window's top-right corner), you will lose access to the previous one.

* The AI Assistant chat history is stored locally in your browser. Conversations are not restored when you access TeamCity from a different browser or after you log out.

* The AI Assistant is disabled in the following cases:

    * You are running a TeamCity Professional server (including those that have active additional agent licenses).
    * The [maintenance period](licensing-policy.md#Valid+TeamCity+Versions) for your Enterprise license has expired.
    * Your server runs on a Java version older than Java 17.
    * You are using the classic TeamCity UI instead of [Sakura UI](teamcity-sakura-ui.md).
    * Your server is deployed in a location that does not permit the usage of JetBrains AI services. See the complete list of supported locations here: [JetBrains AI Service Territory Limitations](https://www.jetbrains.com/legal/docs/terms/jetbrains-ai/service-territory/). We expect to enable AI Assistant for China-based servers in future releases. 

* The AI Assistant currently incorrectly processes questions about queued and virtual (spawned by [](parallel-tests.md) and [](matrix-build.md) features) builds.

* The Assistant is designed to provide guidance on existing TeamCity entities. It cannot create new ones, add dependencies, modify server settings, start and stop builds, and so on. In addition, it cannot perform actions that violate [current user permissions](managing-roles-and-permissions.md) (for example, analyze builds that belong to projects a user has no permissions to access).


## Privacy Policy

TeamCity AI Assistant shares user prompts and code snippets with 3rd-party LLM providers to process questions and generate helpful responses. 

We may also collect non-anonymous usage data and system information to help us improve the feature. This data is used solely by the TeamCity team and is not shared with any external party.

Data collection complies with [general JetBrains Privacy Notice](https://www.jetbrains.com/legal/docs/privacy/privacy/) and [JetBrains AI Data Collection and Usage Notice](https://www.jetbrains.com/help/ai/data-collection-and-use-policy.html).