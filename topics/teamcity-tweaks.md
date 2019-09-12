[//]: # (title: TeamCity Tweaks)
[//]: # (auxiliary-id: TeamCity Tweaks)


This page lists some of the the [Configuring TeamCity Server Startup Properties](configuring-teamcity-server-startup-properties.md) which can be used to tweak certain aspects of TeamCity behavior.
It is not recommended to use any of these unless you face an issue which you expect to address by using the properties.
Please note that the support for any of these properties can be abandoned in the future versions of TeamCity without any notice. Thus, if you find a property useful in your environment please let us know about that: detail your case and the properties/values used in an email sent to our [support email address](https://confluence.jetbrains.com/display/TW/Feedback).


## Web Page Refresh Interval



You can configure different polling intervals (in seconds) for different pages in the TeamCity web UI:



<table>
<tr>


<td>

 Property 


</td>


<td>

 Default 


</td>


<td>

 Description 

</td>
</tr>
<tr>

<td>

teamcity.ui.pollInterval


</td>


<td>
 
 6

[//]: # (Internal note. Do not delete. "TeamCity Tweaksd323e59.txt")    


</td>


<td>

 How often the server is queried for common events (like build statuses, agents counter and so on).   
__Since TeamCity 9.0__ this property works only if WebSocket connection is not available and polling is used instead.


</td>
</tr>
<tr>


<td>

teamcity.ui.events.pollInterval


</td>


<td>
 
6


[//]: # (Internal note. Do not delete. "TeamCity Tweaksd323e83.txt")    


</td>


<td>

The delay between an event (received via polling or WebSockets) and the ajax request to update the UI.

	
* With WebSocket, a client receives the event immediately, but reacts to it after the specified interval; as a result, for example, a started build appears on the Overview page with a delay.
	
* With polling, a client receives the event during the polling request determined by `teamcity.ui.events.pollInterval` and reacts to it after the delay defined by `teamcity.ui.events.pollInterval`: for example, a started build appears on the Overview page after `teamcity.ui.events.pollInterval` \+ `teamcity.ui.events.pollInterval` seconds


</td>
</tr>
<tr>

<td>

teamcity.ui.systemProblems.pollInterval


</td>


<td>
 
20

[//]: # (Internal note. Do not delete. "TeamCity Tweaksd323e125.txt")    


</td>


<td>

 


</td>
</tr>
<tr>


<td>

teamcity.ui.problemsSummary.pollInterval


</td>


<td>
 
8

[//]: # (Internal note. Do not delete. "TeamCity Tweaksd323e146.txt")    



</td>


<td>

 
</td>
</tr>
<tr>


<td>

teamcity.ui.buildQueueEstimates.pollInterval


</td>


<td>
 
10

[//]: # (Internal note. Do not delete. "TeamCity Tweaksd323e168.txt")    
 

</td>


<td>


</td>
</tr>
</table>





[//]: # (Internal note. Do not delete. "TeamCity Tweaksd323e181.txt")    



