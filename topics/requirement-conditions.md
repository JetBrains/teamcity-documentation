[//]: # (title: Requirement Conditions)
[//]: # (auxiliary-id: Requirement Conditions)

This page explains conditions used in [agent requirements](agent-requirements.md) or [build step execution requirements](build-step-execution-conditions.md).



<table>
<tr><td>Condition</td><td>Description</td><td>Example</td></tr>

<tr>
<td>

**exists**

</td><td>

Returns `true` if the specified property exists.

</td><td>

`DotNetCLI_Path` **exists**

`(DotNetCoreSDK7\.[\.\d]*_Path)` **exists**

</td>
</tr>


<tr>
<td>

**does not exist**

</td><td>

Returns `true` if the specified property does not exist.

</td><td>

`env.TEAMCITY_GIT_PATH` **does not exist**

</td>
</tr>

<tr>
<td>

**equals**

</td><td>

Returns `true` if the specified property exists and equals the given value.

You can leave the "value" field empty to check whether the specified property exists but is empty.

</td><td>

`docker.server.osType` __equals__ `Linux`

</td>
</tr>

<tr><td>

**does not equal**

</td><td>

Returns `true` if a value of the specified property differs from the given value, or if this property does not exist.

You can leave the "value" field empty to check whether the specified property exists and is not empty (has a value).

</td><td>

`system.teamcity.buildType.id` __does not equal__ `Tests`

</td>
</tr>



<tr><td>

* __is more than__
* __is not more than__
* __is less than__
* __is not less than__

</td><td>

Return `true` if a value of the target numeric property meets the corresponding condition.

</td><td>

`build.number` __is more than__ `256`

`teamcity.agent.hardware.cpuCount` __is less than__ `10`

</td>

</tr>


<tr><td>

* __version is more than__
* __version is not more than__
* __version is less than__
* __version is not less than__

</td><td>

Compare software versions with the given value. Supports multiple value formats, including dot (`.`) delimiters, leading zeroes, common suffixes like "beta", "EAP". If the version number contains alphabetic characters, they are compared as well, for instance, 1.1e &lt; 1.1g.

</td>

<td>

`maven` __version is more than__ `3.0`

</td>

</tr>


<tr><td>

**contains**

</td><td>

Returns `true` if a value of the specified property includes the given value.

</td><td>

`teamcity.serverUrl` __contains__ `localhost`

</td>
</tr>


<tr><td>

**does not contain**

</td><td>

Returns `true` if a value of the specified property does not include the given value, or if this property does not exist.

</td>

<td>

`system.agent.name` **does not contain** `_local`

</td></tr>


<tr><td>

* **starts with**
* **ends with**

</td><td>

Returns `true` if a value of the specified property starts (ends) with the given value.

</td>

<td>

`teamcity.agent.jvm.os.name` **starts with** `Mac`

`teamcity.agent.work.dir` **ends with** `/work`

</td>
</tr>




<tr><td>

* __matches__
* __does not match__

</td><td>

Returns `true` if the specified property matches (does not match) the given [regular expression](https://java.sun.com/j2se/1.5.0/docs/api/java/util/regex/Pattern.html) pattern.

</td>

<td>

Use `<custom_parameter>` __matches__ `.*(,|^)foo(,|$).*` to match all occurrences of `foo`, `foo,bar`, `bar,foo`, and `bar,foo,xxx`.

</td>

</tr>



</table>