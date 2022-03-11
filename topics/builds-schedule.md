[//]: # (title: Builds Schedule)
[//]: # (auxiliary-id: Builds Schedule)

The __Builds Schedule__ page in the administration area of a specific project displays [schedule triggers](configuring-schedule-triggers.md) configured for [build configurations](managing-builds.md) belonging to this project. __Builds Schedule__ for the [Root Project](project.md#Root+Project) displays the list of triggers for the entire TeamCity server.

You can conveniently view the available schedule and plan your builds optimizing allocation of dependent hardware/software resources.

From this page it is also possible to [edit](configuring-build-triggers.md), disable or delete the triggers.

The __Build Schedule__ page also displays the information for [paused build configurations](changing-build-configuration-status.md).

Since TeamCity 2020.1, __Build Schedule__ offers an alternative filter option: to hide triggers with the enabled "_Trigger only if there are pending changes_" option. This helps to quickly find all triggers where this option is disabled if you need to investigate the builds' behavior.