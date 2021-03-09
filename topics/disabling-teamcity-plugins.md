[//]: # (title: Disabling TeamCity Plugins)
[//]: # (auxiliary-id: Disabling TeamCity Plugins)

TeamCity plugins are parts of TeamCity used to provide integration with specific build tools / perform specific tasks.

<warning>

If you disable some plugins, TeamCity will not be able to run their builds/tasks, so be sure you do not need the features you are going to disable.
</warning>

Аny plugin can be disabled using the TeamCity UI. To do this, go to the __Administration | Plugins__ page, click the menu button opposite the required plugin, and choose __Disable__. On disabling a plugin, TeamCity will display a warning that this action may impact other TeamCity components (for example, other plugins). During the next server restart, the disabled plugin is not loaded. If there are other plugins who depend on the disabled one, they will not be loaded either. Disabled plugins are greyed out in the list.