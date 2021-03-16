[//]: # (title: Configuring Agent Requirements)
[//]: # (auxiliary-id: Configuring Agent Requirements)

By specifying [Agent Requirements](agent-requirements.md) for build configuration you can control on which agents the configuration will be run.

To add a requirement, click the corresponding link and specify the following options:

<table>

<tr>

<td></td>
<td></td>

</tr>

<tr>

<td>

Parameter Type

</td>

<td>

Specify the type of the parameter: system property, environment variable, or configuration parameter. For details on the types of parameters available in TeamCity, refer to [this section](configuring-build-parameters.md).

</td></tr><tr>

<td>

Parameter Name

</td>

<td>

Specify the mandatory property or environment variable name.

</td></tr><tr>

<td>

Condition

</td>

<td>

Select condition from the drop-down menu.

__Some notes on how conditions work__:

* __equals__: This condition will be true if an empty value is specified and the specified property exists and its value is an empty string; or if a value is specified and the property exists with the specified value.
* __does not equal__: This condition is true if an empty value is specified and the property exists and its value is NOT empty; or if a specific value is specified and either the property doesn't exist, or the property exists and its value does not equal the specified value.
* __does not contain__: This condition will be true if the specified property either does not exist or exists and does not contain the specified string.
* __is more than__, __is not more than__, __is less than__, __is not less than__: These conditions only work with numbers.
* __matches__, __does not match__: This condition will be true if the specified property matches/does not match the specified [Regular Expression](http://java.sun.com/j2se/1.5.0/docs/api/java/util/regex/Pattern.html) pattern.
* __version is more than__, __version is not more than__, __version is less than__, __version is not less than__: compares versions of a software. Multiple formats are supported including `.`-delimited, leading zeroes, common suffixes like "beta", "EAP". If the version number contains alphabetic characters, they are compared as well, for instance, 1.1e &lt; 1.1g.

</td></tr><tr>

<td>

Value

</td>

<td>

Is shown for the conditions which require the value to match against (like "equals"). The value supports parameters resolution, but only the parameters which do not originate from the agents are supported in the field.`

</td></tr></table>

[//]: # (Internal note. Do not delete. "Configuring Agent Requirementsd68e98.txt")
 Note that the [Agent Requirements](agent-requirements.md) page also displays the list of compatible and incompatible build agents for this build configuration, which is updated each time you modify the list of requirements. Possible reasons why build agent may be incompatible with this build configuration are [described separately](agent-requirements.md).

 <seealso>
        <category ref="concepts">
            <a href="agent-requirements.md">TeamCity Data Backup</a>
            <a href="build-agent.md">Build Agent</a>
            <a href="build-configuration.md">Build Configuration</a>
        </category>
</seealso>