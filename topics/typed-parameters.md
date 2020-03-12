[//]: # (title: Typed Parameters)
[//]: # (auxiliary-id: Typed Parameters)

When adding a [build parameter](configuring-build-parameters.md) (system property, environment variable or configuration parameter), you can extend its definition with a specification that will regulate parameter's control presentation and validation. This specification is the parameter's "meta" information that is used to display the parameter in the [Triggering a Custom Build](triggering-a-custom-build.md) dialog. It allows making a custom build run more user\-friendly and usable by non\-developers. Consider a simple example. You have a build configuration in which you have a monstrous\-looking build parameter that regulates if a build has to include a license or not; can be either true or false; and by default is false. It may be clear for a build engineer, which build parameter regulates license generation and which value it is to have, but it may not be obvious to a regular user.

Using the build parameter's specification you can make your parameters more readable in the __Run Custom Build__ dialog.

## Adding Parameter Specification

To add specification to a build parameter, click the __Edit__ button in the __Spec__ area when editing/adding a build parameter.

All parameters specifications support a number of common properties, such as:
* __Label__: some text that is shown near the control in the Run Custom Build dialog.
* __Description__: some text that is shown below the control containing an explanatory note of the control use.
* __Display__: If _hidden_ is specified, the parameter will not be shown in the __Run Custom Build__ dialog, but will be sent to a build; if _prompt_ is specified, TeamCity will always require a review of the parameter value when clicking the __Run__ button (won't require the parameter if build is triggered automatically); if _normal_ is selected, the parameter will be shown as usual.
* __Read\-only__: if the box if checked, it will be impossible to override the parameter with a different value  
* __Type__: Currently you can present parameters in following forms:
   * a simple text field with the ability to validate its value using regular expression;
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

The default. Represents a usual text string without any extra handling


</td></tr><tr>

<td>

Checkbox


</td>

<td>

True/false option represented by a checkbox


</td></tr><tr>

<td>

Select


</td>

<td>

"Select one" or "select many" control to set the value to one of predefined settings


</td></tr><tr>

<td>

Password


</td>

<td>

 This is designed to store passwords or other secure data in TeamCity settings. TeamCity makes the value of the password parameter never appear in the TeamCity web UI: it affects the settings screens and the __Run Custom Build__ dialog where password fields appear. Also, the value is replaced in the build's __Parameters__ tab and build log. The value is stored scrambled in the configuration files under TeamCity Data Directory. Please note that build log value hiding is implemented with simple search\-and\-replace, so if you have a trivial password of "123", all occurrences of "123" will be replaced, potentially exposing the password. Setting the parameter to type password does not guarantee that the raw value cannot be retrieved. Any project administrator can retrieve it and also any developer who can change the build script can in theory write malicious code to get the password.


</td></tr></table>

Depending on the specification's type, there are additional settings.

<table>

<tr><td width="150"></td><td></td></tr>

<tr>

<td>

Text


</td>

<td>

__Allowed value__ \- choose the allowed value. For the __Regex__ option, specify __Pattern__, a Java\-style regular expression to validate the field value, as well as a __validation message__.


</td></tr><tr>

<td>

Checkbox


</td>

<td>

__Checked value__/__Unchecked value__: Specify values for the parameter to have depending on the checkbox's state.

It is recommended to __specify the checked value__ and keep the default value and the unchecked value of the parameter __the same__.

Depending on the way the build is triggered, the checkbox behavior will be as follows:
* if the build is triggered by an automatic trigger or by clicking the __Run__ button (without the [run custom build](triggering-a-custom-build.md) dialog), the default value is used
* if the build is triggered via the [run custom build](triggering-a-custom-build.md) dialog without changing anything, the 'unchecked value' is used (if not empty); and if it is empty, the default value is used
* if the build is triggered via the [run custom build](triggering-a-custom-build.md) dialog with the box checked, the 'checked value' is used (if not empty); and if it is empty, the 'true' value is used


</td></tr><tr>

<td>

Select


</td>

<td>

Check the __Allow multiple__ box to enable multiple selection. In the __Items__ field specify a newline\-separated list of items. Use the following syntax `label => value` or `value`.


</td></tr></table>

## Manually Configuring Parameter Specification

Alternatively, you can manually configure a specification using a specially formatted string with the syntax similar to the one used in service messages (`typeName key='value'`).   
For example, for text: `text label='some label' regex='some pattern'`.

## Copying Parameter Specification

If you start editing a parameter that has a specification, you can see a link to its raw value in the "Edit parameter" dialog. Click it to view the specification in its raw form (in the service message format). To use this specification in another build configuration, just copy it from here, and paste in another configuration.

## Modifying Parameter Specification via REST API

You can also view/edit typed parameters specification via [REST API](rest-api.md#Typed+Parameters+Specification).

 __  __

__See also:__

__Administrator's Guide__: [Configuring Build Parameters](configuring-build-parameters.md) | [Defining and Using Build Parameters in Build Configuration](configuring-build-parameters.md) | [Predefined Build Parameters](predefined-build-parameters.md)

__ __