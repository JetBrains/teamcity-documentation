[//]: # (title: TeamCity Specific Directories)
[//]: # (auxiliary-id: TeamCity Specific Directories)

<table>
<tr>

<td width="250">

Directory

</td>


<td>

Description

</td>
</tr>
<tr>

<td>

&lt;[TeamCity Home Directory](teamcity-home-directory.md)&gt;

</td>


<td>

This is TeamCity installation directory chosen in Windows installer, used to unpack the TeamCity `.zip` distribution.

</td>
</tr>
<tr>

<td>

[TeamCity Data Directory](teamcity-data-directory.md)

</td>


<td>

This is the directory used by TeamCity to store configuration and system files.

</td>
</tr>
<tr>

<td>

[Agent Work Directory](agent-work-directory.md)

</td>


<td>

This is the directory on an agent used as default location for build checkout directories.

</td>
</tr>
<tr>


<td>

[Agent Home Directory](agent-home-directory.md) 

</td>


<td>

Build agent installation directory. 

</td>
</tr>
<tr>

<td>

[Build Checkout Directory](build-checkout-directory.md) 

</td>

<td>

The directory used as "root" one for checking out build sources files.

</td>
</tr>
<tr>

<td>

[Build Working Directory](build-working-directory.md) 

</td>

<td>

This is the directory set as current for the build process. By default, the &lt;Build Working Directory&gt; is the same as the &lt;build checkout directory&gt;.

</td>
</tr>
<tr>

<td>

&lt;TeamCity web application&gt;

</td>


<td>

If you have installed TeamCity using the `.exe` or `tar.gz` distribution, the path to the directory is [`<TeamCity Home>`](teamcity-home-directory.md)`/webapps/ROOT`, by default.

</td>
</tr>
</table>
