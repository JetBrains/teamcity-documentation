[//]: # (title: MSpec)
[//]: # (auxiliary-id: MSpec)
_The MSpec Test Runner_ is designed specifically to run [MSpec](https://github.com/machine/machine.specifications) tests. 

<note>

To run tests using MSpec, you need to install it on at least one build agent.
</note>




## MSpec Settings


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

Path to MSpec.exe 


</td>


<td>

A path to `mspec.exe` file.  


</td>
</tr>
<tr>


<td>

.NET Runtime 


</td>


<td>

From the __Platform__ drop\-down, select the desired execution mode on a x64 machine. Supported values are: Auto (MSIL) (default), x86 and x64. From the __Version__ drop\-down, select the desired .NET Framework version.


<tip>

If you have MSpec as an MSIL .NET 2.0/3.5 executable, TeamCity can enforce it to execute under any required runtime: x86 or x64, and .NET2.0 or .NET 4.0 runtime. 
</tip>


</td>
</tr>
<tr>


<td>

Run tests from 


</td>


<td>

Specify the .NET assemblies where the MSpec tests to be run are stored. 


</td>
</tr>
<tr>


<td>

Do not run tests from 


</td>


<td>

Specify the .NET assemblies that should excluded from the list of found assemblies to test.


</td>
</tr>
<tr>


<td>

Include specifications 


</td>


<td>

Specify comma\- or newline separated list of specifications to be executed. 

</td>
</tr>
<tr>


<td>

Exclude specifications 


</td>


<td>

Specify comma\- or new line separated list of specifications to be excluded. 


</td>
</tr>
<tr>


<td>

Additional commandline parameters 


</td>


<td>

Enter additional commandline parameters for `mspec.exe`.


</td>
</tr>
</table>


## Code Coverage

Learn about [Configuring .NET Code Coverage](configuring-.net-code-coverage.md).

__ __

__See also:__

__Administrator's Guide__: [Configuring .NET Code Coverage](configuring-.net-code-coverage.md)

__ __