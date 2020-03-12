[//]: # (title: Mercurial)
[//]: # (auxiliary-id: Mercurial)

TeamCity uses the typical Mercurial command line client: hg command. Mercurial 1.5.2\+ is supported.

<note>

Mercurial is to be installed on the server machine and, if the [agent-side checkout](vcs-checkout-mode.md#agent-checkout) is used, on the agents.
</note>

Note that:
* __Remote Run__ from IDE is not supported. Please use [Branch Remote Run Trigger](branch-remote-run-trigger.md) instead.
* Checkout rules for agent\-side checkout are not supported except for the `.=><target_dir>` rule.
For common VCS Root properties, see [this section](configuring-vcs-roots.md#Common+VCS+Root+Properties). The section below contains the description of Mercurial\-specific fields and options.

TeamCity supports Mercurial out of the box.

## General Settings

<table><tr>

<td>

Option


</td>

<td>

Description


</td></tr><tr>

<td>

Pull changes from


</td>

<td>

The URL of your hosting.


</td></tr><tr>

<td>

Default branch


</td>

<td>

Set to the default branch which used in the absence of branch specification or when the branch of the branch specification cannot be found. Note that parameter references are supported here.


</td></tr><tr>

<td>

Branch specification


</td>

<td>

In this area list all the branches you want to be monitored for changes. The syntax is similar to checkout rules: `+|-:branch_name`, where `branch_name` is specific to the VCS (with the optional `*` placeholder). Note that only one asterisk is allowed and each rule has to start with a new line.  Bookmarks can also be used in the branch and branch specification fields. If a bookmark has the same name as a regular branch, a regular branch wins. More in the related [TeamCity blogpost](http://blog.jetbrains.com/teamcity/2013/04/mercurial-bookmarks/).

<note>

Bookmarks support requires Mercurial 2.4 installed on the TeamCity server.
</note>


</td></tr><tr>

<td>

Use tags as branches


</td>

<td>

Allows you to use tags in branch specification. By default, tags are ignored.


</td>

<td>




[//]: # (Internal note. Do not delete. "Mercuriald211e104.txt")    





</td></tr><tr>

<td>

Detect subrepo changes


</td>

<td>

By default, subrepositories are not monitored for changes.


</td></tr><tr>

<td>

Username for tags/merge


</td>

<td>

A custom username used for labeling


</td></tr><tr>

<td>

Use uncompressed transfer


</td>

<td>

Uncompressed transfer is faster for repositories in the LAN.


</td></tr><tr>

<td>

HG command path


</td>

<td>

The path to the hg executable. Used on TeamCity server only if included into whitelist. See more [below](#Path+to+hg+executable+detection).


</td></tr></table>

 

### Path to hg executable detection

When an agent starts, the hg\-plugin detects Mercurial installed on the agent machine.

The plugin tries to run the `hg version` command using the path specified by the `teamcity.hg.agent.path` parameter. You can change this parameter in \<[Agent Home Directory](agent-home-directory.md)\>\conf\buildAgent.properties.

If this parameter is not set, the plugin uses `hg` as a path to the command, assuming it is somewhere in the $PATH. If the command is executed successfully and mercurial has an appropriate version (1.5.2\+), then the hg\-plugin reports the path to hg in the `teamcity.hg.agent.path` parameter.

During the build, the plugin uses the hg specified in the _HG command path_ field of a VCS root settings. To use the detected hg, put `%teamcity.hg.agent.path%` in this field. Configurations with such settings will be run only on agents which report the path to hg.

The server side of the plugin checks the value of the `teamcity.hg.customServerHgPathWhitelist` [internal property](configuring-teamcity-server-startup-properties.md). The property contains the `;`-separated list of allowed hg paths to use on the server.  If the path specified in VCS root is in whitelist, then it is used on the server. If not, the path specified in the `teamcity.hg.server.path` [internal property](configuring-teamcity-server-startup-properties.md) is used. If this property is not set, TeamCity server uses `hg` from the `$PATH`.

## Agent Settings

These are the settings used in case of the agent\-side checkout ([default mode](vcs-checkout-mode.md#prefer-agent-checkout)), which requires Mercurial installed on all agents.

<table><tr>

<td>

Option


</td>

<td>

Description


</td></tr><tr>

<td>
Mercurial config

</td>

<td>

Specify the Mercurial configuration options to be applied to the repository during agent\-side checkout, for example, enter the following to enable the `largefiles` extension:   

```Plain Text
[extensions] 
`largefiles =

```
 
The configuration format is described [here](http://www.selenic.com/mercurial/hgrc.5.html).

Before 2017.2.2 this option was also used on TeamCity server. This was disabled for security reasons.


</td></tr><tr>

<td>

Purge settings


</td>

<td>

Defines whether to [purge files](https://www.mercurial-scm.org/wiki/PurgeExtension) and directories not being tracked by Mercurial in the current repository. You can choose to remove only unknown files and empty directories, or to remove ignored files as well. Added files and (unmodified or modified) tracked files are preserved.


</td></tr><tr>

<td>

Use mirrors


</td>

<td>

When enabled, TeamCity creates a local agent mirror first (under agent's `system/mercurial` directory) and then clones to the working directory from this local mirror. This option speeds up clean checkout, because only the build working directory is cleaned. Also, if a single root is used in several build configurations, a clone will be faster.


</td></tr></table>

## Internal Properties

This section describes hg\-related [internal properties](configuring-teamcity-server-startup-properties.md). You can modify the defaults to adjust the Mercurial settings as needed.

Server\-side [internal properties](configuring-teamcity-server-startup-properties.md):

<table><tr>

<td>

Property


</td>

<td>

Default


</td>

<td>

Description


</td></tr><tr>

<td>

`teamcity.hg.pull.timeout.seconds`


</td>

<td>

3600


</td>

<td>

Maximum time in seconds for pull operation to run


</td></tr><tr>

<td>

`teamcity.hg.server.path`


</td>

<td>

hg


</td>

<td>

Path to the hg executable on the server (see [Path to hg executable detection](#Path+to+hg+executable+detection) for the details).


</td></tr><tr>

<td>

`teamcity.hg.customServerHgPathWhitelist`

</td>

<td>
 

</td>

<td>

`;`-separated list of allowed paths to hg executable to use on TeamCity server machine

</td></tr></table>

[Agent configuration](build-agent-configuration.md) for Mercurial:

<table><tr>

<td>

Property


</td>

<td>

Default


</td>

<td>

Description


</td>

<td>

 


[//]: # (Internal note. Do not delete. "Mercuriald211e377.txt")    


 


</td></tr><tr>

<td>

`teamcity.hg.pull.timeout.seconds`


</td>

<td>

3600


</td>

<td>

Maximum time in seconds for pull operation to run


</td></tr><tr>

<td>

`teamcity.hg.agent.path`


</td>

<td>

hg


</td>

<td>

Path to hg executable on the agent (see [Path to hg executable detection](#Path+to+hg+executable+detection) for the details).


</td></tr></table>

__  __

__See also:__

__Administrator's Guide__: [Branch Remote Run Trigger](branch-remote-run-trigger.md)

__ __