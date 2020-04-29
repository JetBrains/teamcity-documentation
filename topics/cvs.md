[//]: # (title: CVS)
[//]: # (auxiliary-id: CVS)

Common VCS Root properties are described [here](configuring-vcs-roots.md#Common+VCS+Root+Properties).

This page contains descriptions of CVS\-specific fields and options available when setting up a VCS root. Depending on the selected access method, the page shows different fields that help you to easily define the [CVS settings](http://ximbiot.com/cvs/):

## CVS Root

<table><tr>

<td width="250">

Option


</td>

<td>

Description


</td></tr><tr>

<td>

Module name


</td>

<td>

Specify the name of the module managed by CVS.


</td></tr><tr>

<td>

CVS Root


</td>

<td>

Use these fields to select the access method, point to the user name, CVS server host and repository.  For example: `:pserver:user@host.name.org:/repository/path`.   
For a [local](#Local+CVS+Settings) connection only the path to the CVS repository should be used.   
TeamCity supports the following connection methods (described below):

* [pserver](#PServer+Protocol+Settings)
* [ext](#Ext+Protocol+Settings)
* [ssh](#SSH+Protocol+Settings+%28internal+implementation%29)
* [local](#Local+CVS+Settings)


</td></tr></table>

## Checkout Options

<table><tr>

<td width="250">

Option


</td>

<td>

Description


</td></tr><tr>

<td>

Checkout HEAD revision

Checkout from branch

Checkout by Tag


</td>

<td>

Define the way CVS will fill in and update the working directory


</td></tr><tr>

<td>

Quiet period


</td>

<td>

Since there are no atomic commits in CVS, using this setting  you can instruct TeamCity not to take (detect) a change  until the specified period of time passed since the previous change was detected. It helps avoid situations when one commit is shown as two different changes in the TeamCity web UI.


</td></tr></table>

## PServer Protocol Settings

<table><tr>

<td width="250">

Option


</td>

<td>

Description


</td></tr><tr>

<td>

CVS Password


</td>

<td>

Click this radio button if you want to access the CVS repository by entering a password.


</td></tr><tr>

<td>

Password File Path


</td>

<td>

Click this radio button to specify the path to the `.cvspass` file.


</td></tr><tr>

<td>

Connection Timeout


</td>

<td>

Specify the connection timeout.


</td></tr></table>

## Ext Protocol Settings

<table><tr>

<td width="250">

Option


</td>

<td>

Description


</td></tr><tr>

<td>

Path to external rsh


</td>

<td>

Specify the path to the rsh program used to connect to the repository.


</td></tr><tr>

<td>

Path to private key file


</td>

<td>

Specify the path to the file that contains user authentication information.


</td></tr><tr>

<td>

Additional parameters


</td>

<td>

Enter any required rsh parameters that will be passed to the program. Multiple parameters are separated by spaces.


</td></tr></table>

## SSH Protocol Settings (internal implementation)

<table><tr>

<td width="250">

Option


</td>

<td>

Description


</td></tr><tr>

<td>

 SSH version


</td>

<td>

Select a supported version of SSH.


</td></tr><tr>

<td>

SSH port


</td>

<td>

Specify SSH port number.


</td></tr><tr>

<td>

SSH Password


</td>

<td>

Click this radio button if you want to authenticate via an SSH password.


</td></tr><tr>

<td>

Private Key


</td>

<td>

Click this radio button to use private key authentication. In this case specify the path to the private key file.


</td></tr><tr>

<td>

SSH Proxy Settings


</td>

<td>

See proxy options [below](#Local+CVS+Settings).


</td></tr></table>

## Local CVS Settings

<table><tr>

<td width="250">

Option


</td>

<td>

Description


</td></tr><tr>

<td>

Path to the CVS Client


</td>

<td>

Specify the path to the local CVS client.


</td></tr><tr>

<td>

Server Command


</td>

<td>

Type the server command. The server command is the command which makes the CVS client to work in server mode. Default value is `server`.


</td></tr></table>

## Proxy Settings

<table><tr>

<td width="250">

Option


</td>

<td>

Description


</td></tr><tr>

<td>

Use proxy


</td>

<td>

Check this option if you want to use a proxy, and select the desired protocol from the drop\-down list.


</td></tr><tr>

<td>

Proxy Host


</td>

<td>

Specify the name of the proxy.


</td></tr><tr>

<td>

Proxy Port


</td>

<td>

Specify the port number.


</td></tr><tr>

<td>

Login


</td>

<td>

Specify the login.


</td></tr><tr>

<td>

Password


</td>

<td>

Specify the password.


</td></tr></table>

## Advanced Options

<table><tr>

<td width="250">

Option


</td>

<td>

Description


</td></tr><tr>

<td>

Use GZIP compression


</td>

<td>

Check this option to apply gzip compression to the data passed between the CVS server and the CVS client.


</td></tr><tr>

<td>

Send all environment variables to the CVS server


</td>

<td>

Check this option to send environment variables to the server for compatibility with certain servers.


</td></tr><tr>

<td>

History Command Supported


</td>

<td>

Check this option to use the history file on the server to search for newly committed changes. Enable this option to improve the CVS support performance. By default, the option is not checked because not every CVS server supports keeping a history log file.


</td></tr></table>

__ __