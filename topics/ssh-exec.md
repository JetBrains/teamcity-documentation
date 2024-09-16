[//]: # (title: SSH Exec)
[//]: # (auxiliary-id: SSH Exec)

The _SSH Exec_ enables TeamCity to execute arbitrary remote commands using SSH.

The settings common for all runners are described [here](configuring-build-steps.md). This article details the SSH Exec runner settings.

The fields below support [parameter references](predefined-build-parameters.md): any text between percentage signs (`%`) is considered a reference to a property by TeamCity. To prevent TeamCity from treating the text in the percentage signs as reference to a property, use two percentage signs to escape them: for example, if you want to pass `%\Y%m%\d%H%\M%S` into the build, change it to `%\%Y%\%m%\%d%\%H%\%M%\%S`.

>Watch our **video guide** on how to [use SSH during your builds](https://www.youtube.com/watch?v=D6JOyGd4pWI).

<warning>

When strong cryptography is required by the server that receives the deployment, build agents need to have [Java Cryptography Extension (JCE)](https://www.oracle.com/technetwork/java/javase/downloads/jce8-download-2133166.html) Unlimited Strength Jurisdiction Policy installed on the JRE.
</warning>

<table><tr>

<th width="200">
<p><b>Option</b></p>
</th>

<th>
<p><b>Description</b></p>
</th>
</tr>

<tr>
<td>
<p>Step name</p>
</td>
<td>
<p><i>Optional</i> Name of the build step displayed in the TeamCity UI.</p>
</td>
</tr>

<tr>
<td>
<p>Step ID</p>
</td>
<td>
<p>ID for this build step, which must be unique across all steps of this configuration. Used in URLs, REST API, DSL, HTTP requests to the server, and configuration settings in the TeamCity Data Directory.</p>
</td>
</tr>

<tr>
<td>

<p>Execute step</p>

</td>
<td>

<p>Enables you to modify the default build condition, and optionally add more <a href="configuring-build-steps.md#Execution+Policy">build conditions</a>.</p>

</td>
</tr>

<tr>

<td colspan="2">

__Deployment Target__

</td>
</tr>

<tr>
<td>

Target

</td>

<td>

SSH server hostname or IP address.

</td></tr>

<tr>
<td>
<p>Port</p>
</td>
<td>
<p><i>Optional</i> Port. Defaults to port 22.</p>
</td>
</tr>

<tr>
<td>

Use pty

</td>

<td>

<i>Optional</i> Specify the type of the pseudoterminal (pty). For example, `vt100`.

If empty, pty is not allocated (default).

</td>
</tr>


<tr>

<td colspan="2">

__Deployment Credentials__

</td>

</tr>


<tr>
<td>

Authentication method

</td>

<td>


Select an authentication method:

* __Uploaded Key__ — uses the key(s) uploaded to the project. See [SSH Keys Management](ssh-keys-management.md) for details.
* __Default Private Key__ — performs private key authentication using the `~/.ssh/config` settings or, if no settings file exists, using the `~/.ssh/id_rsa` private key file.
* __Custom Private Key__ — performs private key authentication using the specified private key file and passphrase.
* __Password__ — uses simple password authentication.
* __SSH-Agent__ — uses SSH agent for authentication, where the [SSH-Agent build feature](ssh-agent.md) must be enabled.

<note>

The current secure connection implementation accepts _any_ certificate provided by the remote host. No trust checks are performed!
</note>

</td>
</tr>

<tr>

<td colspan="2">

__SSH Commands__

</td>
</tr>

<tr>
<td>

Commands

</td>

<td>

<p>Specify a new-line delimited set of commands to execute in the remote shell. The remote shell is started in the home directory of an authenticated user. The shell output will be available in the TeamCity build log.</p>

<note><p>SSH Exec runs the shell in non-interactive mode, which imposes corresponding restrictions.</p></note>

<tip>

__Tip for Linux users__

To use bash aliases in this runner, add the following shell options to your script:

```Shell
shopt -s expand_aliases
source /home/user/.bash_profile

```

</tip>

</td></tr></table>

### Example

For example, consider how to create a build step that builds static content for a web site. After uploading the static content to the web server, you need to execute a `deploy.sh` script to refresh the site. Suppose you use the `jdoe` account on the SSH server with the home directory `/jdoe`, and the SSH server is configured to use SSH keys for authentication.

1. Follow the steps in [Generated SSH Keys](ssh-keys-management.md#Generated+SSH+Keys) to generate a new SSH key pair in your project. Call the key pair `WebServerKey`.
2. Copy the public key from the `WebServerKey` key pair.
3. Log in to the `jdoe` account on your SSH server and follow the instructions from your SSH server provider to add the `WebServerKey` public key to this account.
3. In your project's build configuration, go to the **Build Steps** page and click **Add build step**.
4. On the **New Build Step** page, select the **SSH Exec** runner.
5. On the **New Build Step: SSH Exec** page, fill in the fields, as follows:
    * **Step name** — Enter `RunDeployScript`
    * **Target** — Enter `ssh.example.com`
    * **Authentication method** — Select _Uploaded key_
    * **Username** — Enter `jdoe` (the username for the account on the SSH server)
    * **Select key** — Select `WebServerKey` from the dropdown list
    * **Commands** — Enter the following shell commands:

       ```
       echo 'running deploy.sh ...'
       /home/jdoe/scripts/deploy.sh
       ```
6. Click **Save** to create the build step.
