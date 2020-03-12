[//]: # (title: Windows Tray Notifier)
[//]: # (auxiliary-id: Windows Tray Notifier)

The Windows Tray Notifier is an utility which allows monitoring the status of specific build configurations in the system tray via popup alerts and status icons.

## Installing Windows Tray Notifier

To install the Windows Tray Notifier:
1. In the top right corner of the TeamCity web UI, click the arrow next to your username, and select __My Settings &amp; Tools__.
2. In the __TeamCity Tools__ area, click the __download__ link under __Windows tray notifier__.
3. Run the TrayNotifierInstaller.msi file and follow the instructions.

## Configuring Windows Tray Notifier

To launch Windows Tray Notifier, run the __Start &gt; Programs &gt; JetBrains TeamCity Tray Notifier__ menu.

When the application started, you need to connect and log in to your server:
1. Specify your TeamCity server URL and credentials to log in to it.
2. Wait for the notifier to connect to your TeamCity server. Once the connection is established, you'll be prompted to configure notification rules in the __My Settings &amp; Tools | Notification rules | Windows Tray Notifier__ section: <img src="windows-tray-notifier.png" width="750" alt="Windows Tray Notifier"/>
3. When Windows Tray Notifier is launched, the [status icon](#Status+Icons) in the Windows System Tray appears.

## Windows Tray Notifier UI

### Status Icons

After you have launched Windows Tray Notifier and specified your TeamCity username and password, the Notifier icon showing the state of your projects and build configurations appears in Windows System Tray.

If you have no projects and build configurations to monitor, the icon represents a question mark. After you have configured a list of build configurations and projects and their state changes, the status icon changes to reflect the change as well. The table below represents these possible states.

<table><tr>

<td>

Icon


</td>

<td>

Meaning


</td></tr><tr>

<td>

![trayNotifierBuildSuccessfulIcon.png](trayNotifierBuildSuccessfulIcon.png)


</td>

<td>

Build is successful


</td></tr><tr>

<td>

![trayNotifierBuildFailedIcon.png](trayNotifierBuildFailedIcon.png)


</td>

<td>

Build failed and nobody is investigating the failure


</td></tr><tr>

<td>

![errorWithResp.gif](errorWithResp.gif)


</td>

<td>

A team member has started investigating the build failure


</td></tr><tr>

<td>

![fixed.gif](fixed.gif)


</td>

<td>

The person who investigated the build failure has submitted a fix, but the build has not executed successfully yet


</td></tr><tr>

<td>

![paused.gif](paused.gif)


</td>

<td>

Build configuration is paused, and builds are not triggered automatically


</td></tr></table>

<note>

The Notifier icon always shows the status of the _last completed build_ of your watched project or build configurations, unless you select to be notified when __the first build error occurs__ option on the __Windows Tray Notifier settings__ page. In this case, the notifier does not wait for the failing build to finish, and it displays a failed build icon as soon as the running build fails a test.
</note>

 

If you right\-click the status icon, you can access all Windows Tray Notifier features described in table below.

<table><tr>

<td>

Option


</td>

<td>

Description


</td></tr><tr>

<td>

Open Quick View Window


</td>

<td>

Displays the __Quick View__ window.


</td></tr><tr>

<td>

Go to "Projects" Page


</td>

<td>

Opens the __Projects__ tab.


</td></tr><tr>

<td>

Go to "My Changes" Page


</td>

<td>

Opens the __My Changes__ tab.


</td></tr><tr>

<td>

Configure Watched Builds


</td>

<td>

Opens the __Windows Tray Notifier settings__ page where you can select the build configurations to monitor and notification events.


</td></tr><tr>

<td>

Auto Upgrade


</td>

<td>

Select this option to allow the program to automatically upgrade.


</td></tr><tr>

<td>

Run on Startup


</td>

<td>

Select this option to automatically launch the program when windows boots.


</td></tr><tr>

<td>

About


</td>

<td>

Displays the information on the program's splash screen.


</td></tr><tr>

<td>

Logout


</td>

<td>

Use this function to log out of the TeamCity server. This will allow you to a different one.


</td></tr><tr>

<td>

Exit


</td>

<td>

Quits the program.


</td></tr></table>

 

### Quick View Window

On left\-clicking the Notifier icon, the TeamCity Quick View window appears displaying the status of the watched projects / build configurations:

<img src="BCstatus.png" width="350" alt="Windows Tray Notifier Status"/>

Click the _build results_ (__Success__ in the image above) or _changes_ links to navigate to the __Build Results__ and __Changes__ pages respectively in the TeamCity web interface for details.

### Pop-up Notification

Besides the state icons, Windows tray notifier displays pop\-up alerts with a brief build results information on the particular build configurations and notification events.

<img src="poUp.png" width="250" alt="Windows Tray Notifier Pop-up Window"/>

When a pop\-up notification appears, you can click the link in it to go the __Build results__ page for details.

## Windows Tray Notifier Logs

__Since TeamCity 2017.2__  Windows Tray Notifier logs events and warnings to the `%ProgramData%` or `%AppData%` (since 2017.2.1) `JetBrains\TeamCity\TrayNotifier\logs` directory containing the following files:
*  `teamcity-tray.log` with common details
*  `teamcity-update.log` with update details.
 

You can tune the logger verbosity via the `/verbosity` command line switch: debug logs can be enabled using the following command: 


```Shell
C:\Program Files (x86)\JetBrains\TeamCity\TrayNotifier\JetBrains.TrayNotifier.exe /verbosity:debug

```

 __  __

__See also:__


__User's Guide__: [Subscribing to Notifications](subscribing-to-notifications.md)   
__Administrator's Guide__: [Customizing Notifications](customizing-notifications.md)   
__Installing Tools__: [Working with Windows Tray Notifier](windows-tray-notifier.md)

__ __