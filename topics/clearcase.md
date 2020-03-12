[//]: # (title: ClearCase)
[//]: # (auxiliary-id: ClearCase)

<note>

__Since TeamCity 2019.2__, the [VCS Support: ClearCase](https://plugins.jetbrains.com/plugin/13210-vcs-support-clearcase) plugin has been unbundled. To integrate TeamCity with ClearCase, download and [install the plugin](installing-additional-plugins.md).

</note>

## Initial Setup

An installed ClearCase client on the TeamCity server is required to make TeamCity ClearCase integration work. You need to create a ClearCase view on the TeamCity server machine (regardless of whether you plan to use the server\-side or agent\-side checkout) under the same user that TeamCity server runs under. This view will be used for collecting changes in ClearCase VCS roots and for checkout in case of the server\-side checkout mode. In case of the agent\-side checkout mode, the config spec of this view will be also used to automatically create views on agents.


<tip>

If you plan to use the agent\-side [VCS Checkout Mode](vcs-checkout-mode.md), make sure the ClearCase client (cleartool) is also installed on the agents and is properly configured, i.e. the tool must be in system paths and have access to the ClearCase Registry.
</tip>

## ClearCase Settings

Common VCS Root properties are described [here](configuring-vcs-roots.md#Common+VCS+Root+Properties). The section below contains a description of the fields and options specific to the ClearCase Version Control System.


<table>
<tr>


<td>

Option 


</td>


<td>

Description 


</td>
</tr>
<tr>


<td>

ClearCase view path 


</td>


<td>

A local path on the TeamCity server machine to the ClearCase view created during the [initial setup](#Initial+Setup). The snapshot view is preferred as there is no benefit in using the dynamic view with TeamCity. Also, dynamic views are not supported for the agent\-side checkout ([TW-21545](http://youtrack.jetbrains.com/issue/TW-21545)). 


<note>

Do not use the view you are currently working with. TeamCity calls the update procedure _before_ checking for changes and building patch, and thus it might lead to problems with checking in changes.
</note>



</td>
</tr>
<tr>


<td>

Relative path within the view 


</td>


<td>

The path relative to the "ClearCase view path" that limits the sources to watch and checkout for the build. 


</td>
</tr>
<tr>


<td>

 Branches 


</td>


<td>

Branches are used in the `-branch` parameter for the `lshistory` command to significantly improve its performance.    
You can either let TeamCity detect the needed branches automatically (via the view's config spec) or specify your own list of needed branches. Press the __Detect now__ button to see the branches automatically detected by TeamCity.    
You can also choose the `custom` option and leave the text field blank \- then the `-branch` parameter will not be used for the `lshistory` command at all.   
If you specify or TeamCity detects several branches, then `lshistory` will be called for every branch and all results will be merged.   
The `lshistory` options can be customized, see [TW-12390](http://youtrack.jetbrains.com/issue/TW-12390#comment=27-165678)


</td>
</tr>
<tr>


<td>

Use ClearCase 


</td>


<td>

Use the drop\-down menu to select either UCM or BASE. 


</td>
</tr>
<tr>


<td>

Global labeling 


</td>


<td>

Check this option if you want to use global labels 


</td>
</tr>
<tr>


<td>

Global labels VOB 


</td>


<td>

Pathname of the VOB tag (whether or not the VOB is mounted) or of any file system object within the VOB (if the VOB is mounted) 


</td>
</tr>
</table>



<tip>

Make sure that the user that runs the TeamCity server process is also a ClearCase view owner.
</tip>

 __  __

__See also:__

__Administrator's Guide__: [VCS Checkout Mode](vcs-checkout-mode.md)

__ __
