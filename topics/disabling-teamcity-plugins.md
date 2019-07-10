[//]: # (title: Disabling TeamCity Plugins)
[//]: # (auxiliary-id: Disabling TeamCity Plugins)
TeamCity plugins are parts of TeamCity used to provide integration with specific build tools / perform specific tasks.

<warning>

If you disable some plugins, TeamCity will not be able to run their builds/tasks, so be sure you do not need the features you are going to disable.
</warning>

__To disable plugins via config files:__

<note>

This is applicable to versions __prior to TeamCity 2017.2__. Starting from this version, it is recommended to disable plugins __via the web UI.__
</note>

1. Create the `disabled_plugins.txt` file in `<TeamCity data directory>/config`.
2. Provide a new-line separated list of the plugin names (as specified in the `teamcity-plugin.xml` file of every plugin, the file is usually located in the default directory for bundled TeamCity plugins `<TeamCity web application>/WEB-INF/plugins/<plugin>/teamCity-plugin.xml`).
3. Restart the server. The plugin names are displayed as the strikethrough text on the __Administration | Plugins List__ page noting the file where the plugins were disabled.
 
__To disable plugins via Web UI:__

Аny plugin can be disabled using the TeamCity UI: on the __Administration | Plugins List__ page every external plugins has a button, clicking which opens a popup with the corresponding option; the bundled plugins have a link which can be used to disable them. On disabling a plugin, a warning is displayed that this may impact other TeamCity components (for example, other plugins). During the next server restart, the disabled plugin is not loaded. If there are other plugins depending on the disabled one, they will not be loaded either. Disabled plugins are greyed out in the list. 
