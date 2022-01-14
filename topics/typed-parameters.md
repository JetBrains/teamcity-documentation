[//]: # (title: Changing Build Parameter Type and UI Appearance)
[//]: # (auxiliary-id: Changing Build Parameter Type and UI Appearance;Typed Parameters)

When adding a [build parameter](configuring-build-parameters.md), you can extend its definition with meta-information, or _specification_. The parameter's specification defines how its controls are presented and validated in the _[Run Custom Build](running-custom-build.md)_ dialog.

By adding a typed specification to a parameter, you make it a _typed parameter_. Typed parameters are more understandable for non-developers and make experience with builds more user-friendly.

Example: A build configuration has a parameter that defines if a build has to include a license or not. The parameter can be either `true` or `false` (default). For a regular user, it might not be clear which build parameter is responsible for the license generation and what its values mean. It is not a problem when builds run with the default configuration. However, if a user wants to run a [custom build](running-custom-build.md), the variety of available parameters might confuse them. Using the build parameter's specification, you can make the parameters' appearance in the _Run Custom Build_ dialog more readable.

<img src="typed-parameter-in-custom-run-dialog.png" width="700" alt="Typed parameter in a Running Custom Build dialog"/>

## Adding Parameter Specification

To add a specification to a build parameter, click __Edit__ in the __Spec__ section when [editing/adding a build parameter](configuring-build-parameters.md).

All parameters' specifications support the following common properties:


<table>
<tr><td>Setting</td><td>Description</td></tr>

<tr><td>

Label

</td><td>

An alternative text that can be displayed near the control in the _Run Custom Build_ dialog.

</td></tr>

<tr><td>

Description

</td><td>

A text that is displayed below the control. Might contain an explanation on how to use the control.

</td></tr>

<tr><td>

Display

</td><td>

A display mode:

* _Hidden_: the parameter will not be displayed in the _Run Custom Build_ dialog, but will be sent to a build.
* _Prompt_: TeamCity will always require a review of the parameter's value when clicking __Run__ (except if the build is triggered automatically).
* _Normal_: the parameter will be displayed as usual, on the __Parameters__ tab of the _Run Custom Build_ dialog.

</td></tr>

<tr><td>

Read-only

</td><td>

If enabled, it will be impossible to override the parameter's value.

</td></tr>

<tr><td>

Type

</td><td>

A UI field type:
* [Text](#Text+Type)
* [Checkbox](#Checkbox+Type)
* [Select](#Select+Type)
* [Password](#Password+Type)

</td></tr>

</table>

## Types of Parameters

### Text Type

This is the default option which is a usual text string.

For this type, it is possible to define the allowed value:
* _Any_
* _Not empty_
* _Regex_: specify a [Java-style regular expression](https://www.w3schools.com/java/java_regex.asp) to validate the field value, as well as a message to show if the validation fails.

### Checkbox Type

A true/false option represented by a checkbox.

For this type, it is possible to specify the values of the parameter that will be applied depending on the checkbox state (true or false).

We recommend that you keep the _unchecked value_ equal to the default value and specify a custom _checked value_.

Depending on how the build is triggered, the checkbox behavior will be as follows:
* If the build is triggered by an automatic trigger or by clicking __Run__ (without the _[Run Custom Build](running-custom-build.md)_ dialog), the default value is used.
* If the build is triggered via the _Run Custom Build_ dialog without changing anything, the "unchecked" value is used, if it is not empty and differs from the default one. If empty, the default value is used.
* If the build is triggered via the _Run Custom Build_ dialog with the box checked, the "checked" value is used, if it is not empty. If it is empty, the `true` value is used.

### Select Type

A "select one" or "select many" control to set the value to one of the predefined settings.

To allow selection of multiple options, enable the _Allow multiple_ option. In the _Items_ field, specify a newline-separated list of items of the selector menu. Use the following syntax `label => value` or `value`.

### Password Type

This type is designed to store passwords or other secure data in TeamCity settings. The value of the password parameter never appears readable in the TeamCity UI, including the settings' screens and the _Run Custom Build_ dialog. The value is also replaced in the build's __Parameters__ tab and build log.

The password value is stored in the configuration files under [TeamCity Data Directory](teamcity-data-directory.md). Depending on the server [encryption settings](teamcity-configuration-and-maintenance.md#encryption-settings), the value is either scrambled or encrypted with a custom key.
{product="tc"}

The password value is stored in the configuration files under [TeamCity Data Directory](teamcity-data-directory.md).
{product="tcc"}

>The value is hidden in the build log by a plain search-and-replace algorithm. If you have a trivial password of "123", all occurrences of the "123" pattern will be replaced, which could potentially expose the password.  
> Setting the parameter to the _Password_ type does not guarantee that the raw value cannot be retrieved. Any project administrator can retrieve it, and any developer who can change the build script could potentially write malicious code to get the password.
> 
{type="warning"}

Note that if you switch an existing parameter from the _Password_ type to any other type, the original password value will be lost.

## Manually Configuring Parameter Specification

You can also configure a specification manually — using a specially formatted string with the syntax similar to the one used in [service messages](service-messages.md) (`typeName key='value'`).   
For example, for the text label, use `text label='some label' regex='some pattern'`.

## Copying Parameter Specification

If you start editing a parameter that has a specification, you can see a link to its raw value in the _Edit parameter_ dialog. Click it to view the specification in its raw form (in the [service message](service-messages.md) format). To use this specification in another build configuration, just copy it from here, and paste it in another configuration.

## Modifying Parameter Specification via REST API

You can also view/edit typed parameters specification via [REST API](https://www.jetbrains.com/help/teamcity/rest/manage-typed-parameters.html).

 <seealso>
        <category ref="admin-guide">
            <a href="configuring-build-parameters.md">Configuring Build Parameters</a>
            <a href="using-build-parameters.md">Using Build Parameters</a>
            <a href="predefined-build-parameters.md">List of Predefined Build Parameters</a>
        </category>
</seealso>