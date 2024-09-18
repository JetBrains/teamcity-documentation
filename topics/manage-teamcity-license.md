[//]: # (title: Manage TeamCity Licenses)

Users with System Administrator role can manage TeamCity server and agent licenses on the **Administration | Licenses** page. We recommend activating your server license, even for the free Professional version, to ensure system administrators receive timely email notifications about critical TeamCity server security updates.

For users with paid licenses, activating a license grants the "set it and forget it" experience: once your instance is linked with the [JetBrains Account](https://account.jetbrains.com) (JBA), you no longer need to manually enter license keys for every additional agents batch you purchase or every server license update. TeamCity will automatically retrieve information about all currently active licenses, and apply them on-the-fly.

## Activate a License

If you can access the <a href="https://account.jetbrains.com">JetBrains Account</a> and your TeamCity server's machine is connected to the internet and can regularly communicate with <a href="https://account.jetbrains.com">account.jetbrains.com</a> via HTTPS, do the following to activate a license:

1. Navigate to **Administration | Licenses** page.
2. Click **Get License Key** and log into your JetBrains Account. Note that TeamCity does not display this button if the server already has an active license. If your server uses a trial license, [deactivate it](#Deactivate+a+License) before you can switch to a paid version.
3. Choose a server license you want to activate. This page shows licenses owned by all organizations and teams for which you have administrator permissions. See this page for information on permissions required to activate a license: [Required permissions to activate your on-premises team tool license](https://www.google.com/url?q=https://sales.jetbrains.com/hc/en-gb/articles/20070316403090&sa=D&source=docs&ust=1720640931638823&usg=AOvVaw1UTiHZ6e45kndMi-CoDhWL).
   <img src="dk-choose-license.png" width="706" alt="Choose a license"/>
4. Click **Get license key** to confirm. The activation key will be transferred to your TeamCity server automatically. You can also manually copy a key and paste it to the **I already have a key** field on the TeamCity **Licenses** page.




<dl>

<dt>Case 1: You are not a JetBrains Account administrator</dt>
<dd>

<b>Solution:</b> Request [permissions required to activate TeamCity licenses](https://www.google.com/url?q=https://sales.jetbrains.com/hc/en-gb/articles/20070316403090&sa=D&source=docs&ust=1720640931686456&usg=AOvVaw3sYrLj8H_zwxc8ycrK-784). Alternatively, follow the same procedure as above and ask your [JetBrains Account administrator](https://sales.jetbrains.com/hc/en-gb/articles/207739139-Administrator-and-user-roles-in-JetBrains-Account#h_01J0Y2GEAFYT6RQ0JX8YN6Y5FX) to perform actions you have no permissions to do.

<ol>

<li>On the <b>Administration | Licenses</b> page, right-click the <b>Get License Key</b> button and copy the link.</li>

<li>Send this link to your JBA administrator. This link contains your unique TeamCity instance key and looks like the following: <code>https://account.jetbrains.com/license/activate?product=TCS&amp;instPubKey=&lt;your_public_key&gt;</code>.</li>

</ol>

Once the administrator follows this link and activates a license, your TeamCity server will automatically apply it. All future license updates (including build agent license assignments and expirations) will be automatically communicated to your server; you will not require further administrator assistance after the activation is complete.

</dd>

<dt>Case 2: Your TeamCity server machine is not connected to the internet</dt>

<dd>

<b>Solution:</b> Similarly to the previous case, you can utilize the activation link generated in TeamCity.

<ol>

<li>On the <b>Administration | Licenses</b> page, right-click the <b>Get License Key</b> button and copy the link.</li>

<li>Open this link on a trusted device that has access to the JetBrains Account. This link contains your unique TeamCity instance key and looks like the following: <code>https://account.jetbrains.com/license/activate?product=TCS&amp;instPubKey=&lt;your_public_key&gt;</code>.</li>

<li>
After you activate a license in your JetBrains Account, copy or save the activation key.

<img src="dk-activation-successful.png" width="706" alt="Successful activation"/>
</li>

<li>

On the TeamCity <b>Licenses</b> page, click <b>I already have a key</b> and paste the activation key.

</li>

</ol>


Note that since an offline TeamCity server is unable to receive automatic updates from JBA, you will need to copy and paste **server** license keys whenever the server license is renewed or additional agents are purchased.

See also: <a href="configuring-proxy-server.md">Configuring Proxy Server</a>, [Offline Mode](#Offline+Mode).

</dd>

</dl>


### Activate Existing Legacy Licenses

Existing customers who purchased TeamCity licenses previously can still employ the legacy workflow by clicking the **Enter new license key** button and inserting the license key. However, we recommend switching to the JetBrains Account-based workflow to benefit from crucial security email notifications and automatic license updates.


To activate your currently active legacy server license, click the **Get License Key** in TeamCity UI to get into the JetBrains Account. Your legacy license should be recognized and pre-selected in the list of all available licenses. Ensure this is the correct license and follow instructions on your screen to proceed.

If the JetBrains Account is unable to recognize your legacy license, use an alternative approach as shown below:

1. Go to the **Administration | Licenses** page in TeamCity.
2. Click **Download hashed offline keys** under the "Active license keys" table.
3. Click **Get License Key** and log into your JetBrains Account.
4. You should be able to see the "Already have license keys on your server?" tile that suggests you to convert your existing keys. Click **Upload license keys** and upload the "teamcity-offline-keys.txt" file generated in step 2.
5. Review the information about your license and click **Get license key** to generate an updated key.
6. Paste the generated key on the **Administration | Licenses** page.


## Deactivate a License

You may want to deactivate a server license to switch to another server license you have or [transfer it to another team](https://sales.jetbrains.com/hc/en-gb/articles/208460205-Transfer-licenses-between-teams) that has a separate TeamCity instance.

Deactivating a license is the opposite of the [activation process](#Activate+a+License): you need to remove a license from TeamCity and (optionally) deactivate it in the JetBrains account.

1. To remove a license from TeamCity, click **Deactivate** on the **Administration | Licenses** page. In case you had a paid license, your server will revert back to the free Professional tier.

2. After you click **Deactivate** in TeamCity, you can navigate to your JetBrains Account to complete the deactivation process.

   If you do not complete this step, the server license (although inactive in TeamCity) will remain linked to this specific server instance. This means you cannot activate it on another server.


<dl>

<dt>Case 1: You are not a JetBrains Account administrator or TeamCity server is offline</dt>
<dd>

<b>Solution:</b> Similarly to the activation process, copy the **Deactivate** button link and send it to your [JetBrains Account administrator](https://sales.jetbrains.com/hc/en-gb/articles/207739139-Administrator-and-user-roles-in-JetBrains-Account#h_01J0Y2GEAFYT6RQ0JX8YN6Y5FX).

</dd>

<dt>Case 2: You forgot to deactivate a server license and uninstalled the TeamCity server instance. The license is now linked to a non-existent server and cannot be transfered to another instance/team</dt>

<dd>

<b>Solution:</b> Navigate to the JetBrains account and find this active license, then click the **Deactivate** link.

<img src="dk-deactivate-from-jba.png" width="706" alt="Deactivate from JBA"/>

This is an emergency workaround that should only be used when the TeamCity instance is not accessible. In any other case, initiate the deactivation process from the TeamCity UI.


</dd>

</dl>





## Manage Build Agent Licenses

Unlike individual accounts, organization JetBrains accounts can create [teams](https://sales.jetbrains.com/hc/en-gb/articles/360012278699-What-are-teams-in-JetBrains-Account) to distribute different licenses to organization members depending on their tasks.

For TeamCity, additional agent licenses should belong to the same team that owns a server license. Otherwise, your paid agents will not be available after activating a server license. Moreover, any action that affects a team (for example, adding new agent licenses) also alters the corresponding **server key**. This is especially important to remember when working in [offline mode](#Offline+Mode).


If you need to [transfer an agent license to another team](https://sales.jetbrains.com/hc/en-gb/articles/208460205-Transfer-licenses-between-teams), you do not need to deactivate any server licenses in TeamCity. Given that the server licenses of both teams are activated, both TeamCity servers will automatically update their agent numbers.

See also: [Managing TeamCity build agents](https://sales.jetbrains.com/hc/en-gb/articles/20114538108306-Managing-TeamCity-build-agents).


## Offline Mode

The nature of the JetBrains Account-based license activation mechanism implies that TeamCity periodically connects to `account.jetbrains.com` to refresh the information about owned server and agent licenses. This allows your build server to automatically retrieve the current license information, including both server and agent licenses assigned to your [team](#Manage+Build+Agent+Licenses).

Although you will not be able to benefit from these automatic updates if TeamCity cannot access `account.jetbrains.com`, activating a TeamCity license is still preferable since it allows your system administrator to get timely email notifications about available server security updates. See the [](#Activate+a+License) section for the instructions.

All license-related changes made in a [team](#Manage+Build+Agent+Licenses) affect the **server key** (even if the change does not relate to the server directly, for example adding or removing additional agents). Since TeamCity cannot automatically fetch these updates when in offline mode, you need to refresh your server key manually. To do so, copy a <u>server license key</u> from the JetBrains Account page...

<img src="dk-get-license-key.png" width="706" alt="Get license key from JBA"/>

...and paste it to the **Enter your license key** field on the TeamCity **Licenses** page.

<img src="dk-license-offline-refresh.png" width="706" alt="Enter activation key in offline mode"/>

The same page allows you to acquire legacy license keys (the **Legacy license key** link). If you are an individual (non-organization) customer, refer to this knowledge base article for more information: [Downloading TeamCity legacy license keys](https://sales.jetbrains.com/hc/en-gb/articles/20070395820562).





## Limitations and Requirements

<snippet id="license-limitations">

A single license can only be used on a single running TeamCity server at any given time. Running secondary TeamCity nodes in addition to the main node does not require a separate license at this time.

If you create a copy of the server and run two servers at the same time, you should ensure each license key is used on a single server only. You can use the Trial (limited time) license to run a server for testing/non-production purposes. The licenses are not bound to a specific server instance, machine, and so on. The only limitation is that a license cannot be used on several servers at the same time.

When you already own license(s) and buy more licenses, you can [request](https://www.jetbrains.com/support/sales/) JetBrains sales to make the new licenses co\-termed with those already purchased, so that all the licenses have equal maintenance expiration date. The cost of the licenses is then lowered proportionally.   
When buying many licenses, you are welcome to [contact](https://www.jetbrains.com/support/sales/) our sales for available volume discounts.


</snippet>
