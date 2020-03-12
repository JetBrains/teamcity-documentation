[//]: # (title: Command Line)
[//]: # (auxiliary-id: Command Line)
Using this build runner, you can run any script supported by the OS.

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

Specify the [working directory](build-working-directory.md) where the command is to be run (if it differs from the [build checkout directory](build-checkout-directory.md)).


</td></tr><tr>

<td>

Run


</td>

<td>

Specify the mode: run an executable with parameters or run custom shell/batch script (see below).


</td></tr><tr>

<td>

Command executable


</td>

<td>

_The option is available if "Executable with parameters" is selected in the __Run__ dropdown._

Specify the path to an executable to be started.


</td></tr><tr>

<td>

Command parameters


</td>

<td>

_The option is available if "Executable with parameters" is selected in the __Run__ dropdown._

Specify space\-separated parameters to pass to the executable. If a parameter contains a space, it can be enclosed in double quotes. For non-trivial parameters it is recommended to use "Custom script" option instead.


</td></tr><tr>

<td>

Custom script


</td>

<td>
_The option is available if "Custom script" is selected in the __Run__ dropdown._

A platform\-specific script which will be executed as an executable script in Unix\-like environments and as a `*.cmd` batch file on Windows. Under Unix\-like OS the script is saved with the executable bit set and is then executed by OS. This defaults to /bin/sh interpreter on the most systems. If you need a specific interpreter to be used, specify shebang (e.g. "#!/bin/bash") as the first line of the script.
</td></tr><tr>

<td>

Format stderr output as:

</td>

<td>

Specify how the error output is handled by the runner:

* __error__: any output to `stderr` is handled as an error
* __warning__: default; any output to `stderr` is handled as a warning



</td></tr></table>

<tip>

TeamCity treats a string surrounded by percentage signs (`%`) in the script as a [parameter reference](predefined-build-parameters.md). To prevent TeamCity from treating the text in the percentage signs as a property reference, use double percentage signs to escape them: for example, if you want to pass "`%Y%m%d%H%M%S`" into the build, change it to "`%%Y%%m%%d%%H%%M%%S`".
</tip>


### Docker Settings

In this section, you can specify a Docker image which will be [used to run this build step](docker-wrapper.md).

### Code Coverage

To learn about configuring code coverage options, refer to the [Configuring Java Code Coverage](configuring-java-code-coverage.md) page.



__  __

__See also:__


__Concepts__: [Build Runner](build-runner.md) | [Build Checkout Directory](build-checkout-directory.md) | [Build Working Directory](build-working-directory.md)   
__Administrator's Guide__: [Configuring Build Steps](configuring-build-steps.md)

__ __
