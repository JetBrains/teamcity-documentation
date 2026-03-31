[//]: # (title: Licensing Policy)
[//]: # (help-id: Licensing Policy)

Pricing and new licenses/upgrades are available via the __[official web site](https://www.jetbrains.com/teamcity/buy/)__. If you have any questions on the licensing terms, obtaining or upgrading license keys, or other related topics, please [contact](https://www.jetbrains.com/support/sales/) JetBrains sales department.   
You can review the TeamCity [license agreement](https://www.jetbrains.com/teamcity/buy/license.html) on the official website or in the footer of the installed TeamCity server web UI.

## Licensing Overview

JetBrains offers several licensing options that allow you to scale TeamCity to your needs.  
This section illustrates the main differences between the TeamCity server [editions](#TeamCity+Server+Editions) and provides general information on the TeamCity [Build Agent](install-and-start-teamcity-agents.md) license.

_In general, to use TeamCity for production (Enterprise edition), you need to own one **server license**. In addition, you can acquire an indefinite number of **build agent licenses**, depending on how many builds you want to run in parallel._

For detailed information, refer to the sections below.

## TeamCity Server Editions
{help-id="LicensingPolicy-editions"}

TeamCity server comes in two major editions: Professional and Enterprise. Both editions are installed using the same distribution. You can switch to the Enterprise edition by [entering the appropriate license key](manage-teamcity-license.md). All the data is preserved when the edition is switched.

Apart from these major license types, there are two special TeamCity editions: Enterprise Plus and Open Source.

<deflist type="medium">

<def title="Professional" id="edition-professional">

The free tier of TeamCity that does not require a license key, includes 3 build agents (with an option to purchase more), and has the full range of Enterprise features except for the following limitations:

* Supports the maximum of 100 build configurations and 10 pipelines to exist at the same time (including archived ones). If you exceed this number, new builds will not run until you remove some of the older configurations/pipelines. The default build configurations limit raises by 10 for each additional [agent license](#TeamCity+Agent+Licenses) purchased.
* Enterprise features implemented via paid integrations (for example, [](ai-assistant.md) that runs OpenAI models) are not available.
* Support is limited to the [community forum](https://jb.gg/teamcity-forum) and [public issue tracker](https://youtrack.jetbrains.com/issues/TW).

<note>

We recommend that you [activate a TeamCity license](manage-teamcity-license.md) even when using TeamCity Professional. Doing so ensures your system administrators receive timely email notifications about critical TeamCity server security updates.

</note>

</def>

<def title="Enterprise" id="edition-enterprise">

The [paid](https://www.jetbrains.com/teamcity/buy/#on-premises?licence=enterprise) TeamCity edition that requires a valid license key. TeamCity Enterprise enables the full range of TeamCity features, unlocks the unlimited number of build configurations and pipelines, and offers 1-year subscription to upgrades and [priority support](troubleshooting.md) via a private Zendesk instance. 

<note>

In the [high-availability setup](multinode-setup.md), secondary TeamCity nodes do not require additional licenses and share one with the main server.

</note>

TeamCity Enterprise includes 3 build agents with the ability to purchase additional agent slots. TeamCity Enterprise Plus (see below) allows you to exceed the maximum number of build agents for the true-up agent flexibility.

You can try TeamCity Enterprise in [trial mode](https://www.jetbrains.com/teamcity/download/). The trial license can be obtained only once for each major TeamCity version. A second trial license key is not accepted by the same major version of TeamCity server. If you need to extend or repeat the trial, please [contact](https://www.jetbrains.com/support/sales/) our sales department.

</def>


<def title="Enterprise Plus" id="edition-enterprise-plus">

The premium TeamCity license that offers all TeamCity Enterprise benefits plus the following:

* The ability to **exceed the maximum number of build agents** dictated by the currently active agent licenses. Each additional agent slot costs a fixed amount. At the end of each quarter, you are charged based on the peak overuse in each month. This gives you elastic capacity during busy periods without procurement delays or long-term commitments.

* The **additional staging server license** for testing upgrades and configuration changes before rolling out to production. The staging TeamCity server has a fixed number of agents and does not impose any restrictions on build configurations or pipelines.

* The **assigned customer success engineer** who provides proactive guidance about best practices, along with personalized support to help you address challenges efficiently and align your CI/CD strategy with business goals, maximizing your investment.
</def>


<def title="Open Source" id="edition-open-source">

The special type of license granted for open source projects. This license is time-based and provides an unlimited number of agents. Refer to the details on [this page](https://www.jetbrains.com/teamcity/buy/choose_edition.jsp?license=OPEN_SOURCE).

</def>

</deflist>


## TeamCity Agent Licenses

The number of simultaneously running builds is limited by the maximum number of [build agents](install-and-start-teamcity-agents.md). Both TeamCity Professional and TeamCity Enterprise include 3 build agents by default. You can purchase additional agent slots on the [product page](https://www.jetbrains.com/teamcity/buy/). Each additional agent raises the default TeamCity Professional limit of 100 build configurations by 10.

Agent licenses are not bound to specific agents. Instead, they limit the number of [authorized](install-and-start-teamcity-agents.md#Build+Agent+Statuses) agents regardless of their origin (local or remote, bare-metal or cloud). If you exceed the maximum number of authorized agents, TeamCity displays a warning message in the UI and prevents new builds from starting.

The [Enterprise Plus](#edition-enterprise-plus) server license allows you to exceed the agents threshold for the additional cost charged quarterly.


## Managing Licenses

You can activate server license keys (and enter non-activated legacy keys) on the __Administration__ &gt; __Licenses__ page of the TeamCity web UI. By default, only users with the System Administrator role can access the page.

<include element-id="license-limitations" from="manage-teamcity-license.md"/>

See the following article for more information: [](manage-teamcity-license.md).



## Valid TeamCity Versions

TeamCity licenses are perpetual for the TeamCity versions they cover. This means that you can run a covered TeamCity version with existing licenses for an unlimited time and the licenses will stay valid for this TeamCity version.   
Each TeamCity license (including Enterprise Server and Agent) has a __maintenance period__ (generally 1 year). The license key is valid for any version of TeamCity released before the license purchase as well as for any version released within the maintenance period. Licenses valid for the major release (changes in the first two release numbers) are also considered valid for the corresponding minor (bugfix) updates (changes in the third release number).

The set of valid licenses defines if the server works in Enterprise mode and how many agents can run builds on the server. Agent licenses are not bound to specific agents and are only used to determine the maximum number of authorized agents.

Before you [upgrade](upgrading-teamcity-server-and-agents.md) to a newer TeamCity version, check the validity of the existing licenses with the new version.   
If the new TeamCity server effective [release date](previous-releases-downloads.md) is not covered by the maintenance period of some of the licenses, the corresponding licenses will not be valid with the TeamCity version and would need [renewing](https://www.jetbrains.com/teamcity/buy/index.jsp#upgradeuser). Generally, license renewal is priced at approximately 50% of the new license price, provided licenses are renewed prior to their expiration.

When a new version is available, TeamCity displays a notification in the web UI and warns you if any of your license keys are incompatible with this new version. A notification on the new TeamCity version is also displayed in the Global Configuration Items of the [Server Health](server-health.md) report, visible to system administrators. System administrators can use the link in the "Some Licenses are incompatible" message to quickly navigate to the [Licenses](#Managing+Licenses) page, where all incompatible licenses will have a warning icon. The information about the license keys installed on your server is secure as it is not sent over the Internet.

Regular upgrades are highly recommended not only because each new release includes a lot of improvements and new features, but also as this is the only way to run a supported version with the latest security patch level.

Note that TeamCity [email support](troubleshooting.md) covers only the [recent TeamCity versions](teamcity-release-cycle.md) and can only be provided to customers who are under active maintenance for their Enterprise server license.


<!--[//]: # (Internal note. Do not delete. "Licensing Policyd197e369.txt")-->    

<anchor name="LicensingPolicy-LicenseExpiration"/>

## License Expiration

If an Enterprise license key is removed from the server, or an trial license expires, or a TeamCity server is upgraded to a version released out of the maintenance window of the available Enterprise license, TeamCity automatically switches to the Professional mode.

If the number of build configurations or the number of authorized agents exceeds the limits imposed by the valid licenses, the server stops to start any builds (pauses the build queue) and displays a warning message to all users in the web browser.

Build Agent Licenses work the same way as the Server Licenses. If you upgrade the server to the version which is not covered by the agent license maintenance window, then this agent license will expire.

Once sufficient valid license keys are entered which cover the server configuration, the builds begin to be started again.

<anchor name="LicensingPolicy-WaystoObtainaLicense"/>

## Ways to Obtain a License

The following ways to switch your server into the Enterprise mode exist:
* [buy](https://www.jetbrains.com/teamcity/buy/) an Enterprise Server license;
* request a 30-day trial license on the [download page](https://www.jetbrains.com/teamcity/download/) (see details [above](#TeamCity+Server+Editions));
* use TeamCity for open-source projects only and [request an open-source license](https://www.jetbrains.com/buy/opensource/?product=teamcity).

## Upgrading From Previous Versions

### Upgrading from TeamCity 5.x and later

Each license has a maintenance period (typically one year since the purchase date). The license is suitable for any TeamCity version released within the maintenance period. Please check the maintenance period of your licenses before upgrading.

### Upgrading from TeamCity 4.x to TeamCity 5.0 and later

Licenses for previous versions of TeamCity needs upgrading, see details at [Licensing and Upgrade](https://www.jetbrains.com/teamcity/buy#upgradeuser) section on the official site.

### Upgrading from TeamCity 3.x to TeamCity 4.0

Owners of TeamCity 3.x Enterprise Server Licenses upgrade to TeamCity 4.x Enterprise Edition free of charge. TeamCity 3.x Build Agent Licenses are compatible with both Professional and Enterprise editions of TeamCity 4.0.

### Upgrading from TeamCity 1.x-2.x to TeamCity 4.0

Any TeamCity 1.x\-2.x license purchased before December, 05, 2008 can be used as one TeamCity 4.0 Build Agent license for both Professional and Enterprise editions of TeamCity 4.0. Additionally, TeamCity 1.x\-2.x customers qualify for _one_ TeamCity Enterprise Server License free of charge. To request your Enterprise Server License, please contact [sales department](https://www.jetbrains.com/support/sales/) with one of your TeamCity 1.x\-2.x licenses.

### Upgrading with IntelliJ IDEA 6.0 License Key

Any IntelliJ IDEA 6.0 license purchased between July 12, 2006 and January 15, 2007 can be used as one TeamCity 4.0 Build Agent license. Additionally, IntelliJ IDEA customers with such licenses qualify for one TeamCity Enterprise Server license free of charge.   
To check TeamCity upgrade availability for your IntelliJ IDEA licenses and to request your Enterprise Server license, please contact [sales department](https://www.jetbrains.com/support/sales/) with one of your IntelliJ IDEA licenses purchased within the above period.


<!--[//]: # (Internal note. Do not delete. "Licensing Policyd197e452.txt")-->

## Copyright and Trademark Notice

The software described in this documentation is furnished under a software license agreement. _JetBrains_, _IntelliJ_, _IntelliJ IDEA_, _YouTrack_, and _TeamCity_ are trademarks or registered trademarks of _JetBrains, s.r.o._ _Windows_ is a registered trademark of _Microsoft Corporation_ in the United States and other countries. _Mac,_ _Mac OS, macOS_ are trademarks of _Apple Inc._, registered in the U.S. and other countries. _Linux_ is a registered trademark of _Linus Torvalds_. All other trademarks are the properties of their respective owners.

<seealso>
        <category ref="concepts">
            <a href="install-and-start-teamcity-agents.md">Build Agent</a>
        </category>
        <category ref="licensing">
            <a href="https://www.jetbrains.com/teamcity/buy/">Licensing &amp; Upgrade</a>
            <a href="teamcity-release-cycle.md">TeamCity Release Cycle</a>
        </category>
</seealso>
