[//]: # (title: SSH Exec)
[//]: # (auxiliary-id: SSH Exec)
SSH Exec enables TeamCity to execute arbitrary remote commands using SSH.

The settings common for all runners are described in [Configuring Build Steps](configuring-build-steps.md); this page details the SSH Exec runner settings.

The fields below support [parameter references](predefined-build-parameters.md): any text between percentage signs (`%`) is considered a reference to a property by TeamCity. To prevent TeamCity from treating the text in the percentage signs as reference to a property, use two percentage signs to escape them: for example, if you want to pass "`%Y%m%d%H%M%S`" into the build, change it to "`%%Y%%m%%d%%H%%M%%S`".

<warning>

When strong cryptography is required by the server that receives the deployment, build agents need to have [Java Cryptography Extension (JCE)](https://www.oracle.com/technetwork/java/javase/downloads/jce8-download-2133166.html) Unlimited Strength Jurisdiction Policy installed on the JRE.
</warning>

<table><tr>

<td>

Option

</td>

<td>

Description

</td></tr><tr>

<td>

__Deployment Target__

</td>

<td>

</td>

</tr><tr>

<td>

Target

</td>

<td>


__Target__ should point to an SSH server location. Enter hostname or IP address.


</td></tr><tr>

<td>

Port

</td>

<td>

Optional. By default, port 22 is used.

</td></tr><tr>

<td>

Use pty

</td>

<td>

Optional. Specify the type of the pty terminal. For example, `vt100`.

If empty, pty will not be allocated (default).

</td></tr><tr>

<td>

__Deployment Credentials__

</td>

<td>

The settings in this section will vary depending on the selected authentication method.

</td></tr><tr>

<td>

Authentication method

</td>

<td>


Select an SSH authentication method.

* __Uploaded key__ uses the key(s) uploaded to the project. See [SSH Keys Management](ssh-keys-management.md) for details.
* __Default private key__ will try to perform private key authentication using the `~/.ssh/config` settings. If no settings file exists, will try to use the `~/.ssh/rsa_pub` public key file. No passphrases should be set.
* __Custom private key__ will try to perform private key authentication using the given public key file with given passphrase
* __Password__ – simple password authentication.
* __SSH\-Agent__ – use ssh\-agent for authentication, the [SSH-Agent build feature](ssh-agent.md) must be enabled.


<note>

The current secure connection implementation accepts _any_ certificate provided by the remote host. No trust checks are performed!
</note>


</td></tr><tr>

<td>

__SSH Commands__

</td>

<td>

</td>

</tr><tr>

<td>

Commands

</td>

<td>

Specify a new\-line delimited set of commands that will be executed in the remote shell. The remote shell will be started in the home directory of an authenticated user. The shell output will be available in the TeamCity build log.

</td></tr></table>

__ __