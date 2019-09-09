[//]: # (title: Command Line)
[//]: # (auxiliary-id: Command Line)
Using this build runner, you can run any script supported by the OS.

On this page:

<tag-list of="chapter" mode="tree" depth="4"/>

## Command Line Runner Settings

### General Settings

<table><tr>

<td>

Option


</td>

<td>

Description


</td></tr><tr>

<td>

Working directory


</td>

<td>

Specify the [Build Working Directory](build-working-directory.md) if it differs from the [build checkout directory](build-checkout-directory.md).


</td></tr><tr>

<td>

Run


</td>

<td>

Select whether you want to run an executable with parameters or custom shell/batch scripts.


</td></tr><tr>

<td>

Command executable


</td>

<td>

_The option is available if "Executable with parameters" is selected in the __Run__ dropdown._ Specify the executable file of the build runner


</td></tr><tr>

<td>

Command parameters


</td>

<td>

_The option is available if "Executable with parameters" is selected in the __Run__ dropdown._ Specify parameters as a space\-separated list.


</td></tr><tr>

<td>

(__Since TeamCity 2017.2__)

Format stderr output as:

</td>

<td>

Specify how the error output is handled by the runner:

* __error__: any output to `stderr` is handled as an error
* __warning__: default; any output to `stderr` is handled as a warning



</td></tr><tr>

<td>

Custom script


</td>

<td>

A platform\-specific script which will be executed as a `*.cmd` file on Windows or as an executable script in Unix\-like environments. _The option is available if "Custom script" is selected in the __Run__ dropdown_.

<tip>

TeamCity treats a string surrounded by percentage signs (`%`) in the script as a [parameter reference](predefined-build-parameters.md). To prevent TeamCity from treating the text in the percentage signs as a property reference, use double percentage signs to escape them: for example, if you want to pass "`%Y%m%d%H%M%S`" into the build, change it to "`%%Y%%m%%d%%H%%M%%S`".
</tip>


</td></tr></table>

### Docker Settings

In this section, you can specify a Docker image which will be [used to run this build step](docker-wrapper.md).

### Code Coverage

To learn about configuring code coverage options, refer to the [Configuring Java Code Coverage](configuring-java-code-coverage.md) page.



__  __

__See also:__


__Concepts__: [Build Runner](build-runner.md) | [Build Checkout Directory](build-checkout-directory.md) | [Build Working Directory](build-working-directory.md)   
__Administrator's Guide__: [Configuring Build Steps](configuring-build-steps.md)
