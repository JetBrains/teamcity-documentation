[//]: # (title: Typed Parameters)
[//]: # (auxiliary-id: Typed Parameters)

When adding a [build parameter](configuring-build-parameters.md) (system property, environment variable, or configuration parameter), you can extend its definition with meta-information, or _specification_. The parameter's specification defines how its controls are presented and validated in the _[Run Custom Build](running-custom-build.md)_ dialog.

By adding a typed specification to a parameter, you make it a _typed parameter_. Typed parameters are more understandable for non-developers and make experience with builds more user-friendly.

Example: a build configuration has a parameter that defines if a build has to include a license or not. The parameter can be either true or false, and is false by default. For a regular user, it might not be clear which build parameter is responsible for the license generation and what its values mean. It is not a problem when builds run with the default configuration, but if some user wants to run a custom build, the variety of available parameters might confuse them. Using the build parameter's specification, you can make the parameters' appearance in the _Run Custom Build_ dialog more readable.

## Adding Parameter Specification

To add a specification to a build parameter, click __Edit__ in the __Spec__ section when editing/adding a build parameter.

All parameters' specifications support the following common properties:
* __Label__: a text that is displayed near the control in the _Run Custom Build_ dialog.
* __Description__: a text that is displayed below the control containing an explanatory note of the control use.
* __Display__:
   * if _hidden_ is selected, the parameter will not be displayed in the _Run Custom Build_ dialog, but will be sent to a build;
   * if _prompt_ is selected, TeamCity will always require a review of the parameter's value when clicking __Run__ (won't require the parameter if the build is triggered automatically);
   * if _normal_ is selected, the parameter will be displayed as usual.
* __Read-only__: if enabled, it will be impossible to override the parameters value.
* __Type__:
   * a simple text field with the ability to validate its value using a regular expression;
   * a checkbox;
   * a select control;
   * a password field.

The table below provides more details on each control type.

<table><tr>

<td width="150">

Type

</td>

<td>

Description

</td></tr><tr>

<td>

Text

</td>

<td>

The default option. Represents a usual text string without any extra handling.

</td></tr><tr>

<td>

Checkbox

</td>

<td>

A true/false option represented by a checkbox.

</td></tr><tr>

<td>

Select

</td>

<td>

A "select one" or "select many" control to set the value to one of the predefined settings.

</td></tr><tr>

<td>

Password

</td>

<td>

This type is designed to store passwords or other secure data in TeamCity settings. TeamCity makes the value of the password parameter never appear in the TeamCity UI: including the settings' screens and the _Run Custom Build_ dialog where password fields appear. The value is also replaced in the build's __Parameters__ tab and build log.

The password value is stored in the configuration files under [TeamCity Data Directory](teamcity-data-directory.md). Depending on the server [encryption settings](teamcity-configuration-and-maintenance.md#encryption-settings), the value is either scrambled or encrypted with a custom key.
{product="tc"}

The password value is stored in the configuration files under [TeamCity Data Directory](teamcity-data-directory.md).
{product="tcc"}

The build log value is hidden with a simple search-and-replace algorithm, so if you have a trivial password of "123", all occurrences of "123" will be replaced, potentially exposing the password. Setting the parameter to the _password_ type does not guarantee that the raw value cannot be retrieved. Any project administrator can retrieve it, and any developer who can change the build script could potentially write malicious code to get the password.

Note that if you switch an existing parameter from the _password_ type to any other type, the original password value will be lost.

</td></tr></table>

Depending on the specification's type, there are additional settings.

<table>

<tr><td width="150"></td><td></td></tr>

<tr>

<td>

Text

</td>

<td>

__Allowed value__: choose the allowed text value. For the _Regex_ option, specify _Pattern_, a Java-style regular expression to validate the field value, as well as a _validation message_.

</td></tr><tr>

<td>

Checkbox

</td>

<td>

__Checked value/Unchecked value__: Specify values of the parameter respective to these checkbox states.

We recommend that you keep the unchecked value equal to the default value and specify a custom checked value.

Depending on how the build is triggered, the checkbox behavior will be as follows:
* if the build is triggered by an automatic trigger or by clicking __Run__ (without the _[Run Custom Build](running-custom-build.md)_ dialog), the default value is used;
* if the build is triggered via the _Run Custom Build_ dialog without changing anything, the "unchecked" value is used, if it is not empty and differs from the default one; if empty, the default value is used;
* if the build is triggered via the _Run Custom Build_ dialog with the box checked, the "checked" value is used, if it is not empty; if it is empty, the `true` value is used.

</td></tr><tr>

<td>

Select

</td>

<td>

Check the __Allow multiple__ box to enable multiple selection. In the __Items__ field, specify a newline-separated list of items. Use the following syntax `label => value` or `value`.

</td></tr></table>

## Manually Configuring Parameter Specification

Alternatively, you can manually configure a specification using a specially formatted string with the syntax similar to the one used in service messages (`typeName key='value'`).   
For example, for the text label: `text label='some label' regex='some pattern'`.

## Copying Parameter Specification

If you start editing a parameter that has a specification, you can see a link to its raw value in the _Edit parameter_ dialog. Click it to view the specification in its raw form (in the service message format). To use this specification in another build configuration, just copy it from here, and paste in another configuration.

## Modifying Parameter Specification via REST API

You can also view/edit typed parameters specification via [REST API](https://www.jetbrains.com/help/teamcity/rest/manage-typed-parameters.html).

 <seealso>
        <category ref="admin-guide">
            <a href="configuring-build-parameters.md">Configuring Build Parameters</a>
            <a href="configuring-build-parameters.md">Defining and Using Build Parameters in Build Configuration</a>
            <a href="predefined-build-parameters.md">Predefined Build Parameters</a>
        </category>
</seealso>