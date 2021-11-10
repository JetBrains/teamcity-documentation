[//]: # (title: TeamCity Addin for NUnit)
[//]: # (auxiliary-id: TeamCity Addin for NUnit)

<note>

TeamCity NUnit Addin supports __NUnit prior to version 3.0__. For later versions, refer to the [this section](nunit-for-msbuild.md).
</note>

If you run NUnit tests via the [NUnit console](http://www.nunit.org/index.php?p=nunit-console&amp;r=2.2.10) and want TeamCity to track the test results without having to launch the TeamCity test runner, the best solution is to use TeamCity Addin for NUnit. You can plug this addin into NUnit, and the tests will be automatically reported to the TeamCity server.

Alternatively, you can opt to use the [XML Report Processing](xml-report-processing.md) build feature, or manually configure reporting tests by means of [service messages](service-messages.md).

To be able to review test results in TeamCity, do the following:
1. In your build, set the path to the TeamCity Addin to the system property `teamcity.dotnet.nunitaddin` (for MSBuild it would be `teamcity_dotnet_nunitaddin`), and add the version of NUnit at the end of this path. For example:
   * For NUnit 2.4.X, use `${teamcity.dotnet.nunitaddin}-2.4.X.dll` (for MSBuild: `$(teamcity_dotnet_nunitaddin)-2.4.X.dll`)   
   Example for NUnit 2.4.7: `NAnt: ${teamcity.dotnet.nunitaddin}-2.4.7.dll`, MSBuild: `$(teamcity_dotnet_nunitaddin)-2.4.7.dll`
   * For NUnit 2.5.0 alpha 4, use `${teamcity.dotnet.nunitaddin}-2.5.0.dll` (for MSBuild: `$(teamcity_dotnet_nunitaddin)-2.5.0.dll`)
2. Copy the `.dll` and `.pdb` TeamCity addin files to the NUnit addin directory.

<note>

Although you can copy these files once, it is highly recommended to configure your builds so that the TeamCity addin files are copied to the NUnit addin directory for __each build__, because these files could be updated by TeamCity.
</note>

The following example shows how to use the NUnit console runner with the TeamCity Addin for NUnit 2.4.7 (on MSBuild):


```XML
<ItemGroup>
  <NUnitAddinFiles Include="$(teamcity_dotnet_nunitaddin)-2.4.7.*" />
</ItemGroup>
 
<Target Name="RunTests">
  <MakeDir Directories="$(NUnitHome)/bin/addins" />
  <Copy SourceFiles="@(NUnitAddinFiles)" DestinationFolder="$(NUnitHome)/bin/addins" />
  <Exec Command="$(NUnitHome)/bin/NUnit-Console.exe $(NUnitFileName)" />
</Target>

```



### Important Notes

__NUnit 2.4.8 Issue__

NUnit 2.4.8 has the following known issue: NUnit 2.4.8 runner tries to load an assembly according to the created `AssemblyName` object; however, the `addins` folder of NUnit 2.4.8 is not included in application probe paths. Thus NUnit 2.4.8 fails to load any addin in the console mode.   
To solve the problem, we suggest you use any of the following workarounds:
* copy the TeamCity addin assembly both to the NUnit `bin` and `bin/addins` folders
* patch `NUnit-Console.exe.config` to include the addins to application probe paths. Add the following code into the `config/runtime` element:


```XML
<assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">
      <probing privatePath="addins"/>
</assemblyBinding>

```



See original blog post on this issue [http://nunit.com/blogs/?p=56](http://nunit.com/blogs/?p=56)

__Environment variables__ If you need to configure environment variables for NUnit explicitly, specify an environment variable with the value reference of `%\system.teamcity.dotnet.nunitaddin%`. See [Configuring Build Parameters](configuring-build-parameters.md) for details.
