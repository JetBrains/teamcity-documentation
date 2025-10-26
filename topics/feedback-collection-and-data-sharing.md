# User Feedback Collection and Data Sharing

TeamCity occasionally displays feedback forms (about once per major release) to gather user experiences and suggestions.

<img src="" width="706" alt="TeamCity feedback form"/>

The feedback, along with essential contextual data such as system information and usage statistics, is sent to JetBrains. This data is **never** shared with third parties and is used solely to better understand user needs and improve TeamCity.

For details on how we handle data, see the [JetBrains Privacy Policy](https://www.jetbrains.com/legal/docs/privacy/privacy/).

To forcibly disable any feedback collection and data sharing, add the `teamcity.feedback.csat.enabled=false` [internal property](server-startup-properties.md#TeamCity+Internal+Properties).
{instance="tc"}
