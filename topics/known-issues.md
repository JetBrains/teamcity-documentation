[//]: # (title: Known Issues)
[//]: # (auxiliary-id: Known Issues)

This page contains a list of workarounds for known issues in TeamCity.

To see issues specific to particular versions of TeamCity, go to the respective section of [upgrade notes](upgrade-notes.md).
{product="tc"}

<anchor name="jdk8_240"/>

## Incompatibility with JDK 8 update 242+
{product="tc"}

TeamCity versions up to 2019.2.1 running under JDK 8u242+ can report `java.lang.NoClassDefFoundError: Could not initialize class XXX` errors, for example, on Git operations or Windows domain authentication operations.

Until TeamCity 2019.2.2 is released, it is recommended to use Java 8u232 version for TeamCity server.

## Agent running as Windows Service Limitations

When a TeamCity build agent is installed as a Windows service, there may appear various "Permission denied" or "Access denied" errors during the build process, see details below.

### Security-related issues
{product="tc"}

The user account used by the service is required to have sufficient permissions to perform the build and [manage the service](setting-up-and-running-additional-build-agents.md). If you run the TeamCity agent service under the SYSTEM account, do the following:
1. [Change](setting-up-and-running-additional-build-agents.md#Changing+Communication+Protocol) SYSTEM for a usual user account with necessary permissions granted.
2. Restart the service.

### Windows service limitations

As a Windows service, the TeamCity agent and the build processes are not able to access network shares and mapped drives.

To overcome these restrictions, run TeamCity agent [via console](setting-up-and-running-additional-build-agents.md#Starting+the+Build+Agent).

#### Issues with automated GUI and browser testing

These problems include errors running tests headless, issues with the interaction of the TeamCity agent with the Windows desktop, and so on.

<tip>

Note that there is a Windows limitation to accessing a remote computer via mstsc: the desktop of the remote machine will be locked on RDP disconnect, which will cause issues running tests. The VNC protocol allows you to remote control another machine without locking it.To run GUI tests and be able to use RDP, see the [workaround](#Running+automated+GUI+tests+and+using+RDP) below.
</tip>

To resolve / avoid these:
1. Run TeamCity agent [via console](setting-up-and-running-additional-build-agents.md#Starting+the+Build+Agent).
2. Configure the build agent machine not to launch a screensaver locking the desktop.
3. Configure the TeamCity agent to start [automatically](setting-up-and-running-additional-build-agents.md#Starting+the+Build+Agent) (for example, configure an automatic user logon on Windows start and then configure the TeamCity agent start (via `agent.bat start`) on the user logon).
    
For graphical tests the build agent cannot be started as a service and it is recommended to configure the build agent launch with a 1 minute delay after the user auto-logon, e.g. using the `bin\agent.bat start` command in the task scheduler and configuring the delay there.

#### Running automated GUI tests and using RDP 

RDP uses its own video driver overriding the one from the machine's video card for the session. Redirecting the session to console will unload the Windows graphical drivers. This can be done  by adding the following step to your build configuration prior to your tests (the example below is for PowerShell, but other languages (DOS, Python) can be used too):


```Shell
$sessionInfo=((quser $env:USERNAME | select -Skip 1) -split '\s+')
if ($sessionInfo[1] -like "rdp-tcp*") { tscon $sessionInfo[2] /dest:console }
```



where `quser [current username]` lists all the connections to that machine for the user, either console or graphical.  
The one listed as `rdp-tcp#*` is the remote desktop connection which can be redirected to the console using `tscon [connection id] /dest:console`.

 

<note>

An unsupervised computer with a running desktop permanently logged into a user session might be considered a network security threat, as access to it can be difficult to trace. Therefore, it is recommended to run automated GUI and browser tests on a virtual machine isolated from sensitive corporate network resources, e.g. on a machine not included in a Windows domain.
</note>

#### Issues with .NET Selenium 

When a TeamCity agent is started as a Windows service and automated tests for .NET applications use Selenium WebDriver, the tests may fail due to browser drivers limitations. As a solution, consider starting the agent [manually](setting-up-and-running-additional-build-agents.md#Manual+Start).


### Early start of the service before other resources are initialized

To handle this, consider using the __Automatic (Delayed Start)__ option of the service settings or configure [service dependencies](http://youtrack.jetbrains.com/issue/TW-32987#comment=27-608269).

 For more investigation steps, see the [Common Problems](common-problems.md#Build+works+locally+but+fails+or+misbehaves+in+TeamCity) page.

## java.lang.OutOfMemoryError: unable to create new native thread error

If your TeamCity server is running on SUSE® Linux Enterprise (or using systemd Daemon), the __java.lang.OutOfMemoryError: unable to create new native thread error__ may be caused by the [cgroup process number controller](https://www.kernel.org/doc/Documentation/cgroup-v1/pids.txt) feature limiting the number of processes and the amount of threads in a cgroup to 512 by default.

Increasing the limit (e.g. to 4096) on the TeamCity server should solve the issue.

See also this [external posting](https://www.elastic.co/blog/we-are-out-of-memory-systemd-process-limits).

## Clearing Browser Caсhes

There is a web UI-related issue which some users have encountered (and it cannot be reproduced on other computers) which is tied to the cached versions of content. If you have come across such problem, make sure your browser does not use cached versions of content by [clearing browser caches](http://en.wikipedia.org/wiki/Wikipedia:Bypass_your_cache).

## Logging with Log4j in Your Tests

If you use Log4j logging in your tests, in some cases you may miss the Log4j output from your logs. In such cases do the following:
* Use Log4j 1.2.12
* For Log4j 1.2.13\+, add the `Follow=true` parameter for the console appender used in Log4j configuration:

    ```Shell
    <appender name="CONSOLE" class="org.apache.log4j.ConsoleAppender">
      <param name="Follow" value="true"/>
    </appender>
    ```

## Agent Service Can Exit on User Logout under Windows x64

The used version of [Java Service Wrapper](http://wrapper.tanukisoftware.org/) does not fully support Windows 64 and this causes agent launcher process to be killed on user logout. The agent itself will be function until the next restart (server upgrade or agent properties change).


## Failed Build Can be Reported as a Successful One With Maven 2.0.7

This is a known bug in this version of Maven. Consider using any later version.  
In case it's not possible, you can patch `mvn.bat` yourself by replacing the fragment at line 148 of `mvn.bat`:


```Shell
:error
set ERROR_CODE=1

```

with the following one:

```Shell
:error
if "%OS%"=="Windows_NT" @endlocal
set ERROR_CODE=1

```

## Conflicting Software

Most common indicators of conflicting software are errors like "Access is denied", "Permission denied" or _java.io.FileNotFoundException_ mentioning the file that is present and is writable by the user the agent/build runs under. Also, certain software running in the background (like antivirus) can significantly slow down such build agent operations as sources checkout, artifact publishing or even build running.

Certain antivirus software like Kaspersky Internet Security can result in Java process crashes or other misbehavior like inability to access files. For example, see [this issue](http://jetbrains.net/tracker/issue/TW-7138).

ESET antivirus can also slow down Ant/IntelliJ IDEA project builds a great deal (slowing down TCP connections to localhost on an agent).

If you run antivirus on the TeamCity server or agent machines and get disk access errors or experience degraded performance, you may consider temporarily disabling the antivirus software before investigating the issue and reporting it to JetBrains. Note that disabling the antivirus can make your setup more vulnerable to potential external attacks — ensure you take proper security measures before doing this.
{product="tc"}

If you run antivirus on the TeamCity agent machines and get disk access errors or experience degraded performance, you may consider temporarily disabling the antivirus software before investigating the issue and reporting it to JetBrains. Note that disabling the antivirus can make your setup more vulnerable to potential external attacks — ensure you take proper security measures before doing this.
{product="tcc"}

It is recommended to exclude entire TeamCity server home and [TeamCity Data Directory](teamcity-data-directory.md) from the background checks and perform periodical checks there in the well-known maintenance window so that those do not affect server performance much. On TeamCity agent, it is recommended to exclude TeamCity agent home from the background checks.
{product="tc"}

There might be problems with the Windows Indexing Service, so disable various indexing services. See [the related issue](http://youtrack.jetbrains.net/issue/TW-10033#comment=27-82484) for more details. Windows System Restore Feature might also need disabling.

Do not install software with background indexing like WinCVS, TortoiseCVS, TortoiseSVN, and other Tortoise\* products. This applies to server and also to agents if you use agent-side checkout.

Skype software is known to corrupt layout of pages displayed in Internet Explorer. ([TW-13052](http://youtrack.jetbrains.net/issue/TW-13052)).

## Subversion issues

### svn: E175002: Received fatal alert: bad_record_mac
{product="tc"}

Add a system property `-Dsvnkit.http.sslProtocols=SSLv3,TLS` on the build server (see [Configuring TeamCity Server Startup Properties](configuring-teamcity-server-startup-properties.md)).   
If you use checkout on agent, add this property [on build agent](configuring-build-agent-startup-properties.md) as well.

### Subversion-related JVM Crashes

If JVM crashes while executing SVN-related code (e.g. under org.tmatesoft.svn package), you can try to disable it using one of the options:
* Passing `-Dsvnkit.useJNA=false` JVM option to the crashing process (server or agent)
* Making NTLM support less prioritative by passing the `-Dsvnkit.http.methods=Basic,Digest,NTLM` JVM option.  
Anyway, upgrading the JVM used to the [latest available version](http://java.sun.com/javase/downloads) is recommended.


## NUnit 2.4.6 Performance
{product="tc"}

Due to an issue in NUnit 2.4.6, its performance may be slower than NUnit 2.4.1. For additional information, refer to the corresponding issue in our issue tracker: [TW-4709](http://www.jetbrains.net/tracker/issue/TW-4709)

## StarTeam Performance
{product="tc"}

Using StarTeam SDK 9.0 instead of StarTeam SDK 9.3 on the TeamCity server can significantly improve VCS performance when there is a slow connection between TeamCity and StarTeam servers.

## Perforce 2009.2 Performance on Windows

If you run Perforce 2009.2 on Windows you may experience significant slow down. This is an issue with P4 server running on Windows. Refer to corresponding [section](http://kb.perforce.com/article/1175/20092-server-performance-on-windows) in Perforce documentation.

## Wrong times for build scheduled triggering (Timezone issues)
{product="tc"}

Make sure you use the latest update for Java 8 installation available for your platform (e.g. OpenJDK 8 by [Amazon Corretto](https://aws.amazon.com/corretto/)).


## Upgrading IntelliJ IDEA May Affect Active Pre-Tested Commits

Before you upgrade to IntelliJ IDEA X (or other IntelliJ X platform products), make sure you do not have active pre-tested commits, otherwise they will not be able to be committed after upgrade. This is only relevant if you use directory-based IDEA project (project files are stored under `.idea` directory).

## Other Java Applications Running on the Same Server
{product="tc"}

If other web applications are available via the same hostname, a session cookie conflict can occur. This usually is visible via random user logouts or losing session-level data. (e.g. [TW-12654](http://youtrack.jetbrains.net/issue/TW-12654)). To resolve this, you can use different host names when accessing the applications.

## The Server Does Not Start Claiming the Database is in Use
{product="tc"}

Only a single TeamCity server can work with one database, which is checked on the TeamCity server start.
 
"`The Database is in Use`" error on the start\-up is reported in either of the following cases:
* An attempt to start more than one TeamCity server connected to the same database
* A second TeamCity instance detected
* The internal HSQL database is being used by another application

The error is most probably caused by the fact that there is another running TeamCity installation which is connected to the same database. Сheck that the [database properties](setting-up-an-external-database.md) are correct and there is no other TeamCity server using the same database.

## Slow download from TeamCity server
{product="tc"}

If you experience slow speed when downloading artifacts from TeamCity, try checking the speed on the server machine, downloading from localhost. 
If the speed is OK for the localhost, the issue can be in the network configuration or OS/hardware settings when combined with TeamCity (Tomcat) settings.

Make sure \<[TeamCity Home](teamcity-home-directory.md)\>\conf\server.xml file corresponds to the file included in TeamCity distribution (can be checked in .tar.gz distribution). 
If you have the following "Connector" node (ports numbers can be different):


```Shell
<Connector port="8111" protocol="HTTP/1.1"
               connectionTimeout="60000"
               redirectPort="8543"
               useBodyEncodingForURI="true"
    />
```



change it to:


```Shell
<Connector port="8111" protocol="org.apache.coyote.http11.Http11NioProtocol"
                connectionTimeout="60000"
                redirectPort="8543"
                useBodyEncodingForURI="true"
                socket.txBufSize="64000"
                socket.rxBufSize="64000"
                tcpNoDelay="1"
                                />
```



and restart TeamCity server.

If this does not help with the download speed, to investigate the case, you might need to find an administrator with appropriate network\-related issues investigation skills to look into the case.

## Failure to publish artifacts to server behind IIS reverse proxy

This problem is only relevant to configurations that involve an IIS reverse proxy between the build server and agents. Sometimes a build agent can be found in infinite loop trying to publish build artifacts to the server. The build log looks like this:


```Shell
[11:15:05]Publishing artifacts
[11:15:05][Publishing artifacts] Collecting files to publish: [toZip/** => artifact.zip]
[11:15:06][Publishing artifacts] Creating archive artifact.zip (9s)
[11:15:06][Creating archive artifact.zip] Creating C:\BuildAgent\temp\buildTmp\ZipPreprocessor2847146024236637664\artifact.zip
[11:15:15][Creating archive artifact.zip] Archive was created, file size 32142324 bytes
[11:15:15][Publishing artifacts] Sending toZip/**
[11:15:25][Publishing artifacts] Sending toZip/**
[11:15:39][Publishing artifacts] Sending toZip/**
[11:15:48][Publishing artifacts] Sending toZip/**
[11:16:01][Publishing artifacts] Sending toZip/**
[11:16:16][Publishing artifacts] Sending toZip/**

```
meanwhile, `teamcity-agent.log` is filled with 404 responses from IIS:

```Shell
[2012-08-01 12:04:55,514]   WARN -    jetbrains.buildServer.AGENT - <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<meta http-equiv="Content-Type" content="text/html; charset=iso-8859-1"/>
<title>404 - File or directory not found.</title>
<style type="text/css">
<!--
body{margin:0;font-size:.7em;font-family:Verdana, Arial, Helvetica, sans-serif;background:#EEEEEE;}
fieldset{padding:0 15px 10px 15px;}

```

The most common cause for this is `maxAllowedContentLength` setting (in IIS) is either

* set to too small value, or
* left unconfigured and so defaults to 30000000 bytes (&lt;30 Mb)

So any artifact larger than `maxAllowedContentLength` is discarded by IISCheck the settings value and try to rerun your build

## SSL problems when connecting to HTTPS from TeamCity (handshake alert: unrecognized_name)
{product="tc"}

This problem may happen when changing JVM from 1.6 to 1.7 and connecting some incorrectly configured HTTPS servers. 
The problem and workaround for it are described in [this issue](http://youtrack.jetbrains.com/issue/TW-30210).

[//]: # (Internal note. Do not delete. "Known Issuesd193e619.txt")
 

## SSL problems connecting MS SQL Server and TeamCity
{product="tc"}

Since MS SQL Server always establishes an SSL connection between the JDBC client (the TeamCity application) and the server, connection problems may occur (_SQL exception: Connection reset_). 

__Affected  MS SQL versions__ 

Any version _prior to the ones listed below_:
* SQL Server 2012 Production version and later
* SQL Server 2008 Service Pack 3 Cumulative Update 4
* SQL Server 2008R2 Service Pack 1 Cumulative Update 6
* SQL Server 2008R2 Service Pack 2

__Cause__: The problem is caused by the Java 1.6 update addressing [this security vulnerability](https://www.cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2011-3389).

__Solution__: Microsoft SQL Server upgrade is recommended.

__Workarounds:__ If Microsoft SQL Server upgrade is not possible for some reason, TeamCity can be set up to use older Microsoft SQL Server versions with the database connection still SSL- or TLS-encrypted.

<warning>

Any of the workarounds listed below will make the connection between TeamCity and the database server vulnerable. Make sure to take proper security measures before trying them.
</warning>

* Continue using a block cipher such as `AES_128_CBC` or `3DES_EDE_CBC`, but disable CBC protection via `-Djsse.enableCBCProtection=false` Java command-line option (that can be added to `TEAMCITY_SERVER_OPTS` environment variable, as described [here](configuring-teamcity-server-startup-properties.md#JVM+Options).  
    The `jsse.enableCBCProtection` Java system property is also available in all _OpenJDK_ 8 versions and _IBM J9_ [8.0.0 SR1](https://www.ibm.com/support/knowledgecenter/SSYKE2_8.0.0/com.ibm.java.security.component.80.doc/security-component/jsse2Docs/beast.html) and later.   
    Secure connection between _TeamCity_ and _Microsoft SQL Server_ would be stable but still vulnerable to [CVE-2011-3389](https://www.cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2011-3389) also known as _BEAST_.
* Fall back to a stream cipher (which is not susceptible to _BEAST_) such as `RC4_128`. This will render the connection vulnerable to [CVE-2015-2808](https://cve.mitre.org/cgi-bin/cvename.cgi?name=cve-2015-2808).
* Consider running with the temporarily disabled antivirus software, but ensure this doesn't compromise the security of your setup. For example, see [this issue](http://jetbrains.net/tracker/issue/TW-7138).

## Distorted Configuration Window During Agent Reinstallation

When installing a TeamCity agent via a [Windows agent installer](setting-up-and-running-additional-build-agents.md#Installing+via+Windows+installer) on top of the already installed agent with a different version of Java, the "_Configure Build Agent Properties_" installation window might appear distorted.

__This issue has been fixed in TeamCity 2019.1.5__.

To workaround this issue without upgrading to 2019.1.5, uninstall the previously installed agent version before installing a new agent into the same directory.   
To uninstall the agent, invoke `Uninstall.exe` in the [Agent Home Directory](agent-home-directory.md), clear all the "_Remove..._" checkboxes to keep the agent logs and configuration, and click __Uninstall__. After the successful uninstallation, you can proceed with installing the new agent version via the agent installer.

## Windows Docker Containers
{product="tc"}

Problems common to TeamCity Docker container images.

* Since Windows 10 version 1803 with [KB4340917](https://support.microsoft.com/en-us/help/4340917/windows-10-update-kb4340917) it's possible to use port mapping from containers to localhost. For previous Windows versions it works for the non\-localhost IP address associated with this machine and you can access a running application via the machine's hostname or determine the IP address via the `ipconfig` command. Note that the `netstat -an` command may not show that the port is open on any IP address, while in fact it can work. This is also a known problem of Docker on Windows.

* On Windows 10, the memory allocated per container is 1GB by default. To increase this value, use the following memory options:

    ```Shell
        docker run ... -m 2GB -e TEAMCITY_SERVER_MEM_OPTS="-Xmx2g -XX:ReservedCodeCacheSize=350m"
        
    ```

* On Windows 10 containers work via Hyper-V and may experience problems with network and other subsystems. To diagnose these problems, execute the following PowerShell script:

    ```Shell
        Invoke-WebRequest https://aka.ms/Debug-ContainerHost.ps1 -UseBasicParsing | Invoke-Expression
        
    ```

* When starting a TeamCity server from a Windows Docker image, make sure to grant "_Authenticated Users_" the _Full Control_ over the directories used as volumes. [See the related issue](https://github.com/docker/for-win/issues/1058).

* When starting a Windows Docker container with the directory `C:/BuildAgent/work` mapped as a volume to the container host, Git for Windows fails with a following error: "_Invalid path '/ContainerMappedDirectories': No such file or directory_". The workaround is not to add `C:/BuildAgent/work` as a volume.

To analyze the script output, refer to the [following documents](https://github.com/MicrosoftDocs/Virtualization-Documentation/tree/master/windows-server-container-tools/Debug-ContainerHost). If it shows that there are problems with the container network subsystem, try resetting it using the [clean-up scripts](https://github.com/MicrosoftDocs/Virtualization-Documentation/tree/master/windows-server-container-tools/CleanupContainerHostNetworking).

More details on troubleshooting Docker for Windows are available in the [Docker](https://docs.docker.com/docker-for-windows/troubleshoot/) and [Microsoft](https://docs.microsoft.com/en-us/virtualization/windowscontainers/troubleshooting) documentation.

### Information about installed Docker server OS on Windows missing on Agent

On Windows 10, the Docker server depends on Hyper-V service and its start may take a significant amount of time. To resolve the issue, configure the TCBuildAgent Windows service to depend on the Docker for Windows Service, `com.docker.service` by default.

### Linux Docker Containers under Windows

The [Docker Wrapper](docker-wrapper.md) works on Windows when Windows-based containers are started.

If a Linux container is started on a Windows machine, TeamCity displays the error message "Starting Linux Docker containers under Windows is not supported. To avoid this problem, add the [`teamcity.agent.jvm.os.name`](integrating-teamcity-with-docker.md#Parameters+Reported+by+Agent) does not contain Windows [agent requirement](agent-requirements.md).

If you need to support a use case when the Docker wrapper runs Linux containers under Windows platform, vote for the related comment in [TW-51820](https://youtrack.jetbrains.com/issue/TW-51820).

### Problems with a local time in Windows containers

When using Windows Docker containers, there is an issue with [incorrect time in Windows containers](https://youtrack.jetbrains.com/issue/TW-53474) when the system time in a container goes out of sync with the time on the host machine. It could cause problems in integrations where response time is significant (for example, OAuth tokens).

To address it, upgrade your host machine to Windows Server 2019 / Windows 10 1809 and use TeamCity docker images [compatible with Windows containers 1809](https://youtrack.jetbrains.com/issue/TW-58161).

### "Access is denied" or "Access to the path is denied" problem on container start

When Docker is starting Windows containers with __process isolation__, it uses a Windows user account which lacks the write access to the directory with Docker volumes. In this case, build agents may fail to start due to the "_Access to the path is denied_" or "_Access is denied_" error.

To resolve this issue, grant the "Full control" permission to the "Authenticated Users" group for the `%\PROGRAMDATA%\docker\volumes` directory.

## dotCover known issues

### dotCover does not support Windows Nano Server

If you try to run dotCover on an agent with the Nano Server OS, the build will fail with an exit error "_Process exited with code -1073741515_". Instead of Nano Server, consider using [Server Core](https://docs.microsoft.com/en-us/windows-server/administration/server-core/what-is-server-core) which is an alternative minimal installation option of Windows Server.

### dotCover does not support сoverage statistics for msbuild

dotCover does not support collection of coverage statistics for the `dotnet msbuild /t:vstest` command — use `dotnet test` instead.

### Code coverage configuration using Test Settings is deprecated

Code coverage configuration using Test Settings is deprecated in dotCover. [Read more](https://docs.microsoft.com/en-us/previous-versions/dd504821(v=vs.140)) in Microsoft documentation.

If you use `.testsettings` files, TeamCity will display a warning message in the log. To prevent this issue, we suggest that you use [`.runsettings`](https://docs.microsoft.com/en-us/visualstudio/test/configure-unit-tests-by-using-a-dot-runsettings-file?view=vs-2019) files instead. If, for some reason, you have to continue using `.testsettings`, install dotCover version 2019.1.x or earlier on TeamCity agents, as described [here](installing-agent-tools.md).

## Xcode 10 is unable to clean artifacts in custom output directory

If you use a custom output directory for Xcode, note that on upgrading to Xcode 10, TeamCity builds with the Xcode Project build runner might fail with the error: "_Could not delete \<directory\> because it was not created by the build system and it is not a subfolder of derived data."_

This is caused by the following Xcode 10 known issue:   
When performing `xcodebuild clean` on a project that uses a customized build location outside the derived data directory, and that has older build products produced prior to Xcode 10, Xcode might report an error indicating that it won't delete directories not created by the new build system. (40427159)   
See [Xcode documentation](https://developer.apple.com/documentation/xcode_release_notes/xcode_10_release_notes/build_system_release_notes_for_xcode_10) for details.

To resolve this issue, we suggest that you use Xcode 11 instead. To workaround this issue in Xcode 10, you can either clean the output directory manually or try using the previous build system by passing `-UseNewBuildSystem=NO` to command line parameters.

## Cross-server Projects pop-up menu may not work in latest browsers
{product="tc"}

Due to the recent updates in the [SameSite cookie](https://web.dev/samesite-cookies-explained/) support, the __Projects__ pop-up menu may not display [cross-server projects](configuring-cross-server-projects-pop-up-menu.md) in some latest web browsers (see [more details](https://www.chromestatus.com/feature/5088147346030592) for Chrome Platform).
 
If you cannot access the cross-server __Projects__ menu, you can try to temporarily workaround this issue by changing your browser settings:

* In Google Chrome, open the `chrome://flags` page and disable the "_SameSite by default cookies_" option.
* In Mozilla Firefox, open the `about:config` page and disable the `network.cookie.sameSite.laxByDefault` option.
* In Safari, go to __Preferences | Privacy__ and disable the "_Prevent cross-site tracking_" option.

>By disabling this setting, you reduce the overall security of your browser. Use this workaround only if necessary. We highly recommend that you wait until this issue is fixed in the next version of TeamCity.
>
{type="note"}
 
This problem will be resolved in the TeamCity 2020.1.5 update. See the [related issue](https://youtrack.jetbrains.com/issue/TW-67644) for more details.

## .NET runner known issues

### .NET runner is not compatible with obsolete external .NET CLI Support plugin
{product="tc"}

The reworked [.NET build runner](net.md) is not compatible with the obsolete external plugin [.NET CLI Support](https://plugins.jetbrains.com/plugin/9190--net-cli-support) (used in versions 2017.1 and earlier). If the obsolete plugin is installed on your server, you will get the _Error creating bean with name "jetbrains.buildServer.dotnet.DotnetRunnerRunType"_ after updating to TeamCity 2020.1. To solve this issue, please [uninstall the obsolete plugin](installing-additional-plugins.md#Uninstalling+a+plugin+via+Web+UI) from your server.

Note that all the existing .NET CLI Support build steps are automatically switched to the new .NET runner on updating to TeamCity 2019.2.3 or later. For more information, refer to our [upgrade notes](upgrade-notes.md#Reworked+.NET+build+runner).

### Cannot create an instance of the logger when using early Visual Studio 2017 build

In rare cases, if an early build of Visual Studio 2017 is installed on your build agent, you might get the "Cannot create an instance of the logger" error when running the .NET build step. This might be caused by the logger's incompatibility with the framework version being used.

To workaround this problem, you can upgrade Visual Studio 2017 to the latest build or, alternatively, install any later version of .NET Framework.

## NuGet known issues

### Prerelease packages are not visible in the TeamCity NuGet feed

__Problem__: Prerelease packages are not visible in the TeamCity NuGet feed.

__Cause__: NuGet clients __prior to version 3__ fail to list prerelease packages if the package version violates [the required format](http://docs.nuget.org/create/versioning#user-content-prerelease-versions).

__Solution__: Delete build artifacts whose versions violate  [the required format](http://docs.nuget.org/create/versioning#user-content-prerelease-versions).

### Packages indexing is slow in TeamCity NuGet feed
{product="tc"}

__Problem__: After TeamCity server host machine move or upgrade, build metadata can be reset.

__Cause__: The TeamCity NuGet feed relies on build metadata, and packages reindexing can take a lot of time depending on the number of packages and the idle time of the TeamCity server.

__Solution__: To speed up build metadata reindexing, specify the following [internal properties](configuring-teamcity-server-startup-properties.md):


```Shell
teamcity.buildIndexer.indexPackSize=1000
teamcity.buildIndexer.packSleepDurationMs=10

```

To check the metadata indexing progress, look for lines similar to the ones below in the [teamcity-server.log](teamcity-server-logs.md) file:

```Shell
INFO - .index.BuildIndexer (metadata) - Enqueued next 100 builds for indexing, builds left: 7064, last build id: 8142

```

After reindexing is complete, remove these internal properties.

### NuGet does not pull recent package version

__Problem__: NuGet can sometimes skip the recent versions of packages when pulling these packages from the feed.

__Cause__: NuGet caches the server responses, thus pulling does not detect the most recent changes in the feed.

__Solution__: To send a new request directly to the server instead of the cache, use the `--no-cache` parameter in your request.

## Cannot use multiline parameters in PowerShell

Earlier versions of the [PowerShell](powershell.md) runner don't support passing multiline arguments. Since version 2020.1.4, you can enable this support by setting the `teamcity.powershell.arguments.multiline=true` [configuration parameter](configuring-build-parameters.md).