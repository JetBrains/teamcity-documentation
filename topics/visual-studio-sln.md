[//]: # (title: Visual Studio (sln))
[//]: # (auxiliary-id: Visual Studio (sln))
This page contains reference information for the Visual Studio (sln) Build Runner that builds Microsoft Visual Studio 2005\-2017, and __since TeamCity 2019.1__, Microsoft Visual Studio 2019 solution files. To build Microsoft Visual Studio 2003 solution files, use the [Visual Studio 2003](visual-studio-2003.md) runner.

<note>

The Visual Studio (sln) build runner requires the proper version of Microsoft Visual Studio installed on the build agent.
</note>

### General Build Runner Options

<table><tr>

<td>

Option


</td>

<td>

Description


</td></tr><tr>

<td>

Solution file path


</td>

<td>

Specify the path to the solution to be built relative to the [Build Checkout Directory](build-checkout-directory.md). For example:


```Shell
vs-addin\addin\addin.sln

```

</td></tr><tr>

<td>

Working directory


</td>

<td>

Specify the [Build Working Directory](build-working-directory.md). (optional)


</td></tr><tr>

<td>

Visual Studio


</td>

<td>

Select the Visual Studio version.


</td></tr><tr>

<td>

Targets


</td>

<td>

Specify the Microsoft Visual Studio targets specific for the previously selected Visual Studio version. The possible options are __Build__, __Rebuild__, __Clean__, __Publish__ or a combination of these targets based on your needs. Multiple targets are space\-separated.


</td></tr><tr>

<td>

Configuration


</td>

<td>

Specify the name of Microsoft Visual Studio solution configuration to build (optional).


</td></tr><tr>

<td>

Platform


</td>

<td>

Specify the platform for the solution. You can leave this field blank, and TeamCity will obtain this information from the solution settings (optional).


</td></tr><tr>

<td>

Command line parameters


</td>

<td>

Specify additional command line parameters to be passed to the build runner. Instead of explicitly specifying these parameters, it is recommended to define them on the [Configuring Build Parameters](configuring-build-parameters.md) page.


</td></tr></table>

__  __

__See also:__

__Troubleshooting__: [Visual Studio logging](reporting-issues.md)
