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

Parameter type

</td>

<td>

Specify the type of the parameter: system property, environment variable, or configuration parameter. For details on the types of parameters available in TeamCity, refer to [this section](configuring-build-parameters.md).

</td></tr><tr>

<td>

Parameter name

</td>

<td>

Specify the mandatory property or environment variable name.

</td></tr><tr>

<td>

Condition

</td>

<td>

Choose an appropriate condition for the new requirement. See [how conditions work in TeamCity](requirement-conditions.md).

</td></tr><tr>

<td>

Value

</td>

<td>

Is shown for the conditions which require the value to match against (like "equals"). The value supports parameters resolution, but only the parameters which do not originate from the agents are supported in the field.

</td></tr></table>

[//]: # (Internal note. Do not delete. "Configuring Agent Requirementsd68e98.txt")
Note that the __[Agent Requirements](agent-requirements.md)__ page also displays the list of compatible and incompatible build agents for this build configuration, which is updated each time you modify the list of requirements. Possible reasons why build agent may be incompatible with this build configuration are [described separately](agent-requirements.md).

 <seealso>
        <category ref="concepts">
            <a href="agent-requirements.md">TeamCity Data Backup</a>
            <a href="build-agent.md">Build Agent</a>
            <a href="build-configuration.md">Build Configuration</a>
        </category>
</seealso>