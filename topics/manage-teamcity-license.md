[//]: # (title: Manage TeamCity Licenses)

Users with System Administrator role can manage TeamCity server and agent licenses on the **Administration | Licenses** page. We recommend activating your server license, even for the free Professional version, to ensure system administrators receive timely email notifications about critical TeamCity server security updates.

## Activate a License

Starting with version 2024.07, TeamCity interacts with the [JetBrains Account](https://account.jetbrains.com) (JBA) to automatically retrieve the information about your all your purchased server and agent licenses.

If a machine that runs your TeamCity server is connected the internet and can access `account.jetbrains.com`:

1. Navigate to **Administration | Licenses** page.
2. Click **Get License Key** and log into your JetBrains Account.
3. Choose a license you want to activate. The license will get activated automatically.

If your TeamCity server cannot access `account.jetbrains.com`:

1. Log into your JetBrains Account using a device that has access to this page.
2. Choose a license you want to activate and copy or download the key.
3. In TeamCity, go to **Administration | Licenses** and click **I already have a key** to paste it.

Existing customers who purchased TeamCity licences previously can still employ the legacy workflow by clicking the **Enter new license key** button and inserting the license key. However, we recommend switching to the JBA-based workflow to benefit from crucial security email notifications and a single place to manage all of your purchases. To transfer your existing licences to the JBA-based workflow:

1. Navigate to **Administration | Licenses** page.
2. Click **Download hashed offline keys** under the **Active license keys** table.
3. Click **Get License Key** and log into your JetBrains Account.
4. You should be able to see the "Already have license keys on your server?" tile that suggests you to convert your existing keys. Click the **Upload license keys** and upload the "teamcity-offline-keys.txt" file generated in step 2.
5. Paste the updated key on the **Administration | Licenses** page.


## Revoke a License

You may want to revoke a TeamCity server or agent license to [transfer it to another team](https://sales.jetbrains.com/hc/en-gb/articles/208460205-Transfer-licenses-between-teams) within your organization. To revoke a license, click a corresponding button at the top of the **Licenses** page.

Revoking a license in TeamCity does not automatically unassign it from your team, you need to do that manually in your JetBrains account. 

> When transferring licenses between JetBrains Account teams, remember that agent and server licenses should belong to the same team. Otherwise, you will not be able to activate additional agents on this server.
> 
{type="note"}


See this article to learn more about teams: [What are teams in JetBrains Account](https://sales.jetbrains.com/hc/en-gb/articles/360012278699-What-are-teams-in-JetBrains-Account).


## Offline Mode

When a server license is active, TeamCity periodically connects to `account.jetbrains.com` to refresh the information about owned server and agent licenses. If the server is unable to do so, the **Licenses** page shows a corresponding warning.


This warning does not prompt any immediate action. You only need to click **Enter your license key**  after making changes in your JetBrains Account, since TeamCity is unable to fetch this information automatically when in offline mode.


## Limitations and Requirements

A single license can only be used on a single running TeamCity server at any given time. Running secondary TeamCity nodes in addition to the main node does not require a separate license at this time.

If you create a copy of the server and run two servers at the same time, you should ensure each license key is used on a single server only. You can use the Evaluation (limited time) license to run a server for testing/non-production purposes. The licenses are not bound to a specific server instance, machine, and so on. The only limitation is that a license cannot be used on several servers at the same time.

When you already own license(s) and buy more licenses, you can [request](https://www.jetbrains.com/support/sales/) JetBrains sales to make the new licenses co\-termed with those already purchased, so that all the licenses have equal maintenance expiration date. The cost of the licenses is then lowered proportionally.   
When buying many licenses, you are welcome to [contact](https://www.jetbrains.com/support/sales/) our sales for available volume discounts.