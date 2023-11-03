[//]: # (title: HashiCorp Vault Intergration)

[HashiCorp Vault](https://www.vaultproject.io) is a secure storage for your tokens, passwords, certificates and encryption keys. Instead of storing sensitive information inside TeamCity parameters and tokens, you can store it in Vault and set up TeamCity so that it can securely access this data from Vault.

## Common Information

To set up an integration with HashiCorp Vault, you need to set up the [corresponding connection](configuring-connections.md) in the required project (or &lt;Root project&gt;, if you want any TeamCity project to use this connection).

To obtain and use a sensitive data stored in Vault, create a [parameter](configuring-build-parameters.md) whose value is a path to the Vault secret. Then you can reference this parameter in your build steps (for example, in a [](command-line.md) runner that executes the `terraform apply -var password=%\myVaultParam%` line in its script).

When a build that utilizes this parameter starts, a TeamCity server uses the Vault connection set up earlier to request a one-time [response wrapping token](https://developer.hashicorp.com/vault/docs/concepts/response-wrapping), which it then passes to a TeamCity agent that runs this build. The build agent uses this token to request Vault secrets, and never shares obtained credentials back to the TeamCity server. When the build finishes, the agent's token is revoked.

## Set Up a Vault Connection

1. Navigate to **Administration | &lt;Your Project&gt; | Connections** and click **Add Connection**.
2. Choose **HashiCorp Vault** as the connection type.
3. Specify the common connection settings: the connection name, Vault and parameter namespaces, and Vault URL.

   * [Vault namespaces](https://developer.hashicorp.com/vault/docs/enterprise/namespaces) allow you to create "vaults within a vault" — isolated tenants inside a single Vault Enterprise instance. If Vault secrets you need to access are stored in this isolated environment, specify its namespace in the **Vault namespace** connection field (for example, `TeamABC/secrets/`). Otherwise, leave this setting empty.
   * **Parameter namespace** — a custom string that identifies this Vault connection. You can specify this field if you set up multiple Vault connections and intend to create [remote parameters](#Remote+Parameters). Otherwise, leave this field blank.
   * **Vault URL** is the address of your Vault instance. Local Vault installations (the default URL is `http://localhost:8200`) are also supported.

4. Choose the desired authentication method. TeamCity can authenticate to HCP Vault using a Vault's AppRole, via an AWS IAM role, or using a directory access protocol (LDAP).

    <tabs>
    
    <tab title="AWS IAM Auth">

    <table><tr><td>
    
    If both your build agents and Vault are hosted on AWS EC2 instances, you can let build agents to automatically communicate with Vault. Agents send signed [GetCallerIdentity](https://docs.aws.amazon.com/STS/latest/APIReference/API_GetCallerIdentity.html) queries to the Vault server, which forwards them to the AWS STS service and authenticates the agent based on the service response. See this Vault documentation article to learn more: [IAM auth method](https://developer.hashicorp.com/vault/docs/auth/aws#iam-auth-method).<br/><br/>
    
    > The AWS IAM auth method requires only build agents and Vault server being hosted on EC2 instances. The TeamCity server can reside in any custom location. However, if the TeamCity server is deployed outside AWS, the connection's **Test connection** button will report issues that agents will not experience during building.
    > 
    {type="note"}

    </td></tr></table>

    </tab>
    
    <tab title="Vault AppRole">

    <table><tr><td>
    
    This authentication method requires that you enable the AppRole auth method and create an **AppRole**. AppRoles are sets of Vault policies that specify which secrets a user (in this case, TeamCity) can access.<br/><br/>
    
    The Vault AppRole auth method requires you to set up the following settings:<br/><br/>
    
    * **AppRole auth endpoint path** — the path in your Vault instance where the AppRole auth endpoint is mounted. The default path is `approle` (without the preceding backslash character).
          
    * **AppRole Role ID** and **AppRole Secret ID** — these values can be obtained in CLI using the following commands:<br/><br/>
          
        ```Plain Text
        vault read -field=role_id auth/<AE>/role/<RN>/role-id
        vault write -f -field=secret_id auth/<AE>/role/<RN>/secret-id  
        ```

        <br/>where `<AE>` is the auth endpoint path you specified above (for example, `approle`) and `<RN>` is the name of your role (for example, `my-teamcity-role`).<br/><br/>

    Refer to these Vault documentation articles to learn how to create and set up AppRoles: [Getting Started: Policies](https://developer.hashicorp.com/vault/tutorials/getting-started/getting-started-policies), [AppRole auth method](https://developer.hashicorp.com/vault/docs/auth/approle).

    </td></tr></table>

    </tab>
    
    <tab title="LDAP Auth">

   <table><tr><td>

    This method can be used for Vault instances with enabled [LDAP auth method](https://developer.hashicorp.com/vault/docs/auth/ldap). This authentication option requires username and password. The **Path** setting stores the path to the LDAP authentication endpoint (for example, `ldap` for the default `auth/ldap/users/userA` and `auth/ldap/groups/groupB` endpoints).<br/><br/>

    Refer to this article for more information about setting up this authentication method in Vault: [LDAP auth method](https://developer.hashicorp.com/vault/docs/auth/ldap). 
    
    </td></tr></table>

    </tab>
   
    </tabs>

5. Tick **Fail in case of error** if you want builds to fail with the "Error while fetching data from HashiCorp Vault" message if the agent is unable to obtain Vault secrets and write them to [TeamCity parameters](#How+to+Obtain+Vault+Secrets+in+TeamCity). Note that builds will fail even if these parameters are never used. If this setting is disabled, builds will continue running with empty strings as secret parameter values.

**Kotlin DSL:**

```Kotlin
project {

    features {
        hashiCorpVaultParameter {
            id = "PROJECT_EXT_34"
            name = "HashiCorp Vault"
            url = "http://127.0.0.1:8200/"
            authMethod = appRole {
                roleId = "..."
                secretId = "..."
            }
            failOnError = false
        }
    }
}
```


## How to Obtain Vault Secrets in TeamCity

To start using a secret value from HCP Vault in your builds, create a [parameter](configuring-build-parameters.md) that serve two goals:

* Store a path to the Vault secret whose value needs to be obtained
* Store the secret value after it was obtained by a build agent

In addition to legacy [regular](#Regular+Parameters) parameters, TeamCity 2023.11 and newer supports [remote parameters](#Remote+Parameters).

### Regular Parameters

1. Go to **Administration | &lt;Project or Build Configuration&gt; | Parameters** and click **Add new parameter**.
    
    > Note that you can set up parameters only for projects that own a valid [Vault connection](#Set+Up+a+Vault+Connection) (or inherit it from a parent project).
    > 
    {type="note"}

2. Choose the **Environment variable** type and enter the parameter name. For example, `env.AWS_ACCESS_KEY_ID`.

3. In the **Value** field, enter the `%\vault:PATH!/KEY%` string. For example, `%\vault:secret/data/teamcity!/access_key%`.

4. Click **Save** to close the dialog.

**Kotlin DSL:**

```Kotlin
project {
    params {
        param("env.AWS_ACCESS_KEY_ID", "%\vault:secret/data/teamcity!/access_key%")
    }
}
```

### Remote Parameters

Compared to regular parameters, remote parameters showcase the following advantages:

* Vault paths are stored in the [parameter spec](typed-parameters.md#Adding+Parameter+Specification), which leaves the **Value** field free for the default/initial parameter value.
* Secret paths use more straightforward format without the `vault:` prefix.
* You can specify which [Vault connection](#Set+Up+a+Vault+Connection) should be used to retrieve this secret.

To create this parameter, do the following:

1. Go to **Administration | &lt;Project or Build Configuration&gt; | Parameters** and click **Add new parameter**.

   > Note that you can set up parameters only for projects that own a valid [Vault connection](#Set+Up+a+Vault+Connection) (or inherit it from a parent project).
   >
   {type="note"}

2. Choose the **Environment variable** type and enter the parameter name. For example, `env.AWS_ACCESS_KEY_ID`.

3. Click **Edit...** next to the "Spec" label and set up the following values:
    
    * **Type**: Remote
    * **Remote Connection Type**: HashiCorp Vault Parameter
    * **Parameter Namespace**: choose the same value as a [Vault connection](#Set+Up+a+Vault+Connection) you want to retrieve this secret has in its own **Parameter Namespace** setting.
    
        <img src="dk-param-namespace.png" width="706" alt="Parameter Namespace"/>
    
        Select **Default Namespace (empty)** to choose a connection with an empty **Parameter Namespace** field.
    
    * **Vault Query**: the path to the secret (for example, `secret/data/teamcity!/access_key`).

4. Click **Save** to close the dialog.

**Kotlin DSL:**

```Kotlin
project {
    params {
        hashiCorpVaultParameter {
            name = "env.AWS_ACCESS_KEY_ID"
            query = "secret/data/teamcity!/access_key"
            namespace = "DataLore"
        }
    }
}
```