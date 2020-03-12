[//]: # (title: PowerShell)
[//]: # (auxiliary-id: PowerShell)

The _PowerShell_ [build runner](build-runner.md) is specifically designed to run PowerShell scripts. 

The plugin responsible for PowerShell integration has been open-sourced [on GitHub](https://github.com/JetBrains/teamcity-powershell).

<tip>

If you need to run a PowerShell script with elevated permissions, consider using the TeamCity [RunAs plugin](https://github.com/JetBrains/teamcity-runas-plugin).
</tip>

## Cross-Platform PowerShell

* Cross-platform PowerShell ([PowerShell Core](https://blogs.msdn.microsoft.com/powershell/2018/01/10/powershell-core-6-0-generally-available-ga-and-supported/)) is supported on Windows, MacOS, and Linux: [download a PowerShell package](https://github.com/PowerShell/PowerShell#get-powershell) for your platform and install it on the TeamCity agent.
* Side-by-side installation of PowerShell Desktop and PowerShell Core is supported under Windows.

## Detection of Installed PowerShell on Build Agents

During startup, a TeamCity agent searches for the PowerShell installation in standard locations, such as `Program Files` and `Windows` directories. You can specify a custom location in the `teamcity.powershell.detector.search.paths` [agent property](predefined-build-parameters.md#Agent+Properties), so the agent can detect PowerShell in this directory (and its children) as well.   
To list multiple locations, separate their paths with `;`.

## PowerShell Settings

<table><tr>

<td>

Option


</td>

<td>

Description


</td></tr><tr>

<td>

Version

</td>



<td>

List of PowerShell versions supported by TeamCity. It is passed to `powershell.exe` as the `-Version` command line argument.


</td></tr><tr>

<td>

PowerShell run mode


</td>


<td>

Select the desired execution mode on a x64 machine:

__Version__

Specify a version (for example, 1.0 or 2.0). It will be compared to the version installed on the agent, and an appropriate requirement will be added. For Core editions, it will be used as the lower bound. On Desktop editions, the exact version will be used (`-Version` command line argument will be added).

If the version field is left blank, no lower bound on the version requirement will be added, no `-Version` argument will be used on Desktop editions of PowerShell.

__Platform__

Select platform bitness:

* `x64` \- 64\-bit,  default, the corresponding requirement will be added
* `x86` \- 32\-bit, the corresponding requirement will be added
* `Auto` \- when it is selected, no platform requirement will be added to the build configuration, and if both 32\-bit and 64\-bit PowerShells are installed, 64\-bit will be preferred.

__Edition__

Select a PowerShell edition to be used: (__since TeamCity 2017.1__)

* Desktop \- closed\-source edition bundled with Windows, available only on Windows platforms. 
* Core \- open\-source edition based on .NET Core, cross\-platform, 64\-bit only


</td></tr><tr>

<td>

Format stderr output as:


</td>

<td>

Specify how the error output is handled by the runner:

* __error__: any output to `stderr` is handled as an error
* __warning__: default; any output to `stderr` is handled as a warning

<tip>

To fail a build if "an error message is logged by build runner" (see [Build Failure Conditions](build-failure-conditions.md)), change the default setting of the Error Output selector from __warning__ to __error__.
</tip>



</td></tr><tr>

<td>

Working directory


</td>


<td>

Specify the path to the [build working directory](build-working-directory.md).


</td></tr><tr>

<td>

Script


</td>

<td>

Select whether you want to enter the script right in TeamCity, or specify a path to the script:

* __File__: Enter the path to a PowerShell file. The path has to be relative to the checkout directory.
* __Source__: Enter the PowerShell script source. Note that TeamCity [parameter references](configuring-build-parameters.md#Using+Build+Parameters+in+Build+Configuration+Settings) will be replaced in the code.

<note>

There is an issue with PowerShell 2.0 not returning the correct exit code when the script contains explicitly defined parameters. As a [workaround](https://youtrack.jetbrains.com/issue/TW-42996), either upgrade your PowerShell or to use `[Environment]::Exit(code)`.
</note>



</td></tr><tr>

<td>

Script execution mode


</td>

<td>

Specify the PowerShell script execution mode. By default, PowerShell may not allow execution of arbitrary `.ps1` files. TeamCity will try to supply the `-ExecutionPolicy ByPass` argument. If you've selected __Execute .ps1 script from external file__, your script should be signed or you should make PowerShell allow execution of arbitrary `.ps1` files.  If the execution policy doesn't allow running your scripts, select __Put script into PowerShell stdin__ mode to avoid this issue.

The `-Command` mode is deprecated and is not recommended for use with PowerShell of version greater than 1.0


</td></tr><tr>

<td>

Script arguments


</td>


<td>

_Available if "Script execution mode" option is set to "Execute .ps1 script from external file"._

Specify build parameters to be passed as arguments into the PowerShell script.   
When using named arguments, follow this pattern: `-<script_argument1> %build_parameter1% -<script_argument2> %build_parameter2%`.

<note>

__Escaping special symbols__

TeamCity calls `powershell.exe` from the console of your operating system (command prompt on Windows, bash or other on Linux).

If parameters containing special symbols are passed to your PowerShell script in double quotes, make sure these characters are properly escaped: use the escape rules depending on your interpreter, for example, on Windows, if a PowerShell script argument ends with a backslash, use the backslash as the escape symbol for TeamCity to correctly interpret it: `"foo\\"`.
</note>


</td></tr><tr>

<td>

Additional command line parameters


</td>

<td>

Specify parameters to be passed to `powershell.exe`.


</td></tr></table>

## Docker Settings

To enable support for Docker in PowerShell steps, run the TeamCity server with the `-Dteamcity.docker.runners=jetbrains_powershell` [internal property](configuring-teamcity-server-startup-properties.md#TeamCity+internal+properties).

In this section, you can specify a Docker image which will be [used to run the build step](docker-wrapper.md). 

### Current Limitations

* Execution under Docker requires the PowerShell executable to be added to PATH
* When using Docker to run the build step, only Docker\-related build agent requirements are applied to the build
* Selection of Edition in PowerShell build step affects the executable being used (powershell.exe for Desktop, pwsh for Core)
* &lt;Auto&gt; defaults to `pwsh` (Core)
* To specify a custom PowerShell executable, the `teamcity.powershell.virtual.executable` configuration parameter must be set to the full path of this executable inside the provided image
* Current limitations of the Docker wrapper do not allow Linux containers running under Windows systems

## Interaction with TeamCity

Attention must be paid when using PowerShell to interact with TeamCity through service messages. PowerShell tends to wrap strings written to the console with commands like `Write-Output`, `Write-Error` and similar (see [TW-15080](http://youtrack.jetbrains.com/issue/TW-15080)). To avoid this behavior, either use the `Write-Host` command, or adjust the buffer length manually:


```Shell
function Set-PSConsole {
  if (Test-Path env:TEAMCITY_VERSION) {
    try {
      $rawUI = (Get-Host).UI.RawUI
      $m = $rawUI.MaxPhysicalWindowSize.Width
      $rawUI.BufferSize = New-Object Management.Automation.Host.Size ([Math]::max($m, 500), $rawUI.BufferSize.Height)
      $rawUI.WindowSize = New-Object Management.Automation.Host.Size ($m, $rawUI.WindowSize.Height)
    } catch {}
  }
}

```



## Error Handling

Due to [this issue](http://connect.microsoft.com/PowerShell/feedback/details/777375/powershell-exe-does-not-set-an-exit-code-when-file-is-used) in PowerShell itself that causes zero exit code to be always returned to a caller, TeamCity cannot always detect whether the script has executed correctly or not. We recommend several approaches that can help in detecting script execution failures:

* _Manually catching exceptions and explicitly returning exit code_   
The PowerShell plugin does not use the cmd wrapper around `powershell.exe`. It makes returning the explicit exit code possible.

    
    ```Shell
    try {
    # your code here
    } Catch {
    $ErrorMessage = $_.Exception.Message
    Write-Output $ErrorMessage
    exit(1)
    }
    ```


* _Setting  to  and adding a build failure condition_:   
In case syntax errors and exceptions are present, PowerShell writes them to `stderr`. To make TeamCity fail the build, set __Error Output__ option to `Error` and add a [build failure condition](build-failure-conditions.md#Common+build+failure+conditions) that will fail the build on any error output.
* _Failing build on certain message in build log_:   
Add a [build failure condition](build-failure-conditions.md#Common+build+failure+conditions) that will fail the build on a certain message (say "POWERSHELL ERROR") in the build log.


    ```Shell
    $ErrorMessage = "POWERSHELL ERROR"
    try {
      # your code here
    } Catch {
      Write-Output $ErrorMessage
      exit(1)
    }
    ```



## Handling Output

To properly handle non\-ASCII output from PowerShell, the correct encoding must be set both on the PowerShell side and on the TeamCity side.
* To set the output encoding for PowerShell to UTF\-8, add the following line to the beginning of your PowerShell script:


    ```Shell
    [Console]::OutputEncoding = [System.Text.Encoding]::UTF8
    ```

* To set the encoding on the TeamCity agent side, either set the Java launch option `-Dfile.encoding=UTF-8`, or set the build [configuration parameter](configuring-build-parameters.md) `teamcity.runner.commandline.stdstreams.encoding` value to `UTF-8`.

## Temporary Files

The TeamCity PowerShell plugin uses temporary files as an entry point; these files are created in the build temporary folder and removed after the PowerShell build step is finished. To keep the files, set the `powershell.keep.generated` or `teamcity.dont.delete.temp.files` configuration parameter to `true`.

## Development Links

The PowerShell support is implemented as an open\-source plugin. For development links refer to the [plugin's page](https://plugins.jetbrains.com/plugin/9041-powershell-runner).


[//]: # (Internal note. Do not delete. "PowerShelld255e514.txt")    

__ __