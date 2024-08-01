[//]: # (title: SSH Upload)
[//]: # (auxiliary-id: SSH Upload)

The _SSH Upload_ build runner allows uploading files/directories via SSH (using SCP or SFTP protocols).

The settings common for all runners are described in [Configuring Build Steps](configuring-build-steps.md); this page details the SSH Deployer settings.

The fields below support [parameter references](predefined-build-parameters.md): any text between percentage signs (`%`) is considered a reference to a property by TeamCity. To prevent TeamCity from treating the text in the percentage signs as reference to a property, use two percentage signs to escape them: for example, if you want to pass `%\Y%m%\d%H%\M%S` into the build, change it to `%\%Y%\%m%\%d%\%H%\%M%\%S`.

<warning>

When strong cryptography is required by the server that receives the deployment, build agents need to have [Java Cryptography Extension (JCE) ](https://www.oracle.com/technetwork/java/javase/downloads/jce8-download-2133166.html) Unlimited Strength Jurisdiction Policy installed on the JRE.
</warning>

<table><tr>

<th width="200"><p><b>Option</b></p></th>

<th><p><b>Description</b></p></th>
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

<p>SSH server location where the files will be uploaded, specified in the format:</p>

```Shell
{hostname|IP_address}[:targer_dir[/sub_path]] 

```

where `target_dir` can be absolute or relative and `sub_path` can have any depth.

</td></tr><tr>

<td>

Transport protocol

</td>

<td>

Select a protocol to transfer data over SSH. The available options are: SCP and SFTP

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
<p>Timeout</p>
</td>
<td>
<p><i>Optional</i> Timeout for the connection in seconds. Defaults to 0.</p>
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

__Deployment Source__

</td>
</tr>

<tr>
<td>

Paths to sources

</td>

<td>

Specify the deployment sources as a newline- or comma-separated list of paths to files/directories.

The field supports [Ant-style wildcard patterns](wildcards.md#Antlike+Wildcards) (for example, `dir/**/*.zip`).   
You can also specify a target directory to be created using the `file => directory` pattern (for example, `*.zip => winFiles,unix/distro.tgz => linuxFiles` will create the `winFiles` and `linuxFiles` directories, and respectively put the declared files inside them).

</td></tr></table>

### Example

For example, consider the case where you need to add an SSH Upload build step to upload Java packages to an SSH server (on the `ssh.example.com` host). Suppose you use the `jdoe` account on the SSH server with the home directory `/jdoe`, and the SSH server is configured to use SSH keys for authentication.

1. Follow the steps in [Generated SSH Keys](ssh-keys-management.md#Generated+SSH+Keys) to generate a new SSH key pair in your project. Call the key pair `PackageUploadKey`.
2. Copy the public key from the `PackageUploadKey` key pair.
3. Log in to the `jdoe` account on your SSH server and follow the instructions from your SSH server provider to add the `PackageUploadKey` public key to this account.
3. In your project's build configuration, go to the **Build Steps** page and click **Add build step**.
4. On the **New Build Step** page, select the **SSH Upload** runner.
5. On the **New Build Step: SSH Upload** page, fill in the fields, as follows:
   * **Step name** — Enter `UploadJavaPackages`
   * **Target** — Enter `ssh.example.com:/jdoe`
   * **Authentication method** — Select _Uploaded key_
   * **Username** — Enter `jdoe` (the username for the account on the SSH server)
   * **Select key** — Select `PackageUploadKey` from the dropdown list
   * **Paths to sources** — Enter the following paths:
      ```
      ch-simple/simple/target/*.jar => packages
      ```
6. Click **Save** to create the build step.
