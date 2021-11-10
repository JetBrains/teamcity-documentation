[//]: # (title: SMB Upload)
[//]: # (auxiliary-id: SMB Upload)

The _SMB Upload_ build runner enables TeamCity to upload files/directories to Windows shares via Server Message Block (SMB) protocol. 

The settings common for all runners are described in [Configuring Build Steps](configuring-build-steps.md); this page details the SMB Upload settings.

>The fields below support [parameter references](predefined-build-parameters.md): any text between percentage signs (%) is considered a reference to a property by TeamCity. To prevent TeamCity from treating the text in the percentage signs as a property reference, use two percentage signs to escape them: for example, if you want to pass `%\Y%m%\d%H%\M%S` into the build, change it to `%\%Y%\%m%\%d%\%H%\%M%\%S`.

<table><tr>

<td width="200">

Option

</td>

<td>

Description

</td></tr><tr>

<td>

__Deployment Target__

</td>

<td></td>

</tr><tr>

<td>

Target URL

</td>

<td>

The URL should point to a host \+ share at least. Subdirectories are allowed here and will be created if missing. Valid examples:


```Shell
\\host\share_name
\\host\share_name\some\path
\\host\c$\Temp
```

Note that the files with matching names will be rewritten in the target directory.

</td></tr><tr>

<td>

Name resolution

</td>

<td>

The __DNS only name resolution__ allows switching  JCIFS to "DNS-only" mode. May fix performance or out of memory exceptions (see [this bitbucket issue](https://bitbucket.org/nskvortsov/deployer/issue/20/out-of-memory-exception) for details). Is equivalent to following JCIFS settings:

```Shell
-Djcifs.resolveOrder=DNS
-Djcifs.smb.client.dfs.disabled=true
```

</td></tr><tr>

<td>

__Deployment Credentials__

</td>

<td></td>

</tr><tr>

<td>

Username

</td>

<td>

Specify the username

</td></tr><tr>

<td>

Password

</td>

<td>

Specify the password

</td></tr><tr>

<td>

Domain

</td>

<td>

Specify the __domain__

</td></tr><tr>

<td>

__Deployment Source__

</td>

<td></td>

</tr><tr>

<td>

Path to sources

</td>

<td>

Specify the deployment sources as a newline- or comma-separated list of paths to files/directories.

The field supports [Ant-style wildcard patterns](wildcards.md#Antlike+Wildcards) (for example, `dir/**/*.zip`).   
You can also specify a target directory to be created using the `file => directory` pattern (for example, `*.zip => winFiles,unix/distro.tgz => linuxFiles` will create the `winFiles` and `linuxFiles` directories, and respectively put the declared files inside them).

</td></tr></table>