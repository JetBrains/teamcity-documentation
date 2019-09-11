[//]: # (title: SMB Upload)
[//]: # (auxiliary-id: SMB Upload)

SMB upload enables TeamCity to upload files/directories to Windows shares via Server Message Block (SMB) protocol. 

The settings common for all runners are described in [Configuring Build Steps](configuring-build-steps.md); this page details the SMB Upload settings.

<tip>

The fields below support [parameter references](predefined-build-parameters.md): any text between percentage signs (%) is considered a reference to a property by TeamCity. To prevent TeamCity from treating the text in the percentage signs as a property reference, use two percentage signs to escape them: for example, if you want to pass `"%Y%m%d%H%M%S"` into the build, change it to `"%%Y%%m%%d%%H%%M%%S"`.
</tip>

<table><tr>

<td>

Option

</td>

<td>

Description

</td></tr><tr>

<td>

__Deployment Target__

</td></tr><tr>

<td>

__Target URL__

</td>

<td>

 The URL should point to a host \+ share at least. Subdirectories are allowed here and will be created if missing. Valid examples:


```Shell
\\host\share_name
\\host\share_name\some\path
\\host\c$\Temp
```




</td></tr><tr>

<td>

__Name resolution__

</td>

<td>

 The __DNS only name resolution__ allows switching  JCIFS to "DNS\-only" mode. May fix perfomance or out of memory exceptions (see [this bitbucket issue](https://bitbucket.org/nskvortsov/deployer/issue/20/out-of-memory-exception) for details). Is equivalent to following JCIFS settings:


```Shell
-Djcifs.resolveOrder=DNS
-Djcifs.smb.client.dfs.disabled=true
```




</td></tr><tr>

<td>

__Deployment Credentials__

</td></tr><tr>

<td>

__Username__

</td>

<td>

Specify the username

</td></tr><tr>

<td>

__Password__

</td>

<td>

Specify the password

</td></tr><tr>

<td>

__Domain__

</td>

<td>

Specify the __domain__

</td></tr><tr>

<td>

__Deployment Source__

</td></tr><tr>

<td>

__Path to sources__

</td>

<td>

Specify the deployment sources as a newline\- or comma\-separated list of paths to files/directories for deployment. Ant\-style wildcards like `dir/**/*.zip` and target directories like `*.zip => winFiles`,`unix/distro.tgz => linuxFiles`, where `winFiles` and `linuxFiles` are target directories, are supported.  

</td></tr></table> 