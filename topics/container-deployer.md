[//]: # (title: Container Deployer)
[//]: # (auxiliary-id: Container Deployer)

The _Container Deployer_ build runner allows deploying WAR application archives to different containers. The runner supports the following Tomcat versions: 5.x, 6.x, 7.x and 8.x.

The settings common for all runners are described in [Configuring Build Steps](configuring-build-steps.md); this page details the Container Deployer settings.

The fields below support [parameter references](predefined-build-parameters.md): any text between percentage signs (`%`) is considered a reference to a property by TeamCity. To prevent TeamCity from treating the text in the percentage signs as a property reference, use two percentage signs to escape them: for example, if you want to pass `%\Y%m%d%H%M%S` into the build, change it to `%\%Y%%m%%d%%H%%M%%S`.

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

Enter target container info in the following format: `{hostname|IP}[:port]`

</td></tr><tr>

<td>

Container Type

</td>

<td>

Select the version of Tomcat from the dropdown. The default "Manager" web app must be deployed to the target Tomcat. The user must have role `manager-script`.

</td></tr><tr>

<td>

__Deployment Credentials__

</td>

<td>

</td>

</tr><tr>

<td>

Username

</td>

<td>

Specify the username. The user must have `manager-script` role assigned.

</td></tr><tr>

<td>

Password

</td>

<td>

Specify the password. Configuration parameters can be used

</td></tr><tr>

<td>

__Deployment Source__

</td>

<td>

</td>

</tr><tr>

<td>

Path to war archive

</td>

<td>

Specify the source war archive to be deployed.

</td></tr></table>
