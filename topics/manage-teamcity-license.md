[//]: # (title: Manage TeamCity Licenses)

Users with System Administrator role can manage TeamCity server and agent licenses on the **Administration | Licenses** page. We recommend activating your server license, even for the free Professional version, to ensure system administrators receive timely email notifications about critical TeamCity server security updates.

For users with paid licenses, activating a license grants the "set it and forget it" experience: once your instance is linked with the [JetBrains Account](https://account.jetbrains.com) (JBA), you no longer need to manually enter license keys for every additional agents batch you purchase or every server license upgrade. TeamCity will automatically retrieve information about all currently active licenses.

## Activate a License

Activating a TeamCity server license combines two separate actions: applying a license to your server (in case of a paid server license this removes limitations imposed by the free Professional version) and linking this instance with a specific JetBrains Account (this takes care of automatic license updates and security update notifications).

Linking your instance with the JetBrains Account dictates the following requirements:

* a machine that runs your TeamCity server should be connected the internet and able to access `account.jetbrains.com`;
* a person who activates a license should have administrator permissions in the target JetBrains Account.

If you are both TeamCity and JetBrains Account administrator and TeamCity server machine can access JBA:

1. Navigate to **Administration | Licenses** page.
2. Click **Get License Key** and log into your JetBrains Account.
3. Choose a server license you want to activate.
4. Click **Get license key** to confirm. The license will get activated automatically.

If you have [no access to the required JetBrains Account](#Offline+Mode):

1. Navigate to **Administration | Licenses** page.
2. Right-click the **Get License Key** and copy its URL. This URL includes a unique public key of your instance and looks like the following: `https://account.jetbrains.com/license/activate?product=TCS&instPubKey=<your public key>.`.
3. Open this link on a device that has access to the JetBrains Account and choose a TeamCity license that needs to be activated.
4. Click **Get license key** and copy or save the generated private key.
5. On the TeamCity **Administration | Licenses** page, click **I already have a key** and paste a private key.

If you only manage TeamCity and have no administrator permissions for the JetBrains account, steps 3 and 4 should be performed by a JBA administrator instead. You should send them a link from step 2, and they should send you back the activation key.


## Revoke a License

Activating a license is a two-way process: linking your TeamCity instance to the JBA and obtaining a valid license from it. Similarly, revoking a license also has two stages: in TeamCity and in JetBrains account.

### Revoke in TeamCity

On the **Administration | Licenses** page, click **Revoke** to revoke a license. By doing so you disable the target license, but it still remains active in your JetBrains account. This means if you need to re-activate this license on the same server, copy a key from the JBA page and insert it using the **I already have a key** button.

<img src="dk-get-licence-key-from-jba.png" width="706" alt="Get license key from JBA"/>

This license key will be accepted without the need to follow the **Get license key** link first and select which license you want to activate in JetBrains Account.

### Revoke in JetBrains Account

After you click **Revoke** in TeamCity, the JetBrains Account page opens. You can revoke a required license right away, or do it [later as a separate step](https://sales.jetbrains.com/hc/en-gb/articles/207739209-Revoking-licenses-from-users).

Revoking a license from the JetBrains Account severs the link between JBA and your instance. This means if you need to reactivate the same license on the same server, you need to repeat all steps described in the [](#Activate+a+License) section. If a TeamCity instance is not linked to the JBA, the **License key** link mentioned in the [previous](#Revoke+in+TeamCity) section produces a shorter key in the [legacy format](#Legacy+Activation) that will not be accepted in the **I already have a key** field.


## Organizations and Teams

[Teams](https://sales.jetbrains.com/hc/en-gb/articles/360012278699-What-are-teams-in-JetBrains-Account) are separate groups that you can set up in JetBrains Account to distribute different licenses to organization members depending on their tasks.

For TeamCity, additional agent licenses should belong to the same team that owns a server license. Otherwise, your paid agents will not be available after activating a server license. Moreover, any action that affects a team (for example, adding new agent licenses) also alters the corresponding **server key**. This is especially important to remember when working in [offline mode](#Offline+Mode).

If you need to [transfer a license to another team](https://sales.jetbrains.com/hc/en-gb/articles/208460205-Transfer-licenses-between-teams) within your organization, you only need to revoke a license in TeamCity. A following revoke on the JetBrains Account side is not needed.


## Offline Mode

The nature of the JBA-based license activation mechanism implies that TeamCity periodically connects to `account.jetbrains.com` to refresh the information about owned server and agent licenses. This allows your build server to automatically retrieve the current license information, including both server and agent licenses assigned to your [team](#Organizations+and+Teams).

When TeamCity server is behind a proxy server that does not permit communications with `account.jetbrains.com`, the majority of these benefits do not apply. However, we still recommend to activate your license to get timely email notifications about available server security updates. See the [](#Activate+a+License) section for the instructions.

All license-related changes made in a [team](#Organizations+and+Teams) affect the **server key** (even if the change does not relate to the server directly, for example adding or removing additional agents). Since TeamCity cannot automatically fetch these updates when in offline mode, you need to refresh your server key manually. To do so, copy a server license key from the JetBrains Account page (see [](#Revoke+in+TeamCity)) and paste it using the **I already have a key** link in TeamCity.


## Legacy Activation

Existing customers who purchased TeamCity licences previously can still employ the legacy workflow by clicking the **Enter new license key** button and inserting the license key. However, we recommend switching to the JBA-based workflow to benefit from crucial security email notifications and automatic license updates.

To activate your currently applied licenses using the JetBrains Account:

1. Navigate to **Administration | Licenses** page.
2. Click **Download hashed offline keys** under the "Active license keys" table.
3. Click **Get License Key** and log into your JetBrains Account.
4. You should be able to see the "Already have license keys on your server?" tile that suggests you to convert your existing keys. Click **Upload license keys** and upload the "teamcity-offline-keys.txt" file generated in step 2.
5. Paste the generated key on the **Administration | Licenses** page.


## Limitations and Requirements

A single license can only be used on a single running TeamCity server at any given time. Running secondary TeamCity nodes in addition to the main node does not require a separate license at this time.

If you create a copy of the server and run two servers at the same time, you should ensure each license key is used on a single server only. You can use the Evaluation (limited time) license to run a server for testing/non-production purposes. The licenses are not bound to a specific server instance, machine, and so on. The only limitation is that a license cannot be used on several servers at the same time.

When you already own license(s) and buy more licenses, you can [request](https://www.jetbrains.com/support/sales/) JetBrains sales to make the new licenses co\-termed with those already purchased, so that all the licenses have equal maintenance expiration date. The cost of the licenses is then lowered proportionally.   
When buying many licenses, you are welcome to [contact](https://www.jetbrains.com/support/sales/) our sales for available volume discounts.