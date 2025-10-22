# AI Assistant

The TeamCity AI Assistant is your 24/7 companion for debugging failed workflows, suggesting optimal configurations, and guiding you through TeamCity’s capabilities.

## Key Takeaways

**What can TeamCity AI Assistant do?**<br/>
TeamCity AI Assistant offers general guidance (for example, “How do I configure the TeamCity NuGet feed?” or “How do I set up pull requests?”) and troubleshooting help for specific configurations and builds (for instance, “Why did build #17 in my SampleApp configuration fail?”). It can also query the [](teamcity-rest-api.md) to retrieve detailed information about builds and build chains.

**Is it free?**<br/>
This feature may become a paid option in future releases but will remain free throughout the Early Access Program.

**Are there any additional requirements?**<br/>
AI Assistant requires internet access to send user prompts and receive responses, obtain license keys, share usage statistics, and more.

**Hold on, what data does the Assistant collect and share?**<br/>
See the [](#Privacy+Policy) section.


**What model does TeamCity AI Assistant use?**<br/>
TeamCity AI Assistant uses `gpt4o` to communicate with users.


## Initial Setup

AI Assistant ships with all TeamCity 2025.11 installations in a disabled state. However, a corresponding menu element in the TeamCity header is shown to promote this feature.

<img src="ai-assistant-promo-disabled.png" width="706" alt="Disabled AI Assistant"/>

Server administrators can turn it completely off in the **Admin | AI Assistant** section.

<img src="" width="706" alt="Server AI Assistant settings"/>


## Limitations and Special Notes

* The Assistant does not currently support [pipelines](create-and-edit-pipelines.md) and can only analyze and troubleshoot classic [build configurations](creating-and-editing-build-configurations.md).

* Past conversations with the AI Assistant are not saved. When you start a new chat (via the "**+**" button in the chat window's top-right corner), you will lose access to the previous one.


## Privacy Policy

TeamCity AI Assistant shares user prompts and code snippets with 3rd-party LLM providers (the currently used LLM is OpenAI's `gpt4o`) to process questions and generate helpful responses. 

We may also collect non-anonymous usage data and system information to help us improve the feature. This data is used solely by TeamCity team and is not shared with any external party.

Data collection complies with [general JetBrains Privacy Notice](https://www.jetbrains.com/legal/docs/privacy/privacy/) and [JetBrains AI Data Collection and Usage Notice](https://www.jetbrains.com/help/ai/data-collection-and-use-policy.html).