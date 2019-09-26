[//]: # (title: Visual C Build Issues)
[//]: # (auxiliary-id: Visual C Build Issues)
If you experience any problems running Visual C\+\+ build on a build agent, you can try to workaround these issues with the following steps, sequentially:


<note>

Any of these steps may solve your issue. Feel free to leave feedback of you experience.
</note>


	
* Make sure you do not use mapped network drives.
	
* Make sure build user have enough right to access necessary network paths
	
* Log on to the build agent machine under the same user as for build and try running the following command:

    ```Shell
    
    msbuild.exe <path to solution.sln> /p:Configuration:Release /t:Rebuild
    
    ```

* Build Agent service runs under the user with local administrative privileges
	
* Make sure Microsoft Visual Studio is installed on the build agent
	
* You have to start Visual Studio 2005 or Visual Studio 2008 under build user [once](http://www.jetbrains.net/devnet/message/5233781#5233781)
	
* If __Error spawning cmd.exe__ appears, you should put the [following lines](http://www.jetbrains.net/devnet/message/5217957#5217957) exactly into the list in `Tools -> Options -> Projects and Solutions -> VC++ Directories`:

    ```Shell
    --$(SystemRoot)\System32
    --$(SystemRoot)
    --$(SystemRoot)\System32\wbem

    ```
	
* You need to add all environment variables from `...\Microsoft Visual Studio 9.0\VC\vcvarsall.bat` to environment or to [Build Agent Configuration](build-agent-configuration.md)
	
* Try using __devenv.exe__ with [Command Line](command-line.md) instead of [Visual Studio (sln)](visual-studio-sln.md) build runner
	
* Ensure all paths to sources do not contain spaces
	
* Set `VCBuildUserEnvironment=true` in the runner properties
	
* Specify the `VCBuildAdditionalOptions` property with value `/useenv` in the build configuration settings to instruct msbuild to add the `/useenv` commandline argument for spawned vcbuild processes.

__ __
 
 __See also:__


__Administrator's Guide__: [.NET Testing Frameworks Support](net-testing-frameworks-support.md) | [NUnit support](nunit-support.md)

__ __