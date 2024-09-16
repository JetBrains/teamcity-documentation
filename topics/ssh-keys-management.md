[//]: # (title: SSH Keys Management)
[//]: # (auxiliary-id: SSH Keys Management)

You can upload private SSH keys into TeamCity projects. Uploaded keys can be used when configuring VCS roots, and in the [SSH Agent](ssh-agent.md) build feature.



## Supported Key Format

TeamCity supports keys in the PEM and OpenSSH formats. Keys that use different formats need to be converted. For example, you can use the [PuTTY Key Generator](https://www.puttygen.com/) to convert unsupported Putty private keys (`*.ppk`) to the PEM format. To do this, navigate to the **Conversions | Export OpenSSH key** menu.

## Upload SSH Keys to TeamCity Server

To allow TeamCity projects to access remote repositories via SSH URLs, you first need to upload your private keys to these projects.

1. In __[Project Settings](creating-and-editing-projects.md#Managing+Project)__, click __SSH Keys__.
2. On the __SSH Keys__ page, click __Upload SSH Key__.
   <img src="ssh-keys.png" width="706" alt="Add SSH Keys to TeamCity"/>
3. In the "_Upload SSH Key_" dialog, browse for a private key file and specify a name for this key.
4. Click **Save** to save the uploaded key.

You can also upload private keys when creating new projects [from repository URLs](creating-and-editing-projects.md#Creating+project+pointing+to+repository+URL). If you use an SSH URL, TeamCity switches the **Authentication** mode to "SSH Keys" and shows a list of the previously uploaded private keys. If you did not upload a required key before, click **Upload SSH key** and point TeamCity to the required file.

<img src="dk-ssh-on-new-project.png" width="706" alt="Upload SSH key on new project page"/>




Uploaded SSH keys are stored in the following directory:

* `[<TeamCity Data Directory>](teamcity-data-directory.md)/config/projects/<project>/pluginData/ssh_keys`

If a key was uploaded from the "Create Project" page, TeamCity assigns it to the parent project of the created project. In this case, uploaded keys are saved to the following directory:

* `[<TeamCity Data Directory>](teamcity-data-directory.md)/config/projects/<Parent_Project_ID>/pluginData/ssh_keys`



> Keep the access to the [TeamCity Data Directory](teamcity-data-directory.md) secure, as a server stores SSH keys in an unmodified/unencrypted form on the file system.
>
{style="note"}


## Generated SSH Keys

If you use [GitHub Deploy Keys](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/managing-deploy-keys#deploy-keys) or similar authentication workflows, you can let TeamCity generate SSH keys instead of generating them manually. This approach is more secure (since generated keys are not stored on your local machine) and significantly faster. The latter is especially helpful if you re-generate and rotate SSH keys every once in a while.

1. In __[Project Settings](creating-and-editing-projects.md#Managing+Project)__, click __SSH Keys__.
2. Click the **Generate SSH Key** button.
    
    <img src="dk-generateSshKeys.png" width="706" alt="Generate SSH Key"/>

3. Enter the key name, select the key type, and click **Generate**.
4. If you need a private or a public key for your new generated key:

   * A private key is stored in the `[Data Directory](teamcity-data-directory.md)/config/projects/<parent project>/pluginData/ssh_keys` directory.
   * A public key is accessible from the main **SSH Keys** page (click the **Copy the public key** link under a required key). Paste this key to your version control service (for example, in GitHub: "Repository settings | Deploy Keys | Add deploy key").

5. Select the generated key in the [](git.md#Authentication+Settings) of your TeamCity VCS root. For your convenience, keys generated in TeamCity are placed in a separate category.
    
    <img src="dk-generatedKeysInVcs.png" width="706" alt="Choose Generated Keys in VCS Auth Settings"/>

## Configure VCS Root Settings

> Watch our **video tutorial** on [how to check out from SSH repositories](https://www.youtube.com/watch?v=nUTb1BjMMoE) with SSH keys.

Private SSH keys are employed only when your VCS root is configured to work with a remote repository via an SSH URL (for instance, `git@github.com:...`). If you create a project using an SSH URL and choose/upload a private key from the "Create Project" page, TeamCity automatically sets up required settings, and you do not need to modify a [](vcs-root.md).

Otherwise, if you want to update a project that authenticates to a VCS via a token or a password so that it starts using an SSH key instead, you will need to manually update a corresponding [](vcs-root.md).

1. Navigate to **Administration | &lt;Your_Project&gt; | VCS Roots** and click **Edit** next to the required root.
2. Update the **Fetch URL** and/or **Push URL** fields to use an SSH URL instead. For example, change `https://github.com/username/repository_name.git` to `git@github.com:username/repository_name.git`.
3. Scroll down to **Authentication Settings** and choose one of the **Private Key** options.
    
    <img src="dk-selectSshKeyOptions.png" width="706" alt="Select an SSH key"/>
    
    <include from="git.md" element-id="ssh-key-options"/>

4. Click **Test Connection** at the bottom of the page to check you current values, and **Apply** to save and exit the root settings.





## Distribute SSH Keys to Build Agents

If you configure the [agent-side checkout](vcs-checkout-mode.md#agent-checkout), the server passes SSH keys to agents. During a build, the Git plugin downloads the key from the server to the agent, and removes this key after `git fetch/clone` is complete.



> Keys are removed from agents for security reasons. For example, the tests executed by the build can leave some malicious code that will access the build agent file system and acquire the key. However, tests cannot get the key directly since it is removed by the time they are running. It makes it harder but not impossible to steal the key. Therefore, the agent must also be secure.
>
{style="note"}

To transfer the key from the server to the agent, TeamCity encrypts it with a DES symmetric cipher. For a more secure way, configure an [HTTPS connection between agents and the server](using-https-to-access-teamcity-server.md).

In addition to VCS roots, uploaded SSH keys can be used in **SSH Agent** build features. See this link for more information: [SSH Agent](ssh-agent.md).



## REST API

[TeamCity REST API](teamcity-rest-api.md) allows external applications and scripts to access TeamCity resources via URLs. You can utilize this feature to upload SSH keys and customize VCS Root settings.


### View Uploaded Keys


```Plain Text
/app/rest/projects/<project_locator>/sshKeys
```
{prompt="GET"}

### Upload New SSH Keys to a Project

```Plain Text
/app/rest/projects/<project_locator>/sshKeys?fileName=<Key_Name>
```
{prompt="POST"}

* Body: the contents of the private key file
* Content-Type header: "text/plain"


### Generate a New Key

```Plain Text
/app/rest/projects/<project_locator>/sshKeys/generated?keyName=NewKey&keyType=RSA
```
{prompt="POST"}

* `keyName` — any valid string that is your new key's name.
* `keyType` — either "RSA" or "ED25519".

### Set up VCS Authentication Settings

* Switch the "Authentication method" to "Uploaded Key". Request body: "TEAMCITY_SSH_KEY".

    ```Plain Text
    /app/rest/vcs-roots/<locator>/properties/authMethod
    ```
  {prompt="PUT"}

* Select a particular SSH key. Request body: SSH key name.

    ```Plain Text
    /app/rest/vcs-roots/<locator>/properties/teamcitySshKey
    ```
  {prompt="PUT"}

* Specify a passphrase required by password-encrypted SSH keys. Request body: plain password string.

    ```Plain Text
    /app/rest/vcs-roots/<locator>/properties/secure:passphrase
    ```
  {prompt="PUT"}

### Delete a Key

```Plain Text
/app/rest/projects/<project_locator>/sshKeys?fileName=<Key_Name>
```
{prompt="DELETE"}

