[//]: # (title: TeamCity Release Cycle)
[//]: # (auxiliary-id: TeamCity Release Cycle)

>The information on this page can be used for reference purposes only.
> 
{type="note"}

## Why Upgrade TeamCity
{product="tc"}

TeamCity is systematically and frequently updated with new features and optimizations. __We highly recommend upgrading your server and agents as soon as the new version is released__. Each release introduces the following key improvements:
* __new functionality__: including new runners, build features, UI enhancements, integration with third-party software, and instruments for customization. The recent [release notes](what-s-new-in-teamcity.md) give an example of the number and scale of features provided per major release.
  New features are mostly released in major versions. To ensure smooth upgrade/downgrade between minor versions, bugfix updates do not add any important features but aim at fixing occurring problems.
* __performance improvements__: one of the primary focuses of our team is to make TeamCity as fast and responsive as possible, which includes smooth UX, stable server performance, reasonable utilization of hardware resources, and many other aspects.
* __security updates__: to comply with the industry-best security practices, we continuously analyze TeamCity and introduce multiple security improvements per release (for more details on security in TeamCity, read [these notes](security-notes.md)).
* __bug fixes__: thanks to our QA team and active user feedback, we can quickly catch and fix even rare bags. If we notice a critical bug soon after releasing another TeamCity version, it is our priority to release the respective patch or the next bugfix update as soon as possible, which makes it especially crucial that you don't skip regular updates. Any news concerning patches, release issues, and upgrade notes are published [here](upgrade-notes.md).

Major updates are released twice a year, and each major release is followed by multiple minor (bugfix) releases. Read more about the release stages in the [following section](#Release+Stages).

## Why Upgrade TeamCity
{product="tcc"}

TeamCity is systematically and frequently updated with new features and optimizations. Each release introduces the following key improvements:
* __new functionality__: including new runners, build features, UI enhancements, integration with third-party software, and instruments for customization.  
  New features are mostly released in major versions. To ensure smooth upgrade/downgrade between minor versions, bugfix updates do not add any important features but aim at fixing occurring problems.
* __performance improvements__: one of the primary focuses of our team is to make TeamCity as fast and responsive as possible, which includes smooth UX, stable server performance, reasonable utilization of hardware resources, and many other aspects.
* __security updates__: to comply with the industry-best security practices, we continuously analyze TeamCity and introduce multiple security improvements per release (for more details on security in TeamCity, read [these notes](security-notes.md)).
* __bug fixes__: thanks to our QA team and active user feedback, we can quickly catch and fix even rare bags. If we notice a critical bug soon after releasing another TeamCity version, it is our priority to release the respective patch or the next bugfix update as soon as possible, which makes it especially crucial that you don't skip regular updates.

Major updates are released twice a year, and each major release is followed by multiple minor (bugfix) releases.

## Version Numbers

A major release is represented with the `YYYY.N` number, where `YYYY` is the release year and `N` is the serial number of the release during this year. For example, `2020.1` and `2020.2` are two major versions released in year 2020.

A minor release is represented with the `YYYY.N.M` number, where `YYYY.N` corresponds to its preceding major release and `M` is the serial number of the minor release. For example, `2020.2.2` is the second bugfix update released for major version `2020.2`.

The dates of all previous releases and the sequence of TeamCity versions are listed [here](previous-releases-downloads.md).
{product="tc"}

## Release Stages
{product="tc"}

The default stages of a TeamCity release:

<table>

<tr>

<td>Release Stage</td>
<td>Description</td>

</tr>

<tr>

<td>

__Early Access Program (EAP)__

</td>

<td>

Allows trying a new version of TeamCity prior to its official release, while the new features are in the development state.

Usually available only for _major releases_, starts several months before the major release it corresponds to. Typically, new EAP releases are published monthly or bimonthly.

>For EAP builds, we offer a special [evaluation license](licensing-policy.md#evaluation-license).

[EAP download page](https://www.jetbrains.com/teamcity/nextversion/)

</td>

</tr>

<tr>

<td>

__General Availability__

</td>

<td>

A new _major version_ becomes _generally available_ after its release and stays in this stage until the next major version is released.

A major version is usually released every 6-7 months. There are multiple _minor (bugfix) releases_ following the major release. Bugfix releases and support patches for critical issues, if applicable, are provided until __End of Sale__ of the release.

[Release download page](https://www.jetbrains.com/teamcity/download/)

</td>

</tr>

<tr>

<td>

__End of Sale__

</td>

<td>

Occurs for the previous major version with the release of the next major version. After this time, no bugfix updates or patches are usually provided (except critical issues without a workaround which allow for a relatively simple fix, or make it impossible to upgrade to the next version). Only limited support is provided for a major version after its end of sale.

</td>

</tr>

<tr>

<td>

__End of Support__

</td>

<td>

Occurs with the release of two newer major versions. At this point, we stop providing regular technical support for the release.

</td>

</tr>

</table>


<seealso>
        <category ref="installation">
            <a href="licensing-policy.md" product="tc">Licensing Policy</a>
            <a href="previous-releases-downloads.md">Previous Releases Downloads</a>
            <a href="upgrading-teamcity-server-and-agents.md">Upgrade</a>
        </category>
</seealso>