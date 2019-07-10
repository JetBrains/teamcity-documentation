[//]: # (title: VCS Checkout Mode)
[//]: # (auxiliary-id: VCS Checkout Mode)
The Version Control Settings page for a build configuration allows configuring how project source code is retrieved from VCS: you can [attach a VCS Root here](configuring-vcs-settings.md) and configure checkout options.

The VCS Checkout mode is a setting that affects how project sources reach an agent. This mode affects only sources checkout. The current revision and changes data retrieving logic is executed by the TeamCity server and thus TeamCity server needs to access the VCS server in any mode.

Depending on the version control used, agents can require command line clients installed and available in PATH on the agents (for example, Perforce, Git, Mercurial).

The checkout mode is configured on the build configuration's __Version Control Settings__ page, in the Checkout Options section (an advanced setting).

TeamCity has the following VCS checkout modes:

<table><tr>

<td>

Checkout mode


</td>

<td>

Description


</td></tr><tr>

<td>

Prefer to checkout files on agent

</td>

<td>

This is the default setting for the newly created build configurations. When upgrading, the checkout mode settings for existing build configurations are preserved.

With this setting enabled, TeamCity will use the agent\-side checkout (see the "Always checkout files on agent" mode below) if possible. If the agent\-side checkout is not possible, TeamCity will display a corresponding [health report item](server-health.md) and will use the server\-side checkout (see the "Always checkout files on server" mode below).

TeamCity falls back to the server\-side checkout in the following cases:

* No Git or Mercurial client is found on on the agent
* The Git or Mercurial client is present on the agent, but is of the wrong version
* The agent has no access to the repository
* If a Perforce client cannot be found on the agent using the same rules as while performing actual checkout or if stream depot is used and the checkout rules are complex (other than . =&gt; A )


</td></tr><tr>

<td>

 Always checkout files on server


</td>

<td>

The TeamCity server will [export the sources](build-checkout-directory.md) and pass them to an agent before each build. Since the sources are exported rather than checked out, no administrative data is stored in the agent's file system and version control operations (like check\-in, label or update) cannot be performed from the agent. TeamCity optimizes communications with the VCS servers by [caching the sources](clean-checkout.md) and retrieving from the VCS server only the necessary changes. Unless [clean checkout](clean-checkout.md) is performed, the server sends to the agent incremental patches to update only the files changed since the last build on the agent in the given checkout directory.

<note>

* The server side checkout simplifies administration overhead. Using this checkout mode, you need to install VCS client software on the server only (applicable to Perforce, Mercurial, TFS, Clearcase, VSS). Network access to VCS repository can also be opened to the server only. Thus, if you want to control who has access to the source repositories, the server side checkout is usually more effective.
* In some cases this checkout mode can lower the load produced on VCS repositories, especially if [Clean Checkout](clean-checkout.md) is performed often, due to the caching of clean patches by the server.
* Note that in the server checkout mode the administration directories (like `.svn`, `CVS`) are not created on the agent.
</note>


</td></tr><tr>

<td>

 Always checkout files on agent


</td>

<td>

The build agent will [check out the sources](build-checkout-directory.md) before the build. Agent\-side checkout frees more server resources and provides the ability to access version control\-specific directories (.svn, CVS, .git); that is, the build script can perform VCS operations (for example, check\-ins into the version control) – in this case ensure the build script uses credentials necessary for the check\-in.

VCS client software has to be installed on the agent (applicable to Perforce, Mercurial, Git) 

<note>

* Agent checkout is usually more effective with regard to data transfers and VCS server communications. The agent side checkout creates necessary administration directories (like `.svn`, `CVS`), and thus allows you to communicate with the repository from the build: commit changes and so on.
* Machine\-specific settings (like configuring SSL communications, and so on) have to be configured on each machine using agent\-side checkout.
* "Exclude" [VCS Checkout Rules](vcs-checkout-rules.md) in most cases cannot improve agent checkout performance because an agent checks out the entire top\-level directory included into a build, then deletes the files that were excluded. Perforce and TFS are exceptions to the rule, because before performing checkout, specific client mapping(Perforce)/workspace(TFS) is created based on checkout rules. "Exclude" checkout rules are not supported for Git and Mercurial when using checkout on an agent due to these DVCS limitations.    
There is a [known issue](https://youtrack.jetbrains.com/issue/TW-43648) with CVS VCS root ignoring exclude checkout rules when using checkout on an agent.
* Integration with certain version controls can provide additional options when agent\-side checkout is used. For example, [Subversion](subversion.md).

</note>


</td></tr><tr>

<td>

Do not check out files automatically


</td>

<td>

TeamCity will not check out any sources automatically, the [default build checkout directory](build-checkout-directory.md) will still be created so that you could use it to check out the sources via a build script. Note that TeamCity will accurately report changes only if the checkout is performed on the revision specified by the [`build.vcs.number.*`](predefined-build-parameters.md) properties passed into the build.

The build checkout directory will __not__ be cleaned automatically, unless the directory expiration period is [configured.](build-checkout-directory.md)


</td></tr></table>

 
__  __

__See also:__

__Administrator's Guide__: [VCS Checkout Rules](vcs-checkout-rules.md)
