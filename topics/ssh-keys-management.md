[//]: # (title: SSH Keys Management)
[//]: # (auxiliary-id: SSH Keys Management)

You can upload private SSH keys into TeamCity projects. Uploaded keys can be used for VCS Root configurations, and in the [SSH Agent](ssh-agent.md) build feature.



## Supported Key Format

TeamCity supports keys in the PEM and OpenSSH formats. Keys that use different formats need to be converted. For example, you can use the [PuTTY Key Generator](https://www.puttygen.com/) to convert unsupported Putty private keys (`*.ppk`) to the PEM format. To do this, navigate to the **Conversions | Export OpenSSH key** menu.

## Upload SSH Keys to TeamCity Server

To allow your projects to access remote repositories via SSH keys, you first need to upload these keys to your server.

1. In __[Project Settings](creating-and-editing-projects.md#Managing+Project)__, click __SSH Keys__.
2. On the __SSH Keys__ page, click __Upload SSH Key__.
3. In the "_Upload SSH Key_" dialog, browse for a private key file and specify a name for this key.
4. Click **Save** to save the uploaded key.

<img src="ssh-keys.png" width="706" alt="Add SSH Keys to TeamCity"/>

Uploaded SSH keys are stored in the `<[TeamCity Data Directory](teamcity-data-directory.md)>/config/projects/<project>/pluginData/ssh_keys` directory. TeamCity tracks this directory so uploaded keys become available in the current project and its subprojects without the need to restart a server.

> Keep the access to the [TeamCity Data Directory](teamcity-data-directory.md) secure, as a server stores SSH keys in an unmodified/unencrypted form on the file system.
>
{type="note"}

## Configure VCS Root Settings

Once required SSH keys are uploaded, modify the VCS Root settings to select a key that your project should use.


> Private SSH keys are employed only when your VCS root is configured to work with a remote repository via an SSH URL (for instance, `git@github.com:...`). If your "Fetch URL" / "Push URL" in **Project Settings** are set to HTTPS (for instance, `https://github.com/...`), authorization with SSH keys is disabled.
> 
{type="note"}

> Watch our **video tutorial** on [how to check out from SSH repositories](https://www.youtube.com/watch?v=nUTb1BjMMoE) with SSH keys.

1. Go to the **Project Settings | VCS Roots** page and click the required root.
2. In the **Authentication Settings** section, click the required "Private Key" option:
   <include src="git.md" include-id="ssh-key-options"/>

<img src="dk-selectSshKeyOptions.png" width="706" alt="Select an SSH key"/>



## REST API 

[TeamCity REST API](teamcity-rest-api.md) allows external applications and scripts to access TeamCity resources via URLs. You can utilize this feature to upload SSH keys and customize VCS Root settings.

### View Uploaded Keys
{initial-collapse-state="collapsed"}


```Plain Text
GET <server_URL>/app/rest/projects/<project_locator>/sshKeys
```

### Upload New SSH Keys to TeamCity Server
{initial-collapse-state="collapsed"}

```Plain Text
POST <server_URL>/app/rest/projects/<project_locator>/sshKeys?fileName=<Key_Name>
```

* Body: the contents of the private key file
* Content-Type header: "text/plain"



### Set up VCS Authentication Settings
{initial-collapse-state="collapsed"}

* Switch the "Authentication method" to "Uploaded Key". Request body: "TEAMCITY_SSH_KEY".
    
    ```Plain Text
    PUT <server_URL>/app/rest/vcs-roots/<locator>/properties/authMethod
    ```

* Select a particular SSH key. Request body: SSH key name.
    
    ```Plain Text
    PUT <server_URL>/app/rest/vcs-roots/<locator>/properties/teamcitySshKey
    ```

* Specify a passphrase required by password-encrypted SSH keys. Request body: plain password string.
    
    ```Plain Text
    PUT <server_URL>/app/rest/vcs-roots/<locator>/properties/secure:passphrase
    ```


## Distribute SSH Keys to Build Agents

If you configure the [agent-side checkout](vcs-checkout-mode.md#agent-checkout), the server passes SSH keys to agents. During a build, the Git plugin downloads the key from the server to the agent, and removes this key after `git fetch/clone` is complete.



> Keys are removed from agents for security reasons. For example, the tests executed by the build can leave some malicious code that will access the build agent file system and acquire the key. However, tests cannot get the key directly since it is removed by the time they are running. It makes it harder but not impossible to steal the key. Therefore, the agent must also be secure.
> 
{type="note"}

To transfer the key from the server to the agent, TeamCity encrypts it with a DES symmetric cipher. For a more secure way, configure an [HTTPS connection between agents and the server](using-https-to-access-teamcity-server.md).

For more information on how keys are distributed, see this documentation article: [SSH Agent](ssh-agent.md).
