[//]: # (title: Security Notes)
[//]: # (auxiliary-id: Security Notes)

>The following notes are provided for a reference only and are not meant to be complete or accurate in their entirety.
>
{type="warning"}

TeamCity is developed with security concerns in mind, and reasonable efforts are made to ensure the system invulnerable to different types of attacks. We work with third parties on assessing TeamCity security using security scanners and penetration tests.   
We aim to promptly address newly discovered security issues in the nearest bugfix releases for the most recent TeamCity major version. It is recommended to upgrade to newly released TeamCity versions as soon as they become available as they can contain security-related fixes.  
However, the general assumption and __recommended setup is to deploy TeamCity in a trusted environment__ with no possibility for it to be accessed by malicious users.

Along with these guidelines, please review [notes](installing-and-configuring-the-teamcity-server.md#Configuring+Server+for+Production+Use) on configuring the TeamCity server for production use. For the list of disclosed security-related issues, see our [public issue tracker](https://youtrack.jetbrains.com/issues/TW?q=project:TeamCity%20type:%20%7BSecurity%20Problem%7D%20sort%20by:%20%7BFix%20versions%7D%20desc) and the "Security" section in the release notes.

## Security Self-Checklist

This checklist contains the main security recommendations to follow when using TeamCity:

* You are running the latest released TeamCity version and are ready to upgrade to the newly released versions within weeks.
* An access to the TeamCity web interface is secured with HTTPS (for example, with the help a [proxy server](how-to.md#Configure+HTTPS+for+TeamCity+Web+UI) like NGINX). Best practices for securing web applications are employed for the TeamCity web interface: for example, it is not possible to access the server using HTTP the protocol. The reverse proxy does not strip the _Referer_ request header.
* The TeamCity server machine does not run agents (at least under the user permitted to read the  `<[TeamCity server's home directory](teamcity-home-directory.md)>` and `<[TeamCity Data Directory](teamcity-data-directory.md)>`).
* TeamCity server and agents processes run under users with minimal required permissions. Installation directories are readable and writable only by a limited set of OS users. The `conf\buildAgent.properties` file and server logs as well as the Data Directory are only readable by OS users who represent administrators of the services, because reading those locations may allow taking over the agent or server respectively.
* Guest user and user registration is disabled or roles are reviewed for guests and the [All Users](user-group.md#%22All+Users%22+Group) group.
* TeamCity users with administrative permissions have non-trivial passwords.
* If you have an external authentication configured (such as LDAP), the [built-in authentication](configuring-authentication-settings.md#Built-in+Authentication) module is disabled.
* Passwords are not printed into the build log, not stored in build artifacts, nor are they stored in non-[password parameters](typed-parameters.md#Adding+Parameter+Specification).

<anchor name="HowTo...-Security-relatedRisksEvaluation"/>

## Security Risks Evaluation

Here are the notes on different security-related aspects:
* [Man-in-the middle attack](https://en.wikipedia.org/wiki/Man-in-the-middle_attack) concerns:
    * Between the TeamCity server and the user's web browser: It is advised to [use HTTPS](using-https-to-access-teamcity-server.md) for the TeamCity server. During login, TeamCity transmits the user login password in an encrypted form with a moderate encryption level.
    * Between a TeamCity agent and the TeamCity server: see [this section](setting-up-and-running-additional-build-agents.md#Agent-Server+Data+Transfers).
    * Between the TeamCity server and other external servers (version control, issue tracker, and so on): the general rules apply as for a client (the TeamCity server in this case) connecting to the external server, see the guidelines for each specific server.
* Users who have access to the TeamCity web UI: the specific information accessible to the user is defined via TeamCity [user roles](role-and-permission.md).
* Users who can change the code that is used in the builds run by TeamCity (including committers in any branches/pull requests if they are built on TeamCity):
    * can do everything the system user, under whom the TeamCity agent is running, can do; have access to OS resources and other applications installed on the agent machines where their builds can run.
    * can access and change source code of other projects built on the same agent, modify the TeamCity agent code, publish any files as artifacts for the builds run on the agent (which means the files can be then displayed in the TeamCity web UI and expose web vulnerabilities or can be used in other builds), and so on.
    * can impersonate a TeamCity agent (run a new agent looking the same to the TeamCity server).
    * can do everything that users with the "View build configuration settings" permission for all the projects on the server can do (see [below](#view-build-config-settings)).
    * can retrieve settings of the build configurations where the builds are run, including the values of the password fields.
    * can download artifacts from any build on the server.   
      Hence, it is advised to run TeamCity agents under an OS account with only a [necessary set of permissions](setting-up-and-running-additional-build-agents.md#Necessary+OS+and+environment+permissions) and use the [agent pools](configuring-agent-pools.md) feature to ensure that projects requiring a different set of access are not built on the same agents.
<anchor name="view-build-config-settings"/>
* Users with the "View build configuration settings" permission (by default, the Project Developer role) can view all the projects on the server.
* Users with the "Edit project" permission (the "Project Administrator" TeamCity role by default) in one project, by changing settings can retrieve artifacts and trigger builds from any build configuration they have only the view permission for ([TW-39209](https://youtrack.jetbrains.com/issue/TW-39209)).
* Users with the "Change server settings" permission (the "System Administrator" TeamCity role by default): It is assumed that the users also have access to the computer on which the TeamCity server is running under the user account used to run the server process. Thus, the users can get full access to the machine under that OS user account: browse file system, change files, run arbitrary commands, and so on.
* The TeamCity server computer administrators: have full access to TeamCity stored data and can affect TeamCity executed processes. Passwords that are necessary to authenticate in external systems (like VCS, issue trackers, and so on) are stored in a scrambled form in [TeamCity Data Directory](teamcity-data-directory.md) and can also be stored in the database. However, the values are only scrambled, which means they can be retrieved by any user who has access to the server file system or database.
* Users who have read access to the TeamCity server logs (TeamCity server home directory) can escalate their access to the TeamCity server administrator.
* Users who have read access to `<[TeamCity Data Directory](teamcity-data-directory.md)>` can access all the settings on the server, including configured passwords.
* Users who have read access to the build artifacts in `<[TeamCity Data Directory](teamcity-data-directory.md)>/system/artifacts` get the same permissions as users with the "View build runtime parameters and data" permission (in particular, with access to the values of all the password parameters used in the build).
* TeamCity agent computer administrators: same as "users who can change code that is used in the builds run by TeamCity".
* It is recommended to distribute projects among agents, so that one TeamCity agent would not run builds of several projects whose developers and administrators should not get access to each other's projects. The recommended way to distribute projects is to use the [agent pools](configuring-agent-pools.md) feature and make sure that the "Default" agent pool has no agents as a project can be assigned to the Default pool after a certain reconfiguration (that is when there is no other pool the project is assigned to).
* When [storing settings in VCS](storing-project-settings-in-version-control.md) is enabled:
    * Any user who can access the settings repository (including users with "View file content" permission for the build configurations using the same VCS root) can see the settings and retrieve the actual passwords based on their stored scrambled form.
    * Any user who can modify settings in VCS for a single build configuration built on the server, via changing settings can retrieve artifacts and trigger builds from any build configuration they have only view permission for ([TW-39192](https://youtrack.jetbrains.com/issue/TW-39192)).
    * Users who can customize build configuration settings on a per-build basis (for example, one who can run personal builds when versioned settings are set to "use settings from VCS") via changing settings in a build can retrieve artifacts and trigger builds from any build configuration they have only view permission for ([TW-46065](https://youtrack.jetbrains.com/issue/TW-46065)).
* Other notes:
    * TeamCity web application vulnerabilities: the TeamCity development team makes a reasonable effort to fix any significant vulnerabilities (like cross-site scripting possibilities) once they are uncovered. Please note that any user who can affect build files ("users who can change code that is used in the builds run by TeamCity" or "TeamCity agent computer administrators") can make a malicious file available as a build artifact that will then exploit cross-site scripting vulnerability. ([TW-27206](http://youtrack.jetbrains.com/issue/TW-27206))
    * TeamCity agent is fully controlled by the TeamCity server: since TeamCity agents support automatic updates download from the server, agents should only connect to a trusted server. An administrator of the server computer can force execution of arbitrary code on a connected agent.
    * Binaries of the agent plugins installed on the server are available to anyone who can access the server URL.
    * If any of the OAuth [authentication modules](configuring-authentication-settings.md) (Bitbucket Cloud, GitHub.com, GitHub Enterprise, GitLab.com, GitLab CE/EE) are enabled on your server __and__ you restrict authentication to members of a specific Bitbucket workspace, GitHub organization, or GitLab group, note the following:  
      Once signed in to the TeamCity server with an external account, a user can create a password or token which will allow them to sign in to this server directly, bypassing the VCS hosting provider verification. If you delete a user from a workspace/organization/group, remember to restrict their access or delete their user profile in TeamCity as well.

[//]: # (Internal note. Do not delete. "How To...d160e890.txt")

## Restricting Permissions to TeamCity Data Directory

__Since TeamCity 2017.2__, the TeamCity Windows installer modifies permissions of the [TeamCity installation directory](teamcity-home-directory.md) not to use inheritable permissions and explicitly grants access to the directory to the Administrators user group and the account under which the service is configured to run.   
It is strongly recommended to restrict permissions to the [`TeamCity Data Directory`](teamcity-data-directory.md) in the same way.

## Preventing First-User Access to Empty Database

Consider adding the `teamcity.installation.completed=true` line into the `<[TeamCity Home Directory](teamcity-home-directory.md)>\conf\teamcity-startup.properties` file - this will prevent the server started with the empty database from granting access to the machine for the first coming user.

## DoS Attacks Note

TeamCity has no built-in protection against DoS (Denial-of-service) attack: high rate of requests can overload the server and make it irresponsive. If your TeamCity instance is deployed in the environment which allows such service abuse, implement the protection on the reverse-proxy level.

## Encryption Used by TeamCity

TeamCity tries not to pass password values via the web UI (from a browser to the server) in clear text: instead, it uses RSA with 1024-bit key to encrypt them. However, it is recommended to use the TeamCity web UI only via HTTPS so this precaution should not be relevant. TeamCity stores passwords in the settings (where the original password value is necessary to perform authentication in other systems) in a scrambled form. The scrambling is done using 3DES with a fixed key, or using a custom key.

## Third-party Software Vulnerabilities

This section describes the effect and necessary protection steps related to the announced security vulnerabilities of third-party software.

### Heartbleed, ShellShock

TeamCity distributions provided by JetBrains do not contain software/libraries and do not use technologies affected by Heart bleed and Shell shock vulnerabilities. What might still need assessment is the specific TeamCity installation implementation which might use the components behind those provided/recommended by JetBrains and which can be vulnerable to the mentioned exploits.

### POODLE

If you have configured an HTTPS access to the TeamCity server, inspect the solution used for HTTPS as that might be affected (for example, Tomcat seems to be [affected](http://wiki.apache.org/tomcat/Security/POODLE)). At this point, none of the TeamCity distributions include HTTPS access by default and investigating/eliminating HTTPS-related vulnerability is out of scope of TeamCity.

Depending on the settings used, TeamCity server (and agent) can establish HTTPS connections to other servers (for example, Subversion). Depending on the server settings, those connections might fall back to using SSL 3.0 protocol. The recommended solution is not TeamCity-specific and it is to disable SSLv3 on the target SSL-server side.

### GHOST

CVE-2015-0235 vulnerability is found in the glibc library which is not directly used by the TeamCity code. It is used by the Java/JRE used by TeamCity under \*nix platforms. As Java is not bundled with TeamCity Unix distributions (Docker images use a newer glibc version), you should apply the security measures recommended by the vendor of the Java you use. At this time there are no related Java-specific security advisories released, so updating the OS should be enough to eliminate the risk of the vulnerability exploitation.

### FREAK

CVE-2015-0204 vulnerability is found in the OpenSSL implementation. TeamCity does not bundle any parts of the OpenSSL product and so is not vulnerable. You might still need to review the environment in which the TeamCity server and agents are set up, as well as the tools installed in addition to TeamCity for possible vulnerability mitigation steps.

### Apache Struts

CVE-2017-5638 affects Jakarta Multipart parser in Apache Struts. CVE-2016-1181 also affects multipart requests processing in some older versions of Apache Struts.

TeamCity bundles IntelliJ IDEA which contains JARs from both Apache Struts 1.x and Apache Struts 2.x. These JARs are only used by IntelliJ IDEA Struts plugin when IntelliJ IDEA collects inspections for a project on a TeamCity agent.

Under no circumstances these versions of Apache Struts are used to handle any HTTP requests. Thus neither TeamCity server, nor TeamCity agent are affected by these vulnerabilities.

<seealso>
    <category ref="admin-guide">
        <a href="configuring-authentication-settings.md">Configuring Authentication Settings</a>
        <a href="teamcity-data-backup.md">TeamCity Data Backup</a>
        <a href="content-security-policy-in-teamcity.md">Content Security Policy in TeamCity</a>
        <a href="using-https-to-access-teamcity-server.md">Using HTTPS to Access TeamCity Server</a>
    </category>
    <category ref="howto">
        <a href="how-to.md#Set+Up+TeamCity+behind+a+Proxy+Server">How to Set Up TeamCity behind a Proxy Server</a>
        <a href="how-to.md#Configure+HTTPS+for+TeamCity+Web+UI">How to Configure HTTPS for TeamCity Web UI</a>
        <a href="how-to.md#Configure+TeamCity+to+Use+Proxy+Server+for+Outgoing+Connections">How to Configure TeamCity to Use Proxy Server for Outgoing Connections</a>
        <a href="how-to.md#Configure+TeamCity+Agent+to+Use+Proxy+To+Connect+to+TeamCity+Server">How to Configure TeamCity Agent to Use Proxy To Connect to TeamCity Server</a>
        <a href="how-to.md#Personal+User+Data+Processing">Personal User Data Processing</a>
    </category>
</seealso>