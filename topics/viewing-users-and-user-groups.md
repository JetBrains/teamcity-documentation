[//]: # (title: Viewing Users and User Groups)
[//]: # (auxiliary-id: Viewing Users and User Groups)
You can view the list of users and user groups registered in the system on the __Administration | Users__ and __Groups__ pages. The content of this page depends on the [Configuring Authentication Settings](configuring-authentication-settings.md) of server configuration and TeamCity edition you have. For example, user accounts search and assigning roles are not available in TeamCity Professional.

#### Searching Users

On the top of the Users and Groups page there's search panel, which allows you to easily find users in question:
	
* In the _Find_ field you can specify a search string, which can be a user visible name, full name, or email address, or a part of it.
* To narrow down the search you can also restrict it to particular user group, role, or role in specific project using corresponding drop\-down lists. By selecting the _Invert roles filter_ option, you can invert seach results to show the list of users that do not have the specified role assigned.


<seealso>
        <category ref="concepts">
            <a href="user-account.md">User Account</a>
            <a href="user-group.md">User Group</a>
            <a href="role-and-permission.md">Role and Permission</a>
        </category>
</seealso>