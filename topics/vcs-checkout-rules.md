[//]: # (title: VCS Checkout Rules)
[//]: # (auxiliary-id: VCS Checkout Rules)
VCS Checkout Rules allow you to check out a part of the configured VCS root and to map directories from the version control to subdirectories in the [build checkout directory](build-checkout-directory.md) on a build agent. Thus, you can define a VCS root for the entire repository and make each build configuration checkout only the relevant part of it.

The Checkout Rules affect the changes displayed in the TeamCity for the build and the files checked out for the build on agent. To display changes, but not to trigger a build for a change, use [VCS Trigger Rules](configuring-vcs-triggers.md#VCS+Trigger+Rules).

The general recommendation is to have a small number of VCS roots (pointing to the root of the repository) and define what is checked out by a specific build configuration via checkout rules.

__To add a checkout rule__, go to the build configuration's __Version Control Settings__ page, locate the VCS root in the list, and click __Edit checkout rules__ to open a form for entering the rules. Use the VCS repository browser ![VCS-browserIcon.png](VCS-browserIcon.png) to select a directory to check out.


<chunk include-id="note-perforce-vcs">

Note that Perforce support in TeamCity treats checkout rules as case-sensitive. Case-insensitivity for Perforce-based build configurations can be enabled on the __Version Control Settings__ page by adding the following comment in the _Edit Checkout Rules_ form: `##teamcity ignore-case`.

</chunk>


<note>

Checkout rules can only be set to directories, files are __not__ supported.

</note>

## Syntax

In the examples below the paths in the repository (`VCSPath`) are relative to the configured VCS root, the paths on the agent (`AgentPath`) are relative to the build checkout directory.

The general syntax of a single checkout rule is as follows:


```Shell

+|-: VCSPath [=> AgentPath]

```

<include src="branch-filter.md" include-id="OR-syntax-tip"/>

<note>

If no rule is specified, all files are included.   
When you start entering a rule, note that as soon as you enter any `+:` rule, TeamCity will remove the default "include all" setting.   
To include all the files explicitly, use the `+:.` rule.

</note>

<note>

Note that exclude checkout rules (in the form of `-:`) will generally only speed up server-side checkouts, unless you use [Perforce](perforce.md) and [TFS](team-foundation-server.md) agent-side checkout, where exclude rules are processed effectively.   
With other version control systems, agent-side checkouts may emulate the exclude checkout rules by checking out all the root directories mentioned as include rules and deleting the excluded directories. With such systems, exclude checkout rules should generally be avoided for the agent-side checkout. Refer to the [VCS Checkout Mode](vcs-checkout-mode.md) page for more information.   

With Git agent-side checkout, TeamCity translates some of the checkout rules to the sparse checkout patterns. See the details in [Git](git.md#Limitations).
</note>


When entering rules, note the following:
* To enter multiple rules, each rule should be entered on a separate line.
* For each file, the most specific rule will apply if the file is included, regardless of what order the rules are listed in.
* If you don't enter an operator, it will default to `+:`.

Rules can be used to perform the following operations:

<table><tr>

<td>

Syntax


</td>

<td>

Explanation


</td></tr><tr>

<td>

`+:.=>AgentPath`


</td>

<td>

Checks out the root into the `Path` directory on a build agent.


</td></tr><tr>

<td>

`-:VCSPath`


</td>

<td>

Excludes `VCSPath` (the path must be a directory and not a filename).


</td></tr><tr>

<td>

`+:VCSPath=>.`


</td>

<td>

Maps the `VCSPath` from the VCS to the [build agent's default work directory](agent-work-directory.md).


</td></tr><tr>

<td>

`VCSPath=>NewAgentPath`


</td>

<td>

Maps the `VCSPath` from the VCS to the `NewAgentPath` directory on a build agent.


</td></tr><tr>

<td>

`+:VCSPath`


</td>

<td>

Maps the `VCSPath` from the VCS to the same\-named directory (`VCSPath`) on a build agent.


</td></tr></table>


An example with three VCS checkout rules:


```Shell

-:src/help
+:src=>production/sources
+:src/samples=>./samples
```



In the above example, the first rule excludes the `src/help` directory and its contents from checkout. The third rule is more specific than the second rule and maps the `src/samples` path to the `samples` path in the build agent's default work directory. The second rule maps the contents of the `src` path to the `production/sources` on the build agent, except `src/help` which was excluded by the first rule and `src/samples` which was mapped to a different location by the third rule.



 __  __

__See also:__


__Administrator's Guide__: [VCS Checkout Mode](vcs-checkout-mode.md)
