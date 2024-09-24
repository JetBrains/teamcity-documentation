[//]: # (title: Installing Additional Plugins)
[//]: # (auxiliary-id: Installing Additional Plugins)

Plugins can add extra functionality to your TeamCity server. You can get them in the [JetBrains Marketplace](https://plugins.jetbrains.com/teamcity).

## Installing Plugin from JetBrains Plugins Repository

To install plugins from the repository:
1. Go to __Administration | Plugins__ in TeamCity and click __Browse plugins repository__.
2. You will be redirected to the repository. Find the required plugin, click __Get__, and then __Install to http[s]://\<teamcityUrl\>__.   
You will be redirected to the plugin list in TeamCity. 
3. Confirm the plugin installation by clicking __Install__.
4. To enable the plugin after installation, click the plugin context menu and select __Load__.

## Installing Plugin via Web UI

Go to the __Administration | Plugins__ page and upload a plugin ZIP archive from your local machine using the corresponding link.

## Installing Plugin Manually

Copy the ZIP plugin package into the` <[TeamCity Data Directory](teamcity-data-directory.md)>/plugins` directory. If you have an earlier version of the plugin in the directory (though the plugin package can be named differently), remove it.

## Enabling Plugin

To enable the plugin after installation, click the plugin context menu and select __Load__. The plugin will be enabled without the server restart.

## Uninstalling Plugin via Web UI

1. Go to __Administration | Plugins__, locate an external plugin in the list, click the arrow icon next to it, and select __Delete__. 
2. Once the plugin is deleted, the option to restart the server appears on the page. Click it and check that the plugin version is no longer listed in __Administration | Plugins__.

## Uninstalling Plugin Manually

Remove the plugin package from the [`<TeamCity Data Directory>`](teamcity-data-directory.md)`/plugins` directory and restart the TeamCity server.

<seealso>
        <category ref="admin-guide">
            <a href="disabling-teamcity-plugins.md">Disabling TeamCity Plugins</a>
        </category>
</seealso>