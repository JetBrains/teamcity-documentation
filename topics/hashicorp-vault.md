[//]: # (title: HashiCorp Vault Intergration)

[HashiCorp Vault](https://www.vaultproject.io) is a secure storage for your tokens, passwords, certificates and encryption keys. Instead of storing sensitive information inside TeamCity parameters and tokens, you can store it in Vault and set up TeamCity so that it can securely access this data from Vault.

## Common Information

To set up an integration with HashiCorp Vault, you need to set up the [corresponding connection](configuring-connections.md) in the required project (or &lt;Root project&gt;, if you want any TeamCity project to use this connection).

To obtain and use a sensitive data stored in Vault, create a [parameter](configuring-build-parameters.md) whose value is a path to the Vault secret. Then you can reference this parameter in your build steps (for example, in a [](command-line.md) runner that executes the `terraform apply -var password=%\myVaultParam%` line in its script).

When a build that utilizes this parameter starts, a TeamCity server uses the Vault connection set up earlier to request a one-time [response wrapping token](https://developer.hashicorp.com/vault/docs/concepts/response-wrapping), which it then passes to a TeamCity agent that runs this build. The build agent uses this token to request Vault secrets, and never shares obtained credentials back to the TeamCity server. When the build finishes, the agent's token is revoked.

## Set Up a Vault Connection

1. Navigate to **Administration | &lt;Your Project&gt; | Connections** and click **Add Connection**.
2. Choose "HashiCorp Vault" as the connection type.
3. Specify the common connection settings: the connection name, Vault and parameter namespaces, and Vault URL.

   * [Vault namespaces](https://developer.hashicorp.com/vault/docs/enterprise/namespaces) allow you to create "vaults within a vault" — isolated tenants inside a single Vault Enterprise instance. If Vault secrets you need to access are stored in this isolated environment, specify its namespace in the **Vault namespace** connection field (for example, `TeamABC/secrets/`). Otherwise, leave this setting empty.
   * **Parameter namespace** — ???
   * **Vault URL** is the address of your Vault instance. Local Vault installations (the default URL is `http://localhost:8200`) are also supported.

4. Choose the desired authentication method. TeamCity can authenticate to HCP Vault using a Vault's AppRole, via an AWS IAM role, or using a directory access protocol (LDAP).

    <tabs>
    
    <tab title="AWS IAM Auth">
    
    </tab>
    
    <tab title="Vault AppRole">
    
    This authentication method requires that you enable the AppRole auth method and create an **AppRole**. AppRoles are sets of Vault policies that specify which secrets a user (in this case, TeamCity) can access. To learn how to create and set up AppRoles, refer to these Vault documentation articles: [Getting Started: Policies](https://developer.hashicorp.com/vault/tutorials/getting-started/getting-started-policies), [AppRole auth method](https://developer.hashicorp.com/vault/docs/auth/approle).
    
    The Vault AppRole auth method requires you to set up the following settings:
    
    * **AppRole auth endpoint path** — the path in your Vault instance where the AppRole auth endpoint is mounted. The default path is `approle` (without the preceding backslash character).
          
    * **AppRole Role ID** and **AppRole Secret ID** — these values can be obtained in CLI using the following commands:
          
        ```Plain Text
        vault read -field=role_id auth/<AUTH_ENDPOINT>/role/<ROLE_NAME>/role-id
        vault write -f -field=secret_id auth/<AUTH_ENDPOINT>/role/<ROLE_NAME>/secret-id  
        ```
              
        where `<AUTH_ENDPOINT>` is the path you specified above (for example, `approle`) and `<ROLE_NAME>` is the name of your role (for example, `my-teamcity-role`).
    
    </tab>
    
    <tab title="LDAP Auth">
    
    </tab>
    </tabs>

5. Tick **Fail in case of error** if you want builds to fail with the "Error while fetching data from HashiCorp Vault" message if the agent is unable to obtain Vault secrets. Otherwise, ??? (builds will continue with empty strings as secret parameter values?).