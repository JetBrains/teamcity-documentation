[//]: # (title: Build on TeamCity)
[//]: # (auxiliary-id: Build on TeamCity)

To simplify the process of integrating JetBrains Space with TeamCity, you can click the **Build on TeamCity** button from the Space repository page. This button redirects you to TeamCity and guides you through the process of setting up the required connections or, if the connections are already configured, it brings you to the corresponding TeamCity build page for your Space repository.

## Setting up Connections from TeamCity

To perform this procedure, you need to have:

* _Project administration rights_ in the JetBrains Space instance of your organization.
* _Project administration rights_ in either the [TeamCity Cloud instance](https://www.jetbrains.com/teamcity/signup/) or [TeamCity On-Premises server](https://www.jetbrains.com/teamcity/download/) of your organization.

<procedure>
    <step>From the Space repository page, click <b>Build on TeamCity</b>.</step>
    <step>A new tab opens and you are automatically redirected to your organization's instance of TeamCity. Log in, if necessary.</step>
    <step><p>Teamcity opens either on the <b>Create a Project</b> page or the <b>Create a Build Configuration</b> page. The next step depends on the level of connection that already exists between TeamCity and Space:</p>
    <list type="alpha-lower">
        <li>
<p>If you see a <b>Connect</b> button at the bottom of the page:</p>
   <procedure title="Create JetBrains Space Connection in TeamCity" initial-collapse-state="true">
     <step><p>In the <b>Choose a repository</b> section, you see the hint <i>To show available repositories TeamCity requires a connection with JetBrains Space</i>. Click the <b>Connect</b> button.</p>
     <img src="build-on-teamcity-org-level.png" alt="TeamCity requires a connection with JetBrains Space" width="706" border-effect="line"/>
     </step>
     <step><p>A new window opens, showing a dialog that prompts you to install a new TeamCity application in Space. Enter a name for the TeamCity application and click <b>Install</b>.</p>
     <note>
     The certificate associated with the TeamCity HTTPS endpoint must be signed by a well-known certificate authority, otherwise this step raises an error.
     </note>
     </step>
     <step>If the install step succeeds, the dialog status changes to <i>Successfully installed</i>. Click <b>Approve all and go back to TeamCity</b>.</step>
     <step>You are redirected back to the <b>Create a Project</b> page on TeamCity. The <b>Choose a repository</b> section now shows the hint <i>To show available projects TeamCity requires access to your Space account</i> and the <b>Sign in to Space</b> button.</step>
     <step>Proceed to <a href="#Create+Project+Connection+in+TeamCity">Create Project Connection in TeamCity</a>.</step>
   </procedure>
        </li>
        <li>
<p>If you see a <b>Sign in to Space</b> button at the bottom of the page:</p>
<procedure title="Create Project Connection in TeamCity" initial-collapse-state="true" id="Create+Project+Connection+in+TeamCity" >
    <step><p>If this is the first time you are connecting a TeamCity project to Space, you need to authenticate your user profile in Space. Click <b>Sign in to Space</b> and accept the access request.</p>
        <img src="build-on-teamcity-project-level.png" alt="TeamCity requires access to your Space account" width="706" border-effect="line"/>
    </step>
    <step><p>The <b>Connect to Space Project</b> dialog opens. Using the <b>TeamCity project</b> dropdown, you can choose to:</p>
     <list>
         <li>Create a new TeamCity project and connect it to your Space project (default), or</li>
         <li>Connect an existing TeamCity project to your Space project.</li>
     </list>
    </step>
    <step>Click <b>Proceed</b>.</step>
    <step><p>The <b>Connect to Space Project</b> dialog now prompts you to enable the required permissions in the Space application. Click the <b>Space Administration</b> link and click <b>Approve all</b> to authorize the connection.</p></step>
    <step>Return to the TeamCity page and click <b>Ok</b> to close the <b>Connect to Space Project</b> dialog.</step>
    <step>If the <b>Choose a repository</b> section shows the warning <i>The connection is waiting for its permissions to be granted at Space</i>, click the refresh icon to update the status.</step>
    <step>Proceed to <a href="#Create+a+Build+Configuration+in+TeamCity">Create a Build Configuration in TeamCity</a>.</step>
</procedure>
        </li>
        <li>

<p>If you see a preselected repository at the bottom of the page:</p>
<procedure title="Create a Build Configuration in TeamCity" initial-collapse-state="true" id="Create+a+Build+Configuration+in+TeamCity">
    <p>Note that you do <i>not</i> require project administration rights in JetBrains Space to perform this procedure.</p>
    <step>
        <p>The <b>Choose a Space project</b> section shows the list of remote Space projects that are available to connect to this project, with your current Space project preselected. The <b>Choose a repository</b> section shows the list of repositories from the selected Space project, with your current Space repository preselected.</p>
        <img src="build-on-teamcity-build-config.png" alt="Click here to create a new build configuration" width="706" border-effect="line"/>
    </step>
    <step>Click the preselected (highlighted) repository to create a new build configuration.</step>
    <step>The <b>Create Project From URL</b> page opens. You can keep the default settings on this form. Click <b>Proceed</b> to create the new build configuration.</step>
    <step><p>The new build configuration opens on the <b>Build Steps</b> page and TeamCity provides a summary of the recently created entities in the notifications at the top of the page.</p>
        <img src="build-on-teamcity-notifications.png" alt="Summary notifications" width="706" border-effect="line"/>
    </step>
</procedure>
        </li>
    </list>
</step>
</procedure>


## Running a TeamCity Build

If a project administrator has already fully configured a TeamCity project and build configuration for your Space repository, you can use the **Build on TeamCity** button to jump to TeamCity and run a build.

1. From the Space repository page, click **Build on TeamCity**.
2. A new tab opens and you are automatically redirected to your organization's instance of TeamCity. Log in, if necessary.
3. TeamCity opens on the build configuration page corresponding to your Space repository.
   > If there is more than one build configuration associated with your Space repository, TeamCity opens the first build configuration it can find. Use the sidebar to navigate to other related builds.
   >
   {type="tip"}

   > If TeamCity directs you to the **Create a Project** page or the **Create a Build Configuration** page, this indicates that the TeamCity connection is not fully configured. See [](#Setting+up+Connections+from+TeamCity ).
   >
   {type="warning"}

4. To initiate a build manually, click **Run** in the top-right corner.
5. For more information about running TeamCity builds, see [Managing Builds](https://www.jetbrains.com/help/teamcity/managing-builds.html).
