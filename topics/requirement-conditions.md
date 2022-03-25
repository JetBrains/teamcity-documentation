[//]: # (title: Requirement Conditions)
[//]: # (auxiliary-id: Requirement Conditions)

This page explains conditions used in [agent requirements](agent-requirements.md) or [build step execution requirements](build-step-execution-conditions.md).

<table>
<tr><td>Condition</td><td>Description</td><td>Example</td></tr>

<tr><td>

__equals__

</td><td>

Is `true` if:
* A value is empty and the specified property exists and its value is an empty string.  
  _or_
* A value is specified and the property exists with the specified value.

</td>

<td>

`docker.server.osType` __equals__ `Linux`

</td>

</tr>

<tr><td>

__does not equal__

</td><td>

Is `true` if:
* A value is empty and the specified property exists and its value is __NOT__ empty.  
  _or_
* A value is specified and either the property does not exist, or the property exists and its value does not equal the specified value.

</td>

<td>

`system.teamcity.buildType.id` __does not equal__ `Tests`

</td>

</tr>

<tr><td>

__does not contain__

</td><td>

Is `true` if the specified property either does not exist, or exists and does not contain the specified string.

</td>

<td>

`system.agent.name` __does not contain__ `_local`

</td>

</tr>

<tr><td>

* __is more than__
* __is not more than__
* __is less than__
* __is not less than__

</td><td>

Work only with numeric values.

</td>

<td>

`build.number` __is more than__ `256`

</td>

</tr>

<tr><td>

* __matches__
* __does not match__

</td><td>

Is `true` if the specified property matches / does not match the specified [regular expression](http://java.sun.com/j2se/1.5.0/docs/api/java/util/regex/Pattern.html) pattern.

</td>

<td>

Use `<custom_parameter>` __matches__ `.*(,|^)foo(,|$).*` to match all occurrences of `foo`, `foo,bar`, `bar,foo`, and `bar,foo,xxx`.

</td>

</tr>

<tr><td>

* __version is more than__
* __version is not more than__
* __version is less than__
* __version is not less than__

</td><td>

Compares versions of a software. Multiple formats are supported including `.`-delimited, leading zeroes, common suffixes like "beta", "EAP". If the version number contains alphabetic characters, they are compared as well, for instance, 1.1e &lt; 1.1g.

</td>

<td>

`maven` __version is more than__ `3.0`

</td>

</tr>

</table>