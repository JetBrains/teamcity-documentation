[//]: # (title: Installing Additional Plugins)
[//]: # (auxiliary-id: Installing Additional Plugins)
You can get TeamCity plugins in the [plugin repository](https://plugins.jetbrains.com/?teamcity).

## Installing a plugin from JetBrains Plugins repository

__Since TeamCity 2018.2__, you can install plugins from the plugins repository.

1. Go to the __Administration | Plugins List__ page and click __Browse plugins repository__.
2. You will be redirected to the repository. Find the desired plugin and click __Get__ and then __Install to http[s]://\<teamcityUrl\>__.
3. You will be redirected to the plugins list in TeamCity. Confirm plugin installation by click __Install__.
4. To enable plugin after installation, click the plugin context menu and select __Load__. 

## Installing a plugin via Web UI

Go to the __Administration | Plugins List__ page and upload a plugin zip from your local machine using the corresponding link.

## Installing a plugin manually

Copy the zip plugin package into the \<[TeamCity Data Directory](teamcity-data-directory.md)\>/plugins  directory. If you have an earlier version of the plugin in the directory (though the plugin package can be named differently), remove it.


## Enabling the plugin

Prior to TeamCity 2018.2, you need to restart the server to enable the plugin. If the plugin has an agent part, all the agents will be updated automatically. For plugins with the agent part only, the server restart can be skipped: the agents will be updated automatically.

__Since TeamCity 2018.2__, to enable plugin after installation, click the plugin context menu and select __Load__. The plugin will be enabled without server restart.

## Uninstalling a plugin via Web UI

1. Go to the __Administration | Plugins List__ page, locate an external plugin in the list, click the arrow icon next to it, and use the __Delete__ option. 
2. Once the plugin is deleted, the option to restart the server appears on the page. Click the link to restart the server and check that the plugin version is no longer listed on the __Administration | Plugins List__ page.


## Uninstalling a plugin manually

Remove the plugin package from the \<[TeamCity Data Directory](teamcity-data-directory.md)\>/plugins directory and restart the TeamCity server.

__ __