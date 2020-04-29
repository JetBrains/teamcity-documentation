[//]: # (title: Typical LDAP Configurations)
[//]: # (auxiliary-id: Typical LDAP Configurations)
This page contains samples of the `ldap-config.properties` file for different configuration cases.

## Basic LDAP Login

The examples of minimal working configurations are given below.

### Windows Active Directory

```Shell
java.naming.provider.url=ldap://dc.example.com:389/DC=example,DC=com
java.naming.security.principal=<username>
java.naming.security.credentials=<password>
teamcity.users.login.filter=(sAMAccountName=$capturedLogin$)
teamcity.users.username=sAMAccountName

```

Note that `sAMAccountName` is limited to 20 symbols. You might want to use another attribute which contains the entire username.

### Unix

```Shell
java.naming.provider.url=ldap://dc.example.com:389/DC=example,DC=com
java.naming.security.principal=<username>
java.naming.security.credentials=<password>
teamcity.users.login.filter=(uid=$capturedLogin$)
teamcity.users.username=uid

```

TeamCity does not store the user passwords in this case. On each user login, authentication is performed by a direct login into LDAP with the credentials entered in the login form.

### Specifying Backup LDAP server

You can specify a backup LDAP server in the `java.naming.provider.url` property as follows:

```Shell
# The second URL is used when the first server is down.
java.naming.provider.url=ldap://example.com:389/DC=example,DC=com ldap://failover.example.com:389/DC=example,DC=com

```

## Basic LDAP Login for Users in Specific LDAP Group Only

Only users from a specific user group are allowed to log in. The users need to enter the username only the without domain part to log in. The example is for Windows Active Directory:

```Shell
java.naming.provider.url=ldap://example.com:389/DC=example,DC=com
java.naming.security.principal=<username>
java.naming.security.credentials=<password>
 
# filtering only users with specified name and belonging to LDAP group "Group1" with DN "CN=Group1,CN=Users,DC=example,DC=com"
teamcity.users.login.filter=(&(sAMAccountName=$capturedLogin$)(memberOf=CN=Group1,CN=Users,DC=example,DC=com))
 
 
#teamcity.users.username=sAMAccountName
 
# Allow only username part without domain (optional)
teamcity.auth.loginFilter=[^/\\\\@]+
 
# No synchronization, just login.
teamcity.options.users.synchronize=false
teamcity.options.groups.synchronize=false

```

## Active Directory With User Details Synchronization

Users can log in to TeamCity with their domain name without the domain part, there is an account "teamcity" with the password "secret" that can read all Active Directory entries. The TeamCity user display name and email are synchronized from Active Directory.

```Shell
java.naming.provider.url=ldap://example.com:389/DC=example,DC=com
java.naming.security.principal=CN=teamcity,CN=Users,DC=example,DC=com
java.naming.security.credentials=secret
teamcity.users.login.filter=(sAMAccountName=$capturedLogin$)
teamcity.users.username=sAMAccountName
 
# User synchronization: on, synchronize display name and email.
teamcity.options.users.synchronize=true
teamcity.users.filter=(objectClass=user)
teamcity.users.property.displayName=displayName
teamcity.users.property.email=mail

```

## Active Directory With User Details Synchronization and User Creation

Users can log in to TeamCity with their domain name without the domain part, there is an account "teamcity" with the password "secret" that can read all Active Directory entries. The TeamCity user display name and email are synchronized from Active Directory. The users not existing in the TeamCity database are created. Users no longer existing in Active Directory are deleted from the TeamCity user database.

```Shell
java.naming.provider.url=ldap://example.com:389/DC=example,DC=com
java.naming.security.principal=CN=teamcity,CN=Users,DC=example,DC=com
java.naming.security.credentials=secret
teamcity.users.login.filter=(sAMAccountName=$capturedLogin$)
teamcity.users.username=sAMAccountName
 
# User synchronization: on, synchronize display name and email.
teamcity.options.users.synchronize=true
teamcity.users.filter=(objectClass=user)
teamcity.users.property.displayName=displayName
teamcity.users.property.email=mail
 
# Automatic user creation and deletion during user synchronization
teamcity.options.createUsers=true
teamcity.options.deleteUsers=true

```

## Active Directory With Group Synchronization

There should be `ldap-mapping.xml` file with one or more group mappings defined.

`ldap-config.properties` file:

```
java.naming.provider.url=ldap://example.com:389/DC=example,DC=com
java.naming.security.principal=CN=teamcity,CN=Users,DC=example,DC=com
java.naming.security.credentials=secret
teamcity.users.login.filter=(sAMAccountName=$capturedLogin$)
teamcity.users.username=sAMAccountName
 
# User synchronization is on, synchronize display name and email.
teamcity.options.users.synchronize=true
teamcity.users.filter=(objectClass=user)
teamcity.users.property.displayName=displayName
teamcity.users.property.email=mail
 
# Automatic user creation and deletion during users synchronization
teamcity.options.createUsers=true
teamcity.options.deleteUsers=true
 
# Groups synchronization is on
teamcity.options.groups.synchronize=true
 
# The group search LDAP filter used to retrieve groups to synchronize.
# The result includes all the groups configured in the ldap-mapping.xml file.
 
teamcity.groups.filter=(objectClass=group)
 
# The LDAP attribute of a group storing its members.
teamcity.groups.property.member=member

```

### Limiting the number of groups to be synchronized

The `teamcity.users.filter` property helps limit the number of processed user accounts during users synchronization.

It is recommended to create the "TeamCity Users" group in Active Directory, and include all your required groups into this group, for example, you may have the following Active Directory structure:
* Group A with members User 1, User 2
* Group B with members User 3, User 4
* Group "TeamCity Users" with members Group A, Group B

Then update the `teamcity.users.filter` property. For example,

```Shell
teamcity.users.filter=(&(objectClass=user)(memberOf:1.2.840.113556.1.4.1941:=CN=TeamCity Users,OU=Accounts,DC=domain,DC=com))

```

In this case TeamCity creates accounts only if they are members of the corresponding Active Directory group. Nested groups are supported.

Alternatively, you can list several groups:

```Shell
teamcity.users.filter=(&(objectClass=user)(|(memberOf=CN=GroupOne,OU=myou,DC=company,DC=tld)(memberOf=CN=GroupTwo,OU=myou,DC=company,DC=tld)))

```

To limit users who can login into TeamCity you also need to change the `teamcity.users.login.filter` property:

```Shell
teamcity.users.login.filter=(&(sAMAccountName=$capturedLogin$)(memberOf:1.2.840.113556.1.4.1941:=CN=TeamCity Users,OU=Accounts,DC=domain,DC=com))

```

For more details on the filter syntax refer to the [Microsoft documentation](https://msdn.microsoft.com/en-us/library/aa746475%28v=vs.85%29.aspx). For more details on the AD attributes refer to the [Microsoft documentation](https://msdn.microsoft.com/en-us/library/ms677980(v=vs.85).aspx).

__ __