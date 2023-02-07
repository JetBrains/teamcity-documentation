[//]: # (title: SSH Keys Management)
[//]: # (auxiliary-id: SSH Keys Management)

You can upload an SSH private key into a project via the TeamCity web interface and then use it when configuring VCS roots or in the [SSH Agent](ssh-agent.md) build feature.

## Supported Key Format

TeamCity supports keys in the PEM and OpenSSH formats. Keys that use different formats need to be converted. For example, you can use the [PuTTY Key Generator](https://www.puttygen.com/) to convert unsupported Putty private keys (`*.ppk`) to the PEM format. To do this, navigate to the **Conversions | Export OpenSSH key** menu.

## Upload SSH Keys to TeamCity Server

You can use TeamCity web UI or send REST API requests to upload SSH keys.

### TeamCity UI
{id="upload-via-ui"}

1. In __[Project Settings](creating-and-editing-projects.md#Managing+Project)__, click __SSH Keys__. 
2. On the __SSH Keys__ page, click __Upload SSH Key__.
3. In the "_Upload SSH Key_" dialog, browse for a private key file and specify a name for this uploaded key.

<img src="ssh-keys.png" width="706" alt="Add SSH Keys to TeamCity"/>

Uploaded SSH keys are stored in the `<[TeamCity Data Directory](teamcity-data-directory.md)>/config/projects/<project>/pluginData/ssh_keys` directory. TeamCity tracks this directory so uploaded keys become available in the current project and its subprojects without the need to restart a server.


> Keep the access to the [TeamCity Data Directory](teamcity-data-directory.md) secure, as a server stores SSH keys in an unmodified/unencrypted form on the file system.
>
{type="note"}

Once the key is uploaded, a VCS root can be configured to use this uploaded key.


### REST API
{id="upload-via-rest"}

[TeamCity REST API](teamcity-rest-api.md) allows external applications and scripts to access TeamCity resources via URLs.

To upload a new SSH key to a TeamCity server, send the following `POST` request:

```Plain Text
POST <server_URL>/app/rest/projects/<project_locator>/sshKeys?fileName=<Key_Name>
```

* Body: the contents of the private key file
* Content-Type header: "text/plain;"


You can send the following `GET` request to view currently uploaded keys.

```Plain Text
GET <server_URL>/app/rest/projects/<project_locator>/sshKeys
```


## Configure VCS Root Settings

Once required SSH keys are uploaded, assign them to corresponding projects. To do this, modify VCS Root settings.

> Watch our **video tutorial** on [how to check out from SSH repositories](https://www.youtube.com/watch?v=nUTb1BjMMoE) with SSH keys.


### TeamCity UI
{id="configure-vcs-via-ui"}

1. Go to the **Project Settings | VCS Roots** page and click the required root.
2. In the **Authentication Settings** section, click the required "Private Key" option:
   <include src="git.md" include-id="ssh-key-options"/>

<img src="dk-selectSshKeyOptions.png" width="706" alt="Select an SSH key"/>


### REST API
{id="configure-vcs-via-rest"}

Send the following REST API requests to set up VCS authentication settings.

* Send the PUT request with the "TEAMCITY_SSH_KEY" body to `/properties/authMethod`. This request switches the "Authentication method" to "Uploaded Key".
   
   ```Plain Text
   PUT <server_URL>/app/rest/vcs-roots/<locator>/properties/authMethod
   ```

* To select a particular SSH key, send another PUT request with the name of this key as the request body to `/properties/teamcitySshKey`.
   
   ```Plain Text
   PUT <server_URL>/app/rest/vcs-roots/<locator>/properties/teamcitySshKey
   ```

* For password-encrypted private keys, send a passphrase as the body of the `/properties/secure:passphrase` request.
   
   ```Plain Text
   PUT <server_URL>/app/rest/vcs-roots/<locator>/properties/secure:passphrase
   ```








## SSH Key Usage

For information on using SSH keys from within the build scripts, see the following documentation article: [SSH Agent](ssh-agent.md).

If you configure the [agent-side checkout](vcs-checkout-mode.md#agent-checkout), the server passes SSH keys to agents. During a build, the Git plugin downloads the key from the server to the agent, and removes this key after `git fetch/clone` is complete.



> Keys are removed from agents for security reasons. For example, the tests executed by the build can leave some malicious code that will access the build agent file system and acquire the key. However, tests cannot get the key directly since it is removed by the time they are running. It makes it harder but not impossible to steal the key. Therefore, the agent must also be secure.
> 
{type="note"}

To transfer the key from the server to the agent, TeamCity encrypts it with a DES symmetric cipher. For a more secure way, configure an [HTTPS connection between agents and the server](using-https-to-access-teamcity-server.md).
