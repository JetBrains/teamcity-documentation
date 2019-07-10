[//]: # (title: .NET Process Runner)
[//]: # (auxiliary-id: .NET Process Runner)
The _.NET Process Runner_ is able to run any .NET assembly under the selected .NET Framework version and platform, optionally with .NET code coverage. You can use it to run xUnit, Gallio or other .NET tests, for which there is no dedicated build runner. 

On this page:

<tag-list of="chapter" mode="tree" depth="4"/>


<note>

The runner requires .NET Framework installed on the TeamCity Agent.
</note>
 


## .NET Process Runner Settings


<table>
<tr>


<td>

Option 


</td>


<td>

Description 


</td>
</tr>
<tr>


<td>

Path


</td>


<td>

Specify the path to a .NET executable (for example, to the xUnit console)


</td>
</tr>
<tr>


<td>

Command line parameters


</td>


<td>

Provide newline\- or space\-separated command line parameters to be passed to the executable.


</td>
</tr>
<tr>


<td>

Working directory


</td>


<td>

Specify the [Build Working Directory](build-working-directory.md) if it differs from the [Build Checkout Directory](build-checkout-directory.md).


</td>
</tr>
<tr>


<td>

.NET Runtime 


</td>


<td>

From the __Platform__ drop\-down select the desired execution mode on a x64 machine. The supported values are: Auto (MSIL) (default), x86 and x64.  
From the __Version__ drop\-down select the desired .NET Framework version.


<tip>

If you have an MSIL .NET 2.0/3.5 executable, TeamCity can enforce it to execute under any required runtime: x86 or x64, and .NET 2.0 or .NET 4.0 runtime.
</tip>


</td>
</tr>
</table>




## Code Coverage


If needed, [add code coverage](configuring-.net-code-coverage.md).



Note that you do not need to write any additional build scripts.



__  __

__See also:__

__Administrator's Guide__: [Configuring .NET Code Coverage](configuring-.net-code-coverage.md)
