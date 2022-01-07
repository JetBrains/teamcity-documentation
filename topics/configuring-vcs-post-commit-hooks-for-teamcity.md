[//]: # (title: Configuring VCS Post-Commit Hooks for TeamCity)
[//]: # (auxiliary-id: Configuring VCS Post-Commit Hooks for TeamCity)

By default, TeamCity uses a polling approach to detect changes in a VCS repository, that is for each [VCS root](vcs-root.md). It periodically sends requests to the version control repository server to find out whether there are new revisions. For large installations with hundreds of VCS roots, this may create a noticeable load on the VCS server and on TeamCity.

To avoid background polling, it is possible to set up a post-commit hook on the VCS server, which will notify TeamCity to start checking for changes procedure. This way, TeamCity will make background requests for changes detection only when such changes are available.

>If you are using GitHub, try the external [TeamCity Commit Hooks plugin](https://github.com/JetBrains/teamcity-commit-hooks).

## Overview

Even with commit hooks configured and working properly, TeamCity still makes requests for changes on the server start and on each build queuing (or starting) to ensure the latest changes are used even if commit hooks stopped to function.

When a commit hook call comes in, TeamCity starts checking for changes in VCS roots which match the request.   
If a change is found during the check, TeamCity automatically increases the [VCS polling interval](configuring-vcs-roots.md#Common+VCS+Root+Properties) (the minimum after the increase is 15 minutes, maximum is 4 hours, increased by 2 times on each successful check). If the commit hook stops working (for example, TeamCity finds a change in a VCS root which it did not receive a commit hook call for), the [VCS polling interval](configuring-vcs-roots.md#Common+VCS+Root+Properties) value is reset to default.

Commit hooks are received via TeamCity REST API [requests](https://www.jetbrains.com/help/teamcity/rest/manage-vcs-roots.html) which should typically be configured in the post-commit repository triggers:

`POST .../app/rest/vcs-root-instances/commitHookNotification?locator=<vcsRootInstancesLocator>`

The request returns textual details as to the performed operation or an error message.

It is important to find `<vcsRootInstancesLocator>` for the request to match only the affected VCS roots from those configured in the TeamCity instance. If too many VCS roots are matched by the request configured in the commit hook, it will lead to more requests and more overload on the VCS repository and TeamCity than using the default polling approach. Some examples of the "locator" are provided below.   
The request should be performed by a user who has the "_View project and all parent projects_" permission for all the projects where the VCS root is defined.   
Note that by default only the first 100 VCS root instances will be matched by the [request](https://www.jetbrains.com/help/teamcity/rest/manage-vcs-roots.html). To match more, `count:99999` can be added as below.

The most common form of `<vcsRootInstancesLocator>` is:

```Shell
vcsRoot:(type:<TYPE>,count:99999),property:(name:<URL_PROPERTY_NAME>,value:<VCS_REPOSITORY_URL_PART>,matchType:contains,ignoreCase:true),count:99999

```

However, for VCS roots without the parameter `%-references`, a more performant option can be used:

```Shell
vcsRoot:(type:<TYPE>,property:(name:<URL_PROPERTY_NAME>,value:<VCS_REPOSITORY_URL_PART>,matchType:contains,ignoreCase:true),count:99999),count:99999

```

Commit hooks examples for UNIX-based VCS servers are described below.

## Post-commit generic script

Save the script below on a VCS server as `teamcity-trigger.sh` (you will need a personal [access token](configuring-your-user-profile.md#Managing+Access+Tokens)): 


```Shell
SERVER=https://buildserver-url
ACCESS_TOKEN="<access-token>"
 
LOCATOR=$1
 
# The following is one-line:
(sleep 10;  curl --header "Authorization: Bearer $ACCESS_TOKEN" -X POST "$SERVER/app/rest/vcs-root-instances/commitHookNotification?locator=$LOCATOR" -o /dev/null) >/dev/null 2>&1 <&1 &
 
exit 0

```

For Perforce, you can use this [dedicated script](#Using+post-commit+script+for+Perforce).

Set the  variables according to your TeamCity server. The user must have the "_View build configuration settings_" permission for projects where the VCS root is defined. This permission is included in the Project Developer role by default.

<note>

If your TeamCity server uses a custom SSL certificate, you need to pass the `-k` or `--cacert /path/to/correct/internal/CACertificate` parameter to the `curl` command above.
</note>

## Setting up post-receive hook on Git server

1\. Locate the Git repository root on the target VCS server. It should contain the `.git/hooks` directory with some templates.

2\. Create the `.git/hooks/post-receive` file with a line:

```Shell
/path/to/teamcity-trigger.sh 'vcsRoot:(type:jetbrains.git,count:99999),property:(name:url,value:<VCS root repository URL>,matchType:contains,ignoreCase:true),count:99999'

```

where `<VCS root repository URL>` must be replaced with the repository URL specified in the corresponding TeamCity VCS root and the value should be URL-escaped. Note that locator has `matchType:contains` in it, which means you can specify some part of URL `too.file`.

3\. Make sure that both `teamcity-trigger.sh` and `hooks/post-receive` scripts can be read and executed by Git user(s). You may need to execute the following command:
    
```Shell
chmod 755 /path/to/teamcity-trigger.sh /path/to/git_root/.git/hooks/post-receive

```

## Setting up hook on Mercurial server

1\. Locate the Mercurial repository root on the target VCS server.

2\. Create or edit the `.hg/hgrc config` and add the following snippet:

```Shell
[hooks]
changegroup = /path/to/teamcity-trigger.sh 'vcsRoot:(type:mercurial,count:99999),property:(name:repositoryPath,value:<VCS root repository url>,matchType:contains,ignoreCase:true),count:99999'

```

where `<VCS root repository URL>` must be replaced with the repository URL specified in the corresponding TeamCity VCS root and the value should be URL-escaped. Note that the locator has `matchType:contains` in it, which means you can specify some part of the URL too.

3\. Make sure that `teamcity-trigger.sh` is executable. You may need to execute the following command:

```Shell
chmod 755 /path/to/teamcity-trigger.sh

```

## Setting up post-commit hook on Subversion server

1\. Locate the Subversion repository root containing the `db`, `hooks`, `locks`, and other directories. We need the `hooks` directory.

2\. Create the `hooks/post-commit` file with a line:

```Shell
/path/to/teamcity-trigger.sh 'vcsRoot:(type:svn,count:99999),property:(name:url,value:<VCS root repository url>,matchType:contains,ignoreCase:true),count:99999'
```

where `<VCS root repository URL>` must be replaced with the repository URL specified in the corresponding TeamCity VCS root and the value should be URL-escaped. Note that the locator has `matchType:contains` in it, which means you can specify some part of URL too.

 
3\. Make sure that both `teamcity-trigger.sh` and `hooks/post-commit` script can be read and executed by the process of the Subversion server. You may need to execute the following command:


```Shell
chmod 755 /path/to/teamcity-trigger.sh /path/to/svn_repository_root/hooks/post-commit

```

## Setting up post-commit hook on Perforce server

There are two ways to set up a post-commit hook in Perforce:

* [Using the dedicated script](#Using+post-commit+script+for+Perforce). This is a recommended approach.
* [Using the generic script](#Editing+Perforce+specification+with+generic+script). Obsolete approach.

### Using post-commit script for Perforce

You can install the dedicated post-commit script on your Perforce server. This script will autodetect Perforce VCS roots in TeamCity and trigger the respective builds.

To be able to use the script, you need to generate an [access token](configuring-your-user-profile.md#Managing+Access+Tokens) first. The TeamCity user assigned to this token must have the "_Run build_" permission for projects where Perforce VCS roots are defined. This permission is included in the Project Developer role by default.

It is also recommended configuring a _[Perforce Administrator Access](perforce-workspace-handling-in-teamcity.md#perforce-admin-access)_ connection in the project settings. TeamCity will use it to ensure that all changed files in the Perforce changelist are collected. If such a connection is not configured explicitly, TeamCity will try to connect to Perforce using settings of one of the project's VCS roots. 

1. Save this script on your Perforce server as `teamcity-hook.sh`:

    ```Shell
    #!/bin/sh
    # How to use this script:
    # 1. Save the script on the Perforce server under the name /path/teamcity-hook.sh.
    # 2. Run chmod +x /path/teamcity-hook.sh.
    # 3. Set up a change-commit trigger by adding the following line when editing the specification with the `p4 triggers` command:
    #    check-for-changes-teamcity change-commit //depot/... "/path/teamcity-hook.sh %\change%"
    # 4. Update the variables below.
    #
    # Update the following variables: 
    
    # TeamCity server root URL:
    TEAMCITY_SERVER=http://localhost:8111/bs
    
    # Access token of a user on TeamCity server which can run builds in all relevant configurations (Developer role)
    TC_ACCESS_TOKEN="<token_value>"
    
    # This P4PORT value will be used to find relevant VCS roots, it should be equal to the P4Port setting in the VCS root:
    P4PORT="localhost:1666"
    CHANGE=$1
    
    # The following is one line:
    (sleep 10;  curl -H "Authorization: Bearer $TC_ACCESS_TOKEN" -d "p4port=$P4PORT&changelistId=$CHANGE" "$TEAMCITY_SERVER/app/perforce/commitHook" -o /dev/null) >/dev/null 2>&1 <&1 &
    exit 0
    
    ```

2. Run `chmod +x /path/teamcity-hook.sh`.
3. [Edit the Perforce specification](https://www.perforce.com/perforce/r15.1/manuals/p4sag/chapter.scripting.html#scripting.trigger.table.fields) with the `p4 triggers` command and set up a change-commit trigger by adding the following line:
   ```Shell
   check-for-changes-teamcity change-commit //depot/... "/path/teamcity-hook.sh %\change%"
   ```
   where `//depot/...` is the depot which is used in TeamCity builds. If there are multiple depots, you can replace this path with `//...`.
4. Update the variables from the script with your TeamCity settings.

>If your TeamCity server uses a custom SSL certificate, you need to pass the `-k` or `--cacert /path/to/correct/internal/CACertificate` parameter to the `curl` command above.
>
{type="note"}

### Editing Perforce specification with generic script

Set up a `change-commit` trigger by adding one or several lines when [editing the specification](https://www.perforce.com/perforce/r15.1/manuals/p4sag/chapter.scripting.html#scripting.trigger.table.fields). The text below must be placed in one line, one line per trigger.


```Shell
check-for-changes-teamcity change-commit //depot/project1/... "/path/teamcity-trigger.sh '<VCS Root locator>'"

```


where `<VCS Root locator>` can be one of the following:

* for Stream-based VCS roots:
    ```Shell
    vcsRoot:(type:perforce,count:99999),property:(name:stream,value://streamdepot/streamname,matchType:contains,ignoreCase:true),count:99999
    
    ```

* for Client-based VCS roots:
    ```Shell
    vcsRoot:(type:perforce,count:99999),property:(name:client,value:<client name>,matchType:contains,ignoreCase:true),count:99999
    
    ```

* for Client-mapping VCS roots:
    ```Shell
    vcsRoot:(type:perforce,count:99999),property:(name:client-mapping,value:<unique client mapping>,matchType:contains,ignoreCase:true),count:99999
    
    ```
    where `<unique client mapping>` should match the Perforce depot path in the TeamCity VCS root after all parameters' resolution. For the rule `check-for-changes-teamcity change-commit //depot/project1/...` it would be `//depot/project1/`.  
  Each `check-for-changes-teamcity` rule line describes an association between the path with the commit (`//depot/project1`) and a set of VCS roots which should be checked for changes.

## Setting up service hook on Team Foundation Server for TFVC and Git

The latest Azure DevOps Server (formerly, Team Foundation Server) and Azure DevOps Services provide service hooks for code commit events. To create a hook, perform the following steps:

1\. Open the admin page for a team project in web access.

2\. Create a subscription by running the wizard.

3\. Select the "Web Hooks" service to integrate with.

4\. Select the "Code checked in" event and specify a filter.

5\. Fill in the TeamCity username, password, and server URL in the format:

```Shell
"$SERVER/app/rest/vcs-root-instances/commitHookNotification?locator=$LOCATOR"

```

where the `$LOCATOR` value depends on the TFS repository type as described in the sections below.

### TFVC Repository


```Shell

vcsRoot:(type:tfs,count:99999),property:(name:tfs-url,value:<TFS server url>,matchType:contains,ignoreCase:true),property:(name:tfs-root,value:<TFS project>,matchType:contains,ignoreCase:true),count:99999

```

where `<TFS server url>` must be replaced with the value specified in the TFS VCS root URL and path properties. Example:


```Shell

http://teamcity/app/rest/vcs-root-instances/commitHookNotification?locator=vcsRoot:(type:tfs,count:99999),property:(name:tfs-url,value:http%3A%2F%2Ftfs%3Aport%2Ftfs%2Fcollection,matchType:contains,ignoreCase:true),property:(name:tfs-root,value:Project,matchType:contains,ignoreCase:true),count:99999

```

### Git Repository


```Shell
vcsRoot:(type:jetbrains.git,count:99999),property:(name:url,value:<VCS root repository URL>,matchType:contains,ignoreCase:true),count:99999

```


where `<VCS root repository URL>` must be replaced with the repository URL specified in the corresponding TeamCity VCS root and the value should be URL-escaped. Example:


```Shell
http://teamcity/app/rest/vcs-root-instances/commitHookNotification?locator=vcsRoot:(type:jetbrains.git,count:99999),property:(name:url,value:https%3A%2F%2Faccount.visualstudio.com%2FDefaultCollection%2FProject%2F_git%2FRepository,matchType:contains,ignoreCase:true),count:99999

```

6\. Click __Finish__ to create the service hook.

## Troubleshooting

It is recommended to try executing the following command from the command line before configuring the actual hook:


```Shell
curl --header "Authorization: Bearer $ACCESS_TOKEN" -X POST "$SERVER/app/rest/vcs-root-instances/commitHookNotification?locator=$LOCATOR"

```

If the commit hook matches the VCS root on the server correctly, you should see the output similar to this:


```Shell
Scheduled checking for changes for 1 VCS roots. (Server time: 20160719T192540.787+0300)

```

If the commit hook has not found any VCS roots, it will report an error:


```Shell
No VCS roots are found ...

```

Possible reasons for this output:
* the specified locator is incorrect, it does not match any VCS root on the server
* the specified user does not have enough permission for at least one of the matched VCS roots.

To check what roots are actually matched, use the request (see also [details](https://www.jetbrains.com/help/teamcity/rest/manage-vcs-roots.html)):

```Shell
curl --header "Authorization: Bearer $ACCESS_TOKEN" -X POST "$SERVER/app/rest/vcs-root-instances?locator=$LOCATOR"

```

<note>

A commit hook supports matching more than one VCS root, but it is highly recommended limiting the matched VCS roots only to those affected by the change generating the event.

It is recommended to set up a commit hook per a VCS repository. In this case on a check-in to some repository, TeamCity will not spend resources trying to find commits in other non-related VCS roots which were also matched by the commit hook.

By default, the number of VCS roots matched by a commit hook in TeamCity is limited by 100. If you want to match more than 100 VCS roots, add the count parameter: `<locator>,count:1000`. Check the corresponding [request description](https://www.jetbrains.com/help/teamcity/rest/manage-vcs-roots.html).
</note>
