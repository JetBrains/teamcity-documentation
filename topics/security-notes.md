[//]: # (title: Security Notes)
[//]: # (auxiliary-id: Security Notes)

>The following notes are provided for reference only and are not guaranteed to be complete or entirely accurate. We suggest that you follow the best security practices when using TeamCity for production purposes. This document contains our recommendations and points to consider when configuring your CI/CD pipeline with TeamCity.
>
{style="warning"}

TeamCity is developed with security concerns in mind. We make reasonable efforts to ensure the system is invulnerable to different types of attacks. We work with third parties on assessing TeamCity security using security scanners and penetration tests. Newly discovered security issues are promptly addressed in the nearest bugfix releases ([read more](teamcity-release-cycle.md) about our release cycle). It is recommended to [upgrade to newly released TeamCity versions](https://www.jetbrains.com/teamcity/download/) as soon as they become available.  
However, the general assumption and __recommended setup is to deploy TeamCity in a trusted environment__, with no possibility for it to be accessed by malicious users.

Along with these guidelines, please review [notes](configure-server-installation.md#Configuring+Server+for+Production+Use) on configuring the TeamCity server for production use. For the list of disclosed security-related issues, see the [JetBrains Security Bulletin](https://blog.jetbrains.com/blog/tag/security-bulletin/) and the "Security" section in the release notes.
{product="tc"}

For the list of disclosed security-related issues, see the [JetBrains Security Bulletin](https://blog.jetbrains.com/?s=security+bulletin) and the "Security" section in the release notes.
{product="tcc"}

We also recommend that you subscribe to the [security notification service](https://www.jetbrains.com/privacy-security/subscribe/) to obtain the latest information about security issues that may affect TeamCity or any other JetBrains products.

<anchor name="HowTo...-Security-relatedRisksEvaluation"/>

## Recommended Security Practices

This section contains the main security recommendations to follow when using TeamCity.

>You can also download a short version of this section [in PDF format](https://resources.jetbrains.com/storage/products/blog/wp-content/uploads/teamcity/TeamCity%20Security%20Hardening.pdf) to distribute it among your colleagues.

### Credentials

__Use strong credentials, and use them carefully__.

We recommend using strong credentials not only for your TeamCity server, but also for all other services that are involved in a build or that your software requires in production.

Make especially sure to keep your credentials out of:
* Repositories, such as GitHub and GitLab.
* Environment variables, as they are often logged or shared with third-party monitoring systems.
* The [build log](build-log.md) — make sure you don't randomly log sensitive information.
* If you are using [versioned settings](storing-project-settings-in-version-control.md) (in Kotlin DSL or XML format), never store your credentials in your configuration files. Instead, use [tokens](storing-project-settings-in-version-control.md#Storing+Secure+Settings).

TeamCity users with administrative permissions should have complex passwords.

To make user password storage safer, TeamCity uses the BCrypt hashing algorithm.

__Consider disabling the super user access.__
{product="tc"}

The [](super-user.md) feature in TeamCity enables users to log in as system administrators using a token found in the [&lt;TeamCity_server_home&gt;/logs/teamcity-server.log](teamcity-server-logs.md) file.
{product="tc"}

If you publish TeamCity logs to an external source, add the `teamcity.superUser.disable=true` [internal property](server-startup-properties.md#TeamCity+Internal+Properties) to disable this authorization option and prevent unwanted administrator access. Restart the TeamCity server after modifying the `teamcity.superUser.disable` property for the change to apply.
{product="tc"}

__Store secure data using parameters with the "password" type__.

To store passwords or other secure data in the TeamCity settings, you are strongly advised to use the [typed parameters](typed-parameters.md). This will make sure that sensitive values never appear in the web UI and are masked with asterisks in the build log. Make sure passwords are stored as parameters with the [password type](typed-parameters.md).

__Use a secret management tool__.

Although password parameters are masked in the UI, encrypted at REST, and protected from being exposed in the build log as plain text, this often does not provide a high enough level of security.

You may consider using a tool like [HashiCorp Vault](https://www.vaultproject.io/), which lets you manage and rotate all the sensitive credentials you'll be using in a build and which [integrates well with TeamCity](https://blog.jetbrains.com/teamcity/2017/09/vault/).

__Use external authentication__.
{product="tc"}

If applicable, configure one of our [external authentication modules](authentication-modules.md), ranging from LDAP and Windows Domain integration to authenticating via GitHub, GitLab, or others. You can then [disable the TeamCity built-in authentication](configuring-authentication-settings.md), so that TeamCity no longer keeps hashed passwords in the internal database.
{product="tc"}

If any of the OAuth [authentication modules](configuring-authentication-settings.md) (Bitbucket Cloud, GitHub.com, GitHub Enterprise, GitLab.com, GitLab CE/EE) are enabled on your server __and__ you restrict authentication to members of a specific Bitbucket workspace, GitHub organization, or GitLab group, note the following:  
Once signed in to the TeamCity server with an external account, a user can create a password or token which will allow them to sign in to this server directly, bypassing the VCS hosting provider verification. If you delete a user from a workspace/organization/group, remember to restrict their access or delete their user profile in TeamCity as well.
{product="tc"}

__Use a custom encryption key__.
{product="tc"}

Passwords that are necessary to authenticate in external systems (like VCS, issue trackers, and so on) are stored in a scrambled form in the [TeamCity Data Directory](teamcity-data-directory.md) and can also be stored in the database. However, the values are only scrambled, which means they can be retrieved by a user who has access to the server file system or database.
{product="tc"}

Instead of this default scrambling strategy, consider enabling a [custom encryption key](teamcity-configuration-and-maintenance.md#encryption-settings). In this case, TeamCity will use your unique custom key to encrypt all secure values, instead of using the default scrambling mechanism.
{product="tc"}

__Use encrypted SSH keys__.

When you [upload private SSH keys](ssh-keys-management.md) to your TeamCity server, note that they will be stored on disk in plain text, without any additional encryption. It is strongly recommended to use only SSH keys that are already encrypted with a passphrase.

### Permissions 

__Use predefined roles__.

Out of the box, TeamCity offers several predefined [roles](managing-roles-and-permissions.md):
* System Administrator
* Project Administrator
* Project Developer
* Project Viewer

You can create [user groups](creating-and-managing-user-groups.md) that match your organizational structure and assign the above roles to those groups. Then add your users to the respective groups, granting them the lowest level of privileges they need for their day-to-day work.

It is also strongly recommended that you create new roles with additional permissions, instead of immediately assigning the Project Administrator role to anyone who needs slightly more privileges. (This does not work if you disable [per-project permissions](managing-roles-and-permissions.md#Per-Project+Authorization+Mode).)

__Use per-project authorization__.

To tighten security even more, you can also make use of [per-project authorization](managing-roles-and-permissions.md#Per-Project+Authorization+Mode). This way, your developers could, for example, have access only to the compilation part of your build chain, while devops could access and run the deployment part.

<anchor name="caution-guest-login"/>

__Do not enable Guest Login__.

By default, [logging into TeamCity anonymously](enabling-guest-login.md) is disabled. Make sure not to enable it on the production TeamCity server instances that are exposed to the internet, unless you want external users to be able to see all your builds and the associated log files/artifacts. If enabled, user roles should be carefully reviewed for guests and the [All Users](creating-and-managing-user-groups.md#%22All+Users%22+Group) group.

__Create a separate user for REST requests__.

If you access the [TeamCity REST API](https://www.jetbrains.com/help/teamcity/rest/teamcity-rest-api-documentation.html) from an external script or program, we recommend that you create a separate user with a limited number of permissions for it. It would also be wise to create [autoexpiring access tokens](configuring-your-user-profile.md#Managing+Access+Tokens), instead of using the username/passwords to access the API.

__Restrict deployment build permissions__.

Make sure that your deployment build chains do not allow [personal builds](personal-build.md). [Limit the number of developers](managing-roles-and-permissions.md) who can trigger those builds, and use a separate pool of clean agents for those builds.

<anchor name="manage-permissions"/>

__Thoroughly manage permissions granted to users and user groups__.

Note the following nuances:

* Users who can change the code that is used in the builds run by TeamCity (including committers in any branches/pull requests if they are built on TeamCity):
  * can do everything the system user, under whom the TeamCity agent is running, can do; have access to OS resources and other applications installed on the agent machines where their builds can run.
  * can access and change source code of other projects built on the same agent, modify the TeamCity agent code, publish any files as artifacts for the builds run on the agent (which means the files can be then displayed in the TeamCity web UI and expose web vulnerabilities or can be used in other builds), and so on.
  * can impersonate a TeamCity agent (run a new agent looking the same to the TeamCity server).
  * can do everything that users with the "View build configuration settings" permission for all the projects on the server can do (see [below](#view-build-config-settings)).
  * can retrieve settings of the build configurations where the builds are run, including the values of the password fields.
  * can download artifacts from any build on the server.
    <anchor name="view-build-config-settings"/>
* Users with the "View build configuration settings" permission (by default, the Project Developer role) can view all the projects on the server. To restrict this, you can use the `teamcity.buildAuth.enableStrictMode=true` [internal property](server-startup-properties.md#TeamCity+Internal+Properties).
  {product="tc"}
* Users with the "View build configuration settings" permission (by default, the Project Developer role) can view all the projects on the server.
  {product="tcc"}
* Users with the "Edit project" permission (the "Project Administrator" TeamCity role by default) in one project, by changing settings can retrieve artifacts and trigger builds from any build configuration they have only the view permission for ([TW-39209](https://youtrack.jetbrains.com/issue/TW-39209)).
* Users with the "Change server settings" permission (the "System Administrator" TeamCity role by default): It is assumed that the users also have access to the computer on which the TeamCity server is running under the user account used to run the server process. Thus, the users can get full access to the machine under that OS user account: browse file system, change files, run arbitrary commands, and so on.
* The TeamCity server computer administrators: have full access to TeamCity stored data and can affect TeamCity executed processes. Passwords that are necessary to authenticate in external systems (like VCS, issue trackers, and so on) are stored in a scrambled form in [TeamCity Data Directory](teamcity-data-directory.md) and can also be stored in the database. However, the values are only scrambled, which means they can be retrieved by any user who has access to the server file system or database.
* Users who have read access to the TeamCity server logs (TeamCity server home directory) can escalate their access to the TeamCity server administrator.
* Users who have read access to `<[TeamCity Data Directory](teamcity-data-directory.md)>` can access all the settings on the server, including configured passwords.
* Users who have read access to the build artifacts in `<[TeamCity Data Directory](teamcity-data-directory.md)>/system/artifacts` get the same permissions as users with the "View build runtime parameters and data" permission (in particular, with access to the values of all the password parameters used in the build).
* TeamCity agent computer administrators: same as "users who can change code that is used in the builds run by TeamCity".
* When [storing settings in VCS](storing-project-settings-in-version-control.md) is enabled:
  * Any user who can access the settings' repository (including users with "View file content" permission for the build configurations using the same VCS root) can see the settings and retrieve the actual passwords based on their stored scrambled form.
  * Any user who can modify settings in VCS for a single build configuration built on the server, via changing settings can retrieve artifacts and trigger builds from any build configuration they have only the view permission for ([TW-39192](https://youtrack.jetbrains.com/issue/TW-39192)).
  * Users who can customize build configuration settings on a per-build basis (for example, one who can run personal builds when versioned settings are set to "use settings from VCS") via changing settings in a build can retrieve artifacts and trigger builds from any build configuration they have only view permission for ([TW-46065](https://youtrack.jetbrains.com/issue/TW-46065)).

### Server and Data
{product="tc"}

__Update your TeamCity server regularly__.

We strongly recommend that you regularly update TeamCity to the [latest released version](https://www.jetbrains.com/teamcity/download/).

TeamCity will automatically notify you via the UI once a new update is available. You can also manually check for new TeamCity versions under __Server Administration | Updates__ for TeamCity itself and under __Server Administration | Plugins__ for any available plugin updates.

> To keep you ahead of the curve in preventing and mitigating security issues, TeamCity automatically downloads critical security updates. This approach helps to keep your system fortified against emerging risks and to swiftly tackle major vulnerabilities. Note that after an update is downloaded automatically, a system administrator still needs to approve its installation. See this article for more information: [](upgrading-teamcity-server-and-agents.md#Security+Patches).
>
{style="note"}

From a technical perspective, upgrades between bugfix releases within the same major/minor version are backwards compatible (for example, 2021.1.1 → 2021.1.2) and support relatively simple rollbacks. For all other major upgrades, we do our best to ensure that they run as smoothly as possible, though [backups](teamcity-data-backup.md) are strongly recommended for easy rollbacks.

From a licensing perspective, upgrades between bugfix releases are also safe. If your license covers 2021.2, then you will be able to upgrade to any 2021.2.x version.

__Protect the TeamCity Data Directory__.

Users who have read access to the [TeamCity Data Directory](teamcity-data-directory.md) can access all the settings on the server, including configured passwords. Hence, you need to make sure this directory is only readable by OS users who are actually administrators of the services.

The TeamCity Windows installer modifies permissions of the [TeamCity installation directory](teamcity-home-directory.md) not to use inheritable permissions and explicitly grants access to the directory to the Administrators user group and the account under which the service is configured to run. It is strongly recommended that you to restrict permissions to the [`TeamCity Data Directory`](teamcity-data-directory.md) in the same way.

__Protect your server machine__.

Limit access to the machine your TeamCity server runs on. Enable access logs and regularly review them.

In general, don't use the TeamCity server machine for running [build agents](build-agent.md) (at least under the user permitted to read the `<[TeamCity Home Directory](teamcity-home-directory.md)>` and `<[TeamCity Data Directory](teamcity-data-directory.md)>`).

The TeamCity server (and agents) processes run under users with minimal required [permissions](managing-roles-and-permissions.md). Installation directories are readable and writable only by a limited set of OS users. The `conf\buildAgent.properties` file and server logs as well as the [Data Directory](teamcity-data-directory.md) are only readable by OS users who represent administrators of the services, because reading those locations may allow taking over the agent or server respectively.

Note that binaries of the agent plugins installed on the server are available to anyone who can access the server URL.

__Use HTTPS everywhere__.

Access to the TeamCity web interface is [secured with HTTPS](using-https-to-access-teamcity-server.md) (for example, with the help a [proxy server](how-to.md#Configure+HTTPS+for+TeamCity+Web+UI) like NGINX). Make sure that best practices for securing web applications are employed for the TeamCity web interface: for example, that it is not possible to access the server using HTTP the protocol. The reverse proxy does not strip the _Referer_ request header.
{product="tc"}

To learn how to configure a connection between a TeamCity agent and the TeamCity server, see [this section](install-and-start-teamcity-agents.md#Agent-Server+Data+Transfer).

__Protect against DoS__.

TeamCity has no built-in protection against DoS (Denial-of-service) attack: high rate of requests can overload the server and make it non-responsive. If your TeamCity instance is deployed in the environment which allows such service abuse, implement the protection on the reverse-proxy level.

__Secure your database__.

Make sure to use a dedicated database user account with strong credentials for your TeamCity server's database schema. Consider encrypting the database if supported.

Consider adding the `teamcity.installation.completed=true` line into the `<[TeamCity Home Directory](teamcity-home-directory.md)>\conf\teamcity-startup.properties` file — this will prevent the server started with an empty database from granting access to the machine for the first coming user.

__Secure build files__.

The TeamCity development team makes a reasonable effort to fix any significant vulnerabilities (like cross-site scripting possibilities) once they are uncovered. Please note that any user who can affect build files ("users who can change code that is used in the builds run by TeamCity" or "TeamCity agent computer administrators") can make a malicious file available as a build artifact that will then exploit cross-site scripting vulnerability.

### Build Agents

__Run clean production builds__.

We recommend [enforcing clean checkout](clean-checkout.md) for your production builds to prevent tampering with the source code on an agent.

__Use disposable, network-protected build agents__.

Note that the builds running on the same build agent are not isolated, and they can potentially affect each other, which might give an opportunity to perform malicious actions.  
If possible, try using disposable, one-off build agents. The shorter the agent's lifetime, the smaller the chance of a compromise. Also make sure to use OS-dependent firewall rules to disable incoming network access to your cloud agents.

__Use agent pools for different projects__.

In general, it is recommended to distribute projects among agents, so that one TeamCity agent would not run builds of several projects whose developers and administrators should not get access to each other's projects.

If you run several agents on the same machine and do not [enable clean checkout](clean-checkout.md), beware that compromised agents or untrusted projects could potentially modify source code in the "neighbor" working directories. To mitigate this risk, consider running just one agent per machine and use different [agent pools](configuring-agent-pools.md) for different (private/public) projects. Make sure that the "Default" agent pool has no agents as a project can be assigned to the Default pool after a certain reconfiguration (that is when there is no other pool the project is assigned to).

__Control permissions of the OS user who runs a TeamCity agent__.

It is advised to run TeamCity agents under an OS account with only a [necessary set of permissions](system-requirements.md#Common+Requirements).

__Connect agents only to a trusted server__.
{product="tc"}

TeamCity agent is fully controlled by the TeamCity server: since TeamCity agents support automatic updates download from the server, agents should only connect to a trusted server. Note that an administrator of the server computer can force execution of arbitrary code on a connected agent.
{product="tc"}

### Version Control

__Use a recent version of Git__.

Make sure to always use the latest stable Operating System and Git version on your [build agents](build-agent.md). Update regularly.

__Properly manage your SSH keys__.

If you are using SSH keys to access your repositories, do not store them on your build agents. Instead, use TeamCity's [SSH key management](ssh-keys-management.md) to upload them to the TeamCity server.

Instead of disabling known hosts checks, make sure to maintain an [`.ssh/known_hosts`](ssh-agent.md#Using+Multiple+Keys+in+One+Build) file on the server and build agents for every host you are connecting to.

__Use a dedicated VCS user__.

If you are not using advanced features like [Kotlin DSL](kotlin-dsl.md) or, in general, if you don't need to commit to your repository as part of your build process, we recommend keeping a dedicated VCS user without write permissions to connect to your repositories.

### Artifact Storage
{product="tc"}

__Disable anonymous access__.

Regardless of where you [store your build artifacts](configuring-artifacts-storage.md) (such as S3), make sure to disable anonymous access to your storage location.

__Use proper access policies__.

Use proper access policies to protect your S3 or other storage locations / repositories for artifacts. Also use encryption if possible. Check, monitor, and regularly review the access logs of your storage locations.

__Don't put sensitive data into artifacts__.

Do not store credentials or other sensitive information as plain text in your builds' artifacts.

### Build History and Logs

__Keep your build history__.

Keep your build history and logs for a longer period of time, especially for builds doing critical deployments, by specifying corresponding [clean-up rules](teamcity-data-clean-up.md) for your project. Also, make sure to not grant the "remove build" [permissions](managing-roles-and-permissions.md) to developers, as this could prevent the archiving.

Both measures may help with tracing malicious activities, even if they happened a long time ago.

__Archive server and agent logs__.
{product="tc"}

Collect the [TeamCity server](teamcity-server-logs.md) and [build agent logs](viewing-build-agent-logs.md) in an archive and put them under a properly secured storage.
{product="tc"}

Collect [build agent logs](viewing-build-agent-logs.md) in an archive and put them under a properly secured storage.
{product="tcc"}

### Integrations

__Be careful when building public pull requests__.

If you build [pull requests](pull-requests.md) from unknown users or users outside your organization, be aware that pull requests could contain malicious code that would be run on your build agent.

Either disallow building public pull requests, or use separated, isolated, throw-away agents. TeamCity also offers a [built-in health report](server-health.md), which detects and reports pull request builds.

__Be aware of the security implications when using versioned settings__.

When you use [versioned settings](storing-project-settings-in-version-control.md) (Kotlin DSL, XML) and those settings are placed in the same repository as the source code, any malicious developer can potentially modify and leak project configuration settings. This could be done, for example, by adding a build step that prints out passwords or sends them somewhere as a file.

As an option, you could use a separate repository that only a limited number of users can commit to for your versioned settings.

__Be careful with third-party plugins__.
{product="tc"}

When [installing plugins](installing-additional-plugins.md), make sure they come from a trusted source and that their source code is available. Plugins can potentially access all information on a TeamCity server, including sensitive information.
{product="tc"}

[//]: # (Internal note. Do not delete. "How To...d160e890.txt")

## Encryption Used by TeamCity

TeamCity tries not to pass password values via the web UI (from a browser to the server) in clear text: instead, it uses RSA with 1024-bit key to encrypt them. However, it is recommended to use the TeamCity web UI only via HTTPS so this precaution should not be relevant. TeamCity stores passwords in the settings (where the original password value is necessary to perform authentication in other systems) in a scrambled form. The scrambling is done using 3DES with a fixed key, or [using a custom key](teamcity-configuration-and-maintenance.md#encryption-settings).
{product="tc"}

TeamCity tries not to pass password values via the web UI (from a browser to the server) in clear text: instead, it uses RSA with 1024-bit key to encrypt them. However, it is recommended to use the TeamCity web UI only via HTTPS so this precaution should not be relevant. TeamCity stores passwords in the settings (where the original password value is necessary to perform authentication in other systems) in a scrambled form. The scrambling is done using 3DES with a fixed key.
{product="tcc"}

## Third-party Software Vulnerabilities

This section describes the effect and necessary protection steps related to the announced security vulnerabilities of third-party software.

### Heartbleed, ShellShock

TeamCity distributions provided by JetBrains do not contain software/libraries and do not use technologies affected by Heart bleed and Shell shock vulnerabilities. What might still need assessment is the specific TeamCity installation implementation which might use the components behind those provided/recommended by JetBrains and which can be vulnerable to the mentioned exploits.

### POODLE

If you have configured an HTTPS access to the TeamCity server, inspect the solution used for HTTPS as that might be affected (for example, Tomcat seems to be [affected](https://cwiki.apache.org/tomcat/Security/POODLE)). At this point, none of the TeamCity distributions include HTTPS access by default and investigating/eliminating HTTPS-related vulnerability is out of scope of TeamCity.

Depending on the settings used, TeamCity server (and agent) can establish HTTPS connections to other servers (for example, Subversion). Depending on the server settings, those connections might fall back to using SSL 3.0 protocol. The recommended solution is not TeamCity-specific and it is to disable SSLv3 on the target SSL-server side.

### GHOST

CVE-2015-0235 vulnerability is found in the glibc library which is not directly used by the TeamCity code. It is used by the Java/JRE used by TeamCity under \*nix platforms. As Java is not bundled with TeamCity Unix distributions (Docker images use a newer glibc version), you should apply the security measures recommended by the vendor of the Java you use. At this time there are no related Java-specific security advisories released, so updating the OS should be enough to eliminate the risk of the vulnerability exploitation.

### FREAK

CVE-2015-0204 vulnerability is found in the OpenSSL implementation. TeamCity does not bundle any parts of the OpenSSL product and so is not vulnerable. You might still need to review the environment in which the TeamCity server and agents are set up, as well as the tools installed in addition to TeamCity for possible vulnerability mitigation steps.

### Apache Struts

TeamCity can download IntelliJ IDEA which contains JARs from both Apache Struts 1.x and Apache Struts 2.x. These JARs are exclusively used by the IntelliJ IDEA Struts plugin when IntelliJ IDEA collects inspections for a project on a TeamCity agent.

TeamCity does not employ Apache Struts for any HTTP request handling. As such, neither TeamCity server, nor TeamCity agent are affected by related Struts vulnerabilities (CVE-2016-1181, CVE-2017-5638, CVE-2023-50164, and others).

<seealso>
    <category ref="admin-guide">
        <a href="configuring-authentication-settings.md">Configuring Authentication Settings</a>
        <a href="teamcity-data-backup.md">TeamCity Data Backup</a>
        <a href="content-security-policy-in-teamcity.md">Content Security Policy in TeamCity</a>
        <a href="using-https-to-access-teamcity-server.md">Using HTTPS to Access TeamCity Server</a>
    </category>
    <category ref="howto">
        <a href="how-to.md#Set+Up+TeamCity+behind+a+Proxy+Server" product="tc">How to Set Up TeamCity behind a Proxy Server</a>
        <a href="how-to.md#Configure+HTTPS+for+TeamCity+Web+UI" product="tc">How to Configure HTTPS for TeamCity Web UI</a>
        <a href="how-to.md#Configure+TeamCity+to+Use+Proxy+Server+for+Outgoing+Connections" product="tc">How to Configure TeamCity to Use Proxy Server for Outgoing Connections</a>
        <a href="how-to.md#Configure+TeamCity+Agent+to+Use+Proxy+To+Connect+to+TeamCity+Server">How to Configure TeamCity Agent to Use Proxy To Connect to TeamCity Server</a>
        <a href="how-to.md#Personal+User+Data+Processing" product="tc">Personal User Data Processing</a>
    </category>
</seealso>