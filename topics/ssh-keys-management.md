[//]: # (title: SSH Keys Management)
[//]: # (auxiliary-id: SSH Keys Management)

You can upload an SSH private key into a project via the TeamCity web interface and then use it when configuring VCS roots or in the [SSH Agent](ssh-agent.md) build feature.

## Supported Key Format

TeamCity supports keys in the PEM and OpenSSH formats. If your private key uses a different format, it has to be converted. For example, the Putty private key format (`*.ppk`), not supported by TeamCity, can be converted to the PEM format using [PuTTY Key Generator](https://www.puttygen.com/): use the menu  __Conversions | Export OpenSSH key__.

## Upload SSH Keys to TeamCity Server

Before you can start using an SSH key, you need to upload it to a server.

### Upload Keys via Teamcity UI

1. In __[Project Settings](creating-and-editing-projects.md#Managing+Project)__, click __SSH Keys__. 
2. On the __SSH Keys__ page, click __Upload SSH Key__.
3. In the "_Upload SSH Key_" dialog, browse for a private key file and specify a name for this uploaded key.

<img src="ssh-keys.png" width="706" alt="Add SSH Keys to TeamCity"/>

Uploaded SSH keys are stored in the `<[TeamCity Data Directory](teamcity-data-directory.md)>/config/projects/<project>/pluginData/ssh_keys` directory. TeamCity tracks this directory and uploaded keys become available in the current project and its subprojects without the need to restart a server.


> The access to the [TeamCity Data Directory](teamcity-data-directory.md) must be kept secure, as the keys are stored in an unmodified/unencrypted form on the file system.
>
{type="note"}

Once the key is uploaded, a VCS root can be configured to use this uploaded key.


### Upload Keys via REST API

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

1. Go to the **Project Settings | VCS Roots** page and click the required root.
2. In the **Authentication Settings** section, click a required "Private Key" option:
   <include src="git.md" include-id="ssh-key-options"/>

<img src="dk-selectSshKeyOptions.png" width="706" alt="Select an SSH key"/>


### REST API

Send the following requests to choose which of uploaded SSH should be used.

```Plain Text
  PUT <server_URL>/app/rest/vcs-roots/<locator>/properties/authMethod
  ```

* Request body: TEAMCITY_SSH_KEY
* Sets the "Authentication method" to "Uploaded Key"

```Plain Text
PUT <server_URL>/app/rest/vcs-roots/<locator>/properties/teamcitySshKey
```

* Request body: the name of an uploaded SSH key
* Selects the particular uploaded key

```Plain Text
PUT <server_URL>/app/rest/vcs-roots/<locator>/properties/secure:passphrase
```

* Request body: key passphrase
* A password for encrypted private keys.







## SSH Key Usage

See [SSH Agent](ssh-agent.md) for usage from within the build scripts.

The uploaded key can be used in a VCS root. SSH key is used on the server and is also passed to the agent in case [agent-side checkout](vcs-checkout-mode.md#agent-checkout) is configured.

During the build with agent-side checkout, the Git plugin downloads the key from the server to the agent. It temporarily saves the key on the agent's file system and removes it after `git fetch/clone` is completed.



> The key is removed for security reasons: for example, the tests executed by the build can leave some malicious code that will access the build agent file system and acquire the key. However, tests cannot get the key directly since it is removed by the time they are running. It makes it harder but not impossible to steal the key. Therefore, the agent must also be secure.
> 
{type="note"}

To transfer the key from the server to the agent, TeamCity encrypts it with a DES symmetric cipher. For a more secure way, configure an [HTTPS connection between agents and the server](using-https-to-access-teamcity-server.md).
