[//]: # (title: What's New in TeamCity On-Premises 2025.11)

<snippet id="2025-11-tc">


## Server Encryption Enhancements
{product="tc"}

TeamCity lets you configure a custom 128-bit AES key to [encrypt all SSH keys and secrets](teamcity-configuration-and-maintenance.md#encryption-settings) to encrypt sensitive data, replacing the default key and enhancing overall server security.

Version 2025.11 adds two key improvements:

* The **Encryption settings** section in global server properties now includes a link to forcibly re-encrypt all existing entities. Use this option after rotating an encryption key to avoid keeping both old and new keys.

    <img src="start-reencryption.png" width="706" alt="Start reencryption"/>

* TeamCity can now import encryption keys from the `TEAMCITY_ENCRYPTION_KEYS` environment variable. This method is more secure than setting keys manually in the UI, as the keys are not stored in [`TeamCity Data Directory`](teamcity-data-directory.md)`/config/encryption-config.xml`, making it safer to [keep the data directory in a remote repository](teamcity-data-directory.md#TeamCityDataDirectory-centralRepository).


</snippet>
