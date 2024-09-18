[//]: # (title: TeamCity Release Cycle)
[//]: # (auxiliary-id: TeamCity Release Cycle)

>The information on this page can be used for reference purposes only.
> 
{style="note"}

## Why Upgrade TeamCity
{product="tc"}

TeamCity is systematically and frequently updated with new features and optimizations. __We highly recommend upgrading your server and agents as soon as the new version is released__. Each release introduces the following key improvements:
* __New functionality__: including new runners, build features, UI enhancements, integration with third-party software, and instruments for customization. The recent [release notes](what-s-new-in-teamcity.md) give an example of the number and scale of features provided per major release.  
  New features are mostly released in major versions. To ensure smooth upgrade/downgrade between minor versions, bugfix updates do not add any important features but aim at fixing occurring problems.
* __Performance improvements__: one of the primary focuses of our team is to make TeamCity as fast and responsive as possible, which includes smooth UX, stable server performance, reasonable utilization of hardware resources, and many other aspects.
* __Security updates__: to comply with the industry-best security practices, we continuously analyze TeamCity and introduce multiple security improvements per release (for more details on security in TeamCity, read [these notes](security-notes.md)).
* __Bug fixes__: thanks to our QA team and active user feedback, we can quickly catch and fix even rare bugs. If we notice a critical bug soon after releasing another TeamCity version, it is our priority to release the respective patch or the next bugfix update as soon as possible, which makes it especially crucial that you don't skip regular updates. Any news concerning patches, release issues, and upgrade notes are published [here](upgrade-notes.md).

Major updates are released twice a year, and each major release is followed by multiple minor (bugfix) releases. Read more about the release stages in the [following section](#Release+Stages).

## Version Numbers
{product="tc"}

A major release is represented by the `YYYY.MM` number, where `YYYY` is the release year and `MM` is the number of the release month. For example, `2022.04` and `2022.10` are two major versions released in year 2022, in April and October.

A minor release is represented by the `YYYY.MM.B` number, where `YYYY.MM.B` corresponds to its preceding major release and `B` is the serial number of the minor (bugfix) release. For example, `2022.04.1` is the first bugfix update released for major version `2022.04`.

The dates of all previous releases and the sequence of TeamCity versions are listed [here](previous-releases-downloads.md).
{product="tc"}

## Release Stages

>Since version 2021.12, TeamCity adopts a new scheme of releases. We are discontinuing a former Early Access Program in favor of frequent and stable TeamCity Cloud releases, in which all features will be polished and ready to be used in production, but tested only in the Cloud environment.
> 
>The TeamCity Cloud infrastructure allows releasing new features more frequently than in On-Premises: they are now rolled out to the Cloud instances bimonthly. On-Premises instances can be upgraded to a new major version twice a year, exactly as before.
> 
{style="note"}

The default stages of a TeamCity release:

<table>

<tr>

<td>Release Stage</td>
<td>Description</td>

</tr>

<tr>

<td>

__TeamCity Cloud Major Release__

</td>

<td>

This is the first official version where the new features are rolled out to. It is released approximately once in two months as `YYYY.MM`.

See [TeamCity Cloud licensing policy](https://www.jetbrains.com/help/teamcity/cloud/teamcity-cloud-subscription-and-licensing.html).

</td>

</tr>

<tr>

<td>

__TeamCity On-Premises Major Release__

</td>

<td>

A major TeamCity On-Premises version is usually released every 4 months. There are multiple _minor (bugfix) releases_ following the major release. Bugfix releases and support patches for critical issues, if applicable, are provided until __End of Sale__ of the release.

[Release download page](https://www.jetbrains.com/teamcity/download/)

</td>

</tr>

<tr>

<td>

__TeamCity On-Premises End of Sale__

</td>

<td>

Occurs for the previous major version with the release of the next major version. After this time, no bugfix updates or patches are usually provided (except critical issues without a workaround which allow for a relatively simple fix, or make it impossible to upgrade to the next version). Only limited support is provided for a major version after its end of sale.

</td>

</tr>

<tr>

<td>

__TeamCity On-Premises End of Support__

</td>

<td>

Occurs with the release of two or three newer major versions. The exact date depends on the frequency of releases, but as a general rule, this happens a year after the initial release date. At this point, we stop providing regular technical support for the On-Premises release.

</td>

</tr>

</table>


<seealso>
        <category ref="installation">
            <a href="licensing-policy.md" product="tc">Licensing Policy</a>
            <a href="previous-releases-downloads.md" product="tc">Previous Releases Downloads</a>
            <a href="upgrading-teamcity-server-and-agents.md" product="tc">Upgrade</a>
        </category>
</seealso>