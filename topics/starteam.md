[//]: # (title: StarTeam)
[//]: # (auxiliary-id: StarTeam)
This page describes the fields and options available when setting up VCS roots using StarTeam.

Common VCS Root properties are described [here](configuring-vcs-roots.md#Common+VCS+Root+Properties).

## StarTeam Connection Settings



<table>
<tr>


<td>

Option 


</td>


<td>

Description 


</td>
</tr>
<tr>


<td>

 URL 


</td>


<td>

 Specify the StarTeam URL that points to the required sources 


</td>
</tr>
<tr>


<td>

 Username 


</td>


<td>

 Enter the login for the StarTeam Server 


</td>
</tr>
<tr>


<td>

 Password 


</td>


<td>

 Enter the corresponding password for the user specified in the field above 


</td>
</tr>
<tr>


<td>

 EOL Conversion 


</td>


<td>

 Define the EOL (End Of Line) symbol conversion rules for text files. Select one of the following:



	
* __As stored in repository__ – EOL symbols are preserved as they are stored in the repository. No conversion is done.
	
* __Agent's platform default__ – whatever EOL symbol is used in a particular file in the repository, it will be converted to the platform\-specific line separator when passed to the build agent. The resulting EOL symbol exclusively depends on the agent's platform.




</td>
</tr>
<tr>


<td>

 Directory naming 


</td>


<td>

 Define the mode for naming the local directories. Select one of the following:



	
* __Using working folders__ – StarTeam folders have the "working folder" property. It defines which local path corresponds to the StarTeam folder (by default, the working folder equals the folder's name). In this mode TeamCity gives the local directories the names stored in the "working folder" properties. Note that even though StarTeam allows using absolute paths as working folders, TeamCity supports relative paths only and doesn't detect the presence of absolute paths.
	
* __Using StarTeam folder names__ – In this mode the local directories are named after corresponding StarTeam folders' names. This mode can suit users who keep the working directory structure the same as the project structure in the repository and don't want to rely on "working folder" properties because they can be uncontrollably modified.




</td>
</tr>
<tr>


<td>

 Checkout mode 


</td>


<td>

 Files can be retrieved by their current (tip) revision, by label, or by promotion state. 
Note that if you select checkout by label or promotion state, change detection won't be possible for this VCS root. As a result, your builds cannot be triggered if the label is moved or the promotion state is switched to another label. The only way of keeping the build up\-to\-date is using the schedule\-based triggering. To make it possible, a full checkout is always performed when you select checkout by label or promotion state options. 


</td>
</tr>
</table>




### Notes on Directory Naming Convention



When checking out sources TeamCity (just as StarTeam native client) forms local directory structure using _working folder_ names instead of just a folder name. By default, the working folder name for a particular StarTeam folder equals the folder's name.
For example, your project has a folder named "A" with a subfolder named "B". By default, their working folders are "A" and "B" correspondingly, and the local directory structure will look like `<checkout dir>/A/B`. But if the working folder for folder "A" is set to something different (for example, `Foo`), the directory structure will also be different: `<checkout dir>/Foo/B`.

StarTeam allows specifying absolute paths as working folders. However, TeamCity supports only relative working folders. This is done by design; all files retrieved from the source control must reside under the checkout directory. The build will fail if TeamCity detects the presence of absolute working folders.

You need to ensure that all the folders under the VCS root have relative working folder names.

__ __