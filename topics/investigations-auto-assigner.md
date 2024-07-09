[//]: # (title: Investigations Auto Assigner)
[//]: # (auxiliary-id: Investigations Auto Assigner)

TeamCity can analyze build problems (for example, compilation errors) and test failures, and try to identify users whose commits potentially led to these problems.

For failed tests, TeamCity suggests these potentially responsible users as investigators.

<img src="dk-investigator-suggestions.png" width="706" alt="Automatic Investigation Suggestions"/>



In addition to suggestions, you can now add the *Investigations Auto Assigner* [build feature](adding-build-features.md) that uses the same heuristics to assign investigations of failed tests and build problems automatically. A user assigned as an investigator will see the corresponding notification next to the **Administration** button.

<img src="dk-autoinvestigation.png" width="706" alt="Investigation automatically assigned to a user"/>

## Feature Settings

The Investigations Auto Assigner feature is functional out of the box. All of its settings are optional.

<img src="dk-autoassigner-settings.png" width="706" alt="AutoAssigner Feature Settings"/>

<table>

<tr>
<th>Setting Name</th>
<th>Description</th>
</tr>

<tr>
<td>Assign</td>

<td>

Allows you to ignore the first occurrences of build issues and automatically assign investigators only for repeatedly occurring problems. See this section for more information: [Delay Automatic Assignment](#Delay+Automatic+Assignment).

</td></tr>
<tr>

<td>Default assignee</td>
<td>

The Investigations Auto Assigner attempts to identify the following users who are potentially responsible for a failure:

<ul>
<li>A user who was the only committer to the build.</li>
<li>A user who was the only committer to the suspicious file. A suspicious file is the one whose name appears in the test or build problem error text.</li>
<li>A user who was previously responsible for this problem.</li>
</ul>

If no such user was found, the investigation will be assigned to the default assignee.

<br/>

<tip>

When specifying users, enter usernames instead of public names.

<img src="dk-autoassigner-username.png" width="460" alt="User Name"/>

</tip>



</td>
</tr>

<tr>

<td>Users to ignore</td>
<td>The list of users who should never be automatically appointed as investigators, even if they satisfy the aforementioned conditions.</td>
</tr>

<tr>
<td>Build problems to ignore</td>
<td>Choose the types of problems that do not require investigators to be assigned automatically.</td>
</tr>


</table>


## Delay Automatic Assignment

<anchor name="delay-auto-assign"/>

If necessary, you can delay the auto-assignment of investigations.   
TeamCity needs time after the build finish to detect [flaky tests](investigating-and-muting-build-failures.md#Flaky+Tests). In some cases, Investigations Auto Assigner may assign an investigation to a user even if the build fails without any user involvement. When there are many flaky tests in a project, this may be distracting.   


To prevent this scenario, select __Assign__: "_On second failure_" when configuring this build feature. Investigations Auto Assigner will delay the assignment until the problem repeats in a build configuration twice in a row (in the default branch). This option affects only failed tests and "Exit code" build problems; any compilation errors will be assigned with an investigation right on the first failure.
