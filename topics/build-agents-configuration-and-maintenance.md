[//]: # (title: Build Agents Configuration and Maintenance)
[//]: # (auxiliary-id: Build Agents Configuration and Maintenance)

## Viewing TeamCity agents details

The __Agents__ page of the TeamCity web UI provides the comprehensive information on the TeamCity agents. The number of tabs on the page may differ depending on your agent setup.

### Connected / Disconnected

The __Connected__ and __Disconnected__ tabs display the agents by [Agent pool](agent-pools.md) (default). To view the agents alphabetically, uncheck the __Group by agent pool__ box.   
For each pool TeamCity displays the status of its build agents. Clicking the arrow next to the pool displays the list of the pools agents with their statuses.

#### Enabling/Disabling Agents via UI

TeamCity distributes builds only among the enabled agents. Agents can be manually enabled/disabled via the web UI by clicking the status icon (1) next to the agent's name. Optionally, you can tell TeamCity to automatically disable/enable the agent after a period of time and enter your comment. TeamCity will follow the instructions and show the comment icon (2). Hovering over the icons will display the related information (3).

<img src="agent-enable-disable.png" width="483" alt="Enable/disable an agent"/>

### Pools 

Refer to a [separate page](agent-pools.md) for information on configuring agent pools in TeamCity.

### Parameters report

Filter all available agents using a specified parameter.

### Matrix and Statistics 

Refer to a separate page for information on [viewing the agents workload](viewing-agents-workload.md).

### Cloud

Lists all configured [agent cloud profiles](agent-cloud-profile.md).

### Diff 

Compare two agents and see their differences highlighted.

__ __