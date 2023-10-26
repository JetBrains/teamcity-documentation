[//]: # (title: Build on TeamCity)
[//]: # (auxiliary-id: Build on TeamCity)

## Integration with TeamCity


## Running a TeamCity Build

If a project administrator has already fully configured a TeamCity project and build configuration for your Space repository, you can use the **Build on TeamCity** button to jump to TeamCity and run a build or view the history of TeamCity builds.

<procedure title="Run a TeamCity Build">
    <step>From the Space repository page, click <b>Build on TeamCity</b>.</step>
    <step>A new tab opens and you are automatically redirected to your organization's instance of TeamCity. Log in, if necessary.</step>
    <step><p>TeamCity opens on the build configuration page corresponding to your Space repository</p>
        <tip>If there is more than one build configuration associated with your Space repository, TeamCity opens the first build configuration it can find. Use the sidebar to navigate to other related builds.</tip>
        <warning>If TeamCity directs you to the <b>Create a Project</b> page or the <b>Create a Build Configuration</b> page, this indicates that the TeamCity connection is not fully configured. See <a href="#Setting+up+Connections+from+TeamCity">Setting up Connections from TeamCity</a></warning>
    </step>
    <step>To initiate a build manually, click <b>Run</b> in the top-right corner.</step>
    <step>For more information about running TeamCity builds, see <a href="https://www.jetbrains.com/help/teamcity/managing-builds.html">Managing Builds</a>.</step>
</procedure>



## Setting up Connections from TeamCity

<procedure title="Setting up the TeamCity Integration">
    <step>From the Space repository page, click <b>Build on TeamCity</b>.</step>
    <step>A new tab opens and you are automatically redirected to your organization's instance of TeamCity. Log in, if necessary.</step>
    <step><p>Teamcity opens either on the <b>Create a Project</b> page or the <b>Create a Build Configuration</b> page. The next step depends on the level of connection that already exists between TeamCity and Space:</p>
        <list type="alpha-lower">
            <li>If you see a <b>Connect</b> button at the bottom of the page, proceed to <a href="#Creating+an+Organization-Level+Connection">Creating an Organization-Level Connection</a></li>
            <li>If you see a <b>Sign in to Space</b> button at the bottom of the page, proceed to <a href="#Creating+a+Project-Level+Connection">Creating a Project-Level Connection</a></li>
            <li>If you see a preselected repository at the bottom of the page, proceed to <a href="#Creating+a+Build+Configuration">Creating a Build Configuration</a></li>
        </list>
    </step>
</procedure>


### Creating an Organization-Level Connection

NOTES:
- Org connections can be created at different project levels
  Need to add a note about project levels here.
- What about [Authenticating in TeamCity with JetBrains Space Account](). Is this also something we should recommend here?
  Or is it automatically set up for you?

To perform this procedure, you need to have:

* _Project administration rights_ in the JetBrains Space instance of your organization.
* _Project administration rights_ in either the [TeamCity Cloud instance](https://www.jetbrains.com/teamcity/signup/) or [TeamCity On-Premises server](https://www.jetbrains.com/teamcity/download/) of your organization.
* To enable authentication via JetBrains Space in TeamCity, _system administration rights_ in TeamCity.


<procedure title="Create JetBrains Space Connection in TeamCity">
    <step>From the Space repository page, click <b>Build on TeamCity</b>.</step>
    <step>A new tab opens and you are automatically redirected to your organization's instance of TeamCity. Log in, if necessary.</step>
    <step><p>TeamCity opens on the <b>Create a Project</b> page and, in the <b>Choose a repository</b> section, you see the hint <i>To show available repositories TeamCity requires a connection with JetBrains Space</i>. Click the <b>Connect</b> button.</p>
    <img src="build-on-teamcity-org-level.png" alt="TeamCity requires a connection with JetBrains Space" width="706" border-effect="line"/>
    </step>
    <step><p>A new window opens, showing a dialog that prompts you to install a new TeamCity application in Space. Enter a name for the TeamCity application and click <b>Install</b>.</p>
     <note>
        The certificate associated with the TeamCity HTTPS endpoint must be signed by a well-known certificate authority, otherwise this step raises an error.
     </note>
    </step>
    <step>If the install step succeeds, the dialog status changes to <i>Successfully installed</i>. Click <b>Approve all and go back to TeamCity</b>.</step>
    <step>You are redirected back to the <b>Create a Project</b> page on TeamCity. The <b>Choose a repository</b> section now shows the hint <i>To show available projects TeamCity requires access to your Space account</i> and the <b>Sign in to Space</b> button.</step>
    <step>Proceed to <a href="#Creating+a+Project-Level+Connection">Creating a Project-Level Connection</a>.</step>
</procedure>

### Creating a Project-Level Connection

NOTES:
- This step does the following:
  - (Optionally) creates a new TeamCity project
  - In the new/existing TC project, create a connection to JetBrains Space project.
  - Create a corresponding application in Space

To perform this procedure, you need to have:

* _Project administration rights_ in the JetBrains Space instance of your organization.
* _Project administration rights_ in either the [TeamCity Cloud instance](https://www.jetbrains.com/teamcity/signup/) or [TeamCity On-Premises server](https://www.jetbrains.com/teamcity/download/) of your organization.


<procedure title="Create Project Connection in TeamCity">
    <step>From the Space repository page, click <b>Build on TeamCity</b>.</step>
    <step>A new tab opens and you are automatically redirected to your organization's instance of TeamCity. Log in, if necessary.</step>
    <step><p>TeamCity opens on the <b>Create a Project</b> page. If this is the first time you are connecting a TeamCity project to Space, you need to authenticate your user profile in Space. Click <b>Sign in to Space</b> and accept the access request.</p>
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
    <step>Proceed to <a href="#Creating+a+Build+Configuration">Creating a Build Configuration</a>.</step>
</procedure>


### Creating a Build Configuration 

NOTES:
- Depending on the starting context, you might begin this workflow either on the "Create Project" page or the "Create a Build Configuration" page.
- Actually, after the project has been created, if I start the workflow again, I get directed to the "Create Project" page again. When I follow the steps, I end up with a new sub-project of the existing project. E.g. a new `<Root Project> / JetBrains Demo / JetBrains Demo` project.

To perform this procedure, you need to have:

* _Project administration rights_ in either the [TeamCity Cloud instance](https://www.jetbrains.com/teamcity/signup/) or [TeamCity On-Premises server](https://www.jetbrains.com/teamcity/download/) of your organization.

<procedure title="Create a Build Configuration in TeamCity">
    <step>From the Space repository page, click <b>Build on TeamCity</b>.</step>
    <step><p>A new tab opens and you are automatically redirected to your organization's instance of TeamCity. Log in, if necessary.</p>
    </step>
    <step>
        TeamCity opens either on the <b>Create a Project</b> page or the <b>Create a Build Configuration</b> page, depending on the context.
        <img src="build-on-teamcity-build-config.png" alt="Click here to create a new build configuration" width="706" border-effect="line"/>
    </step>
    <step>
        <p>The <b>Choose a Space project</b> section shows the list of remote Space projects that are available to connect to this project, with your current Space project preselected. The <b>Choose a repository</b> section shows the list of repositories from the selected Space project, with your current Space repository preselected.</p>
    </step>
    <step>Click the preselected (highlighted) repository to create a new build configuration.</step>
    <step>The <b>Create Project From URL</b> page opens. You can keep the default settings on this form. Click <b>Proceed</b> to create the new build configuration.</step>
    <step><p>The new build configuration opens on the <b>Build Steps</b> page and TeamCity provides a summary of the recently created entities in the notifications at the top of the page.</p>
        <img src="build-on-teamcity-notifications.png" alt="Summary notifications" width="706" border-effect="line"/>
    </step>
</procedure>
