[//]: # (title: TeamCity Memory Monitor)
[//]: # (auxiliary-id: TeamCity Memory Monitor)

TeamCity server checks available memory on a regular basis and warns you if the amount of the memory available is too low.

There are several warning types reported:

## Low pool memory

Is reported when memory usage in a single memory pool exceeds 90% after garbage collection. High server activity may cause such memory usage.

## Low total memory

Is reported when more than 90% of total memory has been in use during the last 5 minutes and more than 20% of CPU resources are being consumed by garbage collection. Lasting memory lack  may result in performance degradation and server instability as well.

## Heavy GC overload

Is reported when memory cleaning takes more than 50% of CPU resources  on average. It usually means really serious problems with memory resulting in high performance degradation.

## Customization

Several [internal properties](configuring-teamcity-server-startup-properties.md#TeamCity+internal+properties) can be used to customize the Monitor:

* `teamCity.memoryUsageMonitor.poolNames` sets up pool names to track. Case\-sensitive comma\-separated string is accepted.

* `teamCity.memoryUsageMonitor.warningThreshold` allows setting up a minimal warning threshold. Affects all tracked memory pools except for PermGen ([replaced with metaspace](http://javaeesupportpatterns.blogspot.ru/2013/02/java-8-from-permgen-to-metaspace.html) memory allocation in TeamCity 10).

* `teamCity.memoryUsageMonitor[<Pool name>].warningThreshold` can be used to modify single memory pool threshold. Spaces should be escaped or changed to `\ ` signs.

* `teamCity.memoryUsageMonitor.gcWarningThreshold` allows setting up the allowed percentage of resources to spent for cleaning the memory.


[//]: # (Internal note. Do not delete. "TeamCity Memory Monitord317e56.txt")    

__  __

__See also:__


__Reporting Issues__: [Out Of Memory Problems](reporting-issues.md)   
__Increasing Server Memory__: [Installing and Configuring the TeamCity Server](installing-and-configuring-the-teamcity-server.md)

__ __