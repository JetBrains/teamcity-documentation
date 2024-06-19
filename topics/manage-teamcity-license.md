[//]: # (title: Manage TeamCity Licenses)

Users with System Administrator role can manage TeamCity server and agent licenses on the **Administration | Licenses** page. We recommend activating your server license, even for the free Professional version, to ensure system administrators receive timely email notifications about critical TeamCity server security updates.

For users with paid licenses, activating a license grants the "set it and forget it" experience: once your instance is linked with the [JetBrains Account](https://account.jetbrains.com) (JBA), you no longer need to manually enter license keys for every additional agents batch you purchase or every server license update. TeamCity will automatically retrieve information about all currently active licenses, and apply them on-the-fly.

## Activate a License

To activate a paid or a free TeamCity license:

1. Navigate to **Administration | Licenses** page.
2. Click **Get License Key** and log into your JetBrains Account.
3. Choose a server license you want to activate. This page shows licenses owned by all teams associated with the current account.
   <img src="dk-choose-license.png" width="706" alt="Choose a license"/>
4. Click **Get license key** to confirm. The activation key will be transfered to your TeamCity server automatically. You can also manually copy a key and paste it to the **I already have a key** field on the TeamCity **Licenses** page.

This simple sequence may vary depending on whether you can access the <a href="https://account.jetbrains.com">JetBrains Account</a>, or whether your TeamCity server's machine is connected to the internet.


<dl>

<dt>Case 1: You are not a JetBrains Account administrator</dt>
<dd>

<b>Solution:</b> Follow the same procedure as above and ask your JetBrains Account administrator to perform actions you have no permissions to do.

<ol>

<li>On the <b>Administration | Licenses</b> page, right-click the <b>Get License Key</b> and copy the link.</li>

<li>Send this link to your JBA administrator. This link contains your unique TeamCity instance key and looks like the following: <code>https://account.jetbrains.com/license/activate?product=TCS&amp;instPubKey=&lt;your_public_key&gt;</code>.</li>

</ol>

Once the administrator follows this link and activates a license, your TeamCity server will automatically apply it. All future license updates, including build agent license assignments and expirations, will be automatically communicated to your server, you will not require further administrator assistance after the activation is complete.

</dd>

<dt>Case 2: Your TeamCity server machine is not connected to the internet</dt>

<dd>

<b>Solution:</b> Similarly to the previous case, you can utilize the activation link generated in TeamCity.

<ol>

<li>On the <b>Administration | Licenses</b> page, right-click the <b>Get License Key</b> and copy the link.</li>

<li>Open this link on a trusted device that has access to the JetBrains Account. This link contains your unique TeamCity instance key and looks like the following: <code>https://account.jetbrains.com/license/activate?product=TCS&amp;instPubKey=&lt;your_public_key&gt;</code>.</li>

<li>
After you activate a license in your JetBrains Account, copy or save the activation key.

<img src="dk-activation-successful.png" width="706" alt="Successful activation"/>
</li>

<li>

On the TeamCity <b>Licenses</b> page, click <b>I already have a key</b> and paste the activation key.

</li>
</ol>


Note that since an offline TeamCity server is unable to receive automatic updates from JBA, you will need to copy and paste activation keys whenever additional licenses are purchased.

See also: <a href="configuring-proxy-server.md">Configuring Proxy Server</a>

</dd>

</dl>


## Deactivate a License

You may want to deactivate a server license to [transfer it to another team](https://sales.jetbrains.com/hc/en-gb/articles/208460205-Transfer-licenses-between-teams) or switch to another server license you have.

Deactivating a license is an opposite of the [activation process](#Activate+a+License): you need to remove a license from TeamCity and (optionally) deactivate in on the JetBrains account.

1. To remove a license from TeamCity, click **Deactivate** on the **Administration | Licenses** page. In case you had a paid license, your server will revert back to the free Professional tier.

2. When you click **Deactivate**, TeamCity automatically brings you to the JetBrains Account page where you can proceed to deactivate this license.
   
   If you do not complete this step, the server license (although inactive in TeamCity) will remain linked to this specific server instance. This means you cannot activate it on another server. On the other hand, keeping a license linked to the TeamCity instance allows you to easily re-activate it should you plan to do this later. Instead of completing all four [activation steps](#Activate+a+License), go directly to JetBrains Account and copy an activation key.
   
   <img src="dk-get-license-key.png" width="706" alt="Get license key from JBA"/>
   
   Insert this key on the TeamCity **Licenses** page using the **I already have a key** button, and your license will re-activate.
   
   If you do not plan to re-activate this license on the same server (for example, you wish to activate it on a different TeamCity instance), complete this second step to sever the link between this TeamCity instance and the license.


Similarly to activating a license, the deactivation process may change if you have no access to the related JetBrains account. In this case, copy the **Deactivate** button link before clicking it, and send this link to a JetBrains Account administrator.




## Organizations and Teams

[Teams](https://sales.jetbrains.com/hc/en-gb/articles/360012278699-What-are-teams-in-JetBrains-Account) are separate groups that you can set up in JetBrains Account to distribute different licenses to organization members depending on their tasks.

For TeamCity, additional agent licenses should belong to the same team that owns a server license. Otherwise, your paid agents will not be available after activating a server license. Moreover, any action that affects a team (for example, adding new agent licenses) also alters the corresponding **server key**. This is especially important to remember when working in [offline mode](#Offline+Mode).


If you need to [transfer an agent license to another team](https://sales.jetbrains.com/hc/en-gb/articles/208460205-Transfer-licenses-between-teams), you do not need to deactivate any server licenses in TeamCity. Given that the server licenses of both teams are activated, both TeamCity servers will automatically update their agent numbers.


## Offline Mode

The nature of the JetBrains Account-based license activation mechanism implies that TeamCity periodically connects to `account.jetbrains.com` to refresh the information about owned server and agent licenses. This allows your build server to automatically retrieve the current license information, including both server and agent licenses assigned to your [team](#Organizations+and+Teams).

Although you will not be able to benefit from these automatic updates if TeamCity cannot access `account.jetbrains.com`, activating a TeamCity license is still preferable since it allows your system administrator to get timely email notifications about available server security updates. See the [](#Activate+a+License) section for the instructions.

All license-related changes made in a [team](#Organizations+and+Teams) affect the **server key** (even if the change does not relate to the server directly, for example adding or removing additional agents). Since TeamCity cannot automatically fetch these updates when in offline mode, you need to refresh your server key manually. To do so, copy a <u>server license key</u> from the JetBrains Account page...

<img src="dk-get-license-key.png" width="706" alt="Get license key from JBA"/>

...and paste it to the **Enter your license key** field on the TeamCity **Licenses** page.

<img src="dk-license-offline-refresh.png" width="706" alt="Enter activation key in offline mode"/>

## Legacy Licenses

Existing customers who purchased TeamCity licences previously can still employ the legacy workflow by clicking the **Enter new license key** button and inserting the license key. However, we recommend switching to the JetBrains Account-based workflow to benefit from crucial security email notifications and automatic license updates.

To activate your currently applied licenses using the JetBrains Account:

1. Navigate to **Administration | Licenses** page.
2. Click **Download hashed offline keys** under the "Active license keys" table.
3. Click **Get License Key** and log into your JetBrains Account.
4. You should be able to see the "Already have license keys on your server?" tile that suggests you to convert your existing keys. Click **Upload license keys** and upload the "teamcity-offline-keys.txt" file generated in step 2.
5. Paste the generated key on the **Administration | Licenses** page.


## Limitations and Requirements

A single license can only be used on a single running TeamCity server at any given time. Running secondary TeamCity nodes in addition to the main node does not require a separate license at this time.

If you create a copy of the server and run two servers at the same time, you should ensure each license key is used on a single server only. You can use the Trial (limited time) license to run a server for testing/non-production purposes. The licenses are not bound to a specific server instance, machine, and so on. The only limitation is that a license cannot be used on several servers at the same time.

When you already own license(s) and buy more licenses, you can [request](https://www.jetbrains.com/support/sales/) JetBrains sales to make the new licenses co\-termed with those already purchased, so that all the licenses have equal maintenance expiration date. The cost of the licenses is then lowered proportionally.   
When buying many licenses, you are welcome to [contact](https://www.jetbrains.com/support/sales/) our sales for available volume discounts.