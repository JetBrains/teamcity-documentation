[//]: # (title: SSH Keys Management)
[//]: # (auxiliary-id: SSH Keys Management)

You can upload an SSH private key into a project via the TeamCity web interface and then use it when configuring VCS roots or in the [SSH Agent](ssh-agent.md) build feature.

## Supported Key Format

TeamCity supports keys in the PEM and OpenSSH formats. If your private key uses a different format, it has to be converted. For example, the Putty private key format (`*.ppk`), not supported by TeamCity, can be converted to the PEM format using [PuTTY Key Generator](https://www.puttygen.com/): use the menu  __Conversions | Export OpenSSH key__.

## Uploading SSH Key to TeamCity Server

1. In __[Project Settings](creating-and-editing-projects.md#Managing+Project)__, click __SSH Keys__. 
2. On the __SSH Keys__ page, click __Upload SSH Key__.
3. In the "_Upload SSH Key_" dialog, select a private key (usually stored in `<USER_HOME>/.ssh/id_rsa` or `<USER_HOME>/.ssh/id_dsa`).

When you upload an SSH key for a project, it is stored in `<[TeamCity Data Directory](teamcity-data-directory.md)>/config/projects/<project>/pluginData/ssh_keys`. TeamCity tracks this directory and is able to pick up new keys on-the-fly. The key will be available in the current project and its subprojects.

<note>

The access to the [TeamCity Data Directory](teamcity-data-directory.md) must be kept secure, as the keys are stored in an unmodified/unencrypted form on the file system.
</note>

Once the key is uploaded, a VCS root can be configured to use this uploaded key.

>Watch our **video tutorial** on [how to check out from SSH repositories](https://www.youtube.com/watch?v=nUTb1BjMMoE) with SSH keys.

_(Since TeamCity Cloud 2022.02)_ On the __SSH Keys__ page, you can copy the public part of an uploaded non-encrypted SSH key using the __Copy the public key__ link under the key name.

<img src="ssh-keys.png" width="706" alt="SSH Keys"/>

## SSH Key Usage

See [SSH Agent](ssh-agent.md) for usage from within the build scripts.

The uploaded key can be used in a VCS root. SSH key is used on the server and is also passed to the agent in case [agent-side checkout](vcs-checkout-mode.md#agent-checkout) is configured.

During the build with agent-side checkout, the Git plugin downloads the key from the server to the agent. It temporarily saves the key on the agent's file system and removes it after `git fetch/clone` is completed.

<note>

The key is removed for security reasons: for example, the tests executed by the build can leave some malicious code that will access the build agent file system and acquire the key. However, tests cannot get the key directly since it is removed by the time they are running. It makes it harder but not impossible to steal the key. Therefore, the agent must also be secure.
</note>

To transfer the key from the server to the agent, TeamCity encrypts it with a DES symmetric cipher. For a more secure way, configure an [HTTPS connection between agents and the server](using-https-to-access-teamcity-server.md).
