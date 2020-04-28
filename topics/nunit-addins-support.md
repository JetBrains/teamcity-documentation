[//]: # (title: NUnit Addins Support)
[//]: # (auxiliary-id: NUnit Addins Support)

NUnit addin is an extension that plugs into NUnit core and changes the way it operates. Refer to the [NUnit addins page](http://www.nunit.org/index.php?p=nunitAddins&amp;r=2.6.3) for more information. This section covers description of NUnit addins support for:

## NAnt Build Runner

To support NUnit addins for NAnt build runner you need to provide in your build script the `teamcity.dotnet.nant.nunit2.addins` property in the following format:

```XML
<property name="teamcity.dotnet.nant.nunit2.addins" value="<list of paths>" />

```

where `<list>` is the list of paths to NUnit addins separated by `;`.

For example:

```XML
<property name="teamcity.dotnet.nant.nunit2.addins" value="../tools/addins/MyFirst.AddIn.dll;MySecond.AddIn.dll" />

```

## TeamCity NUnit Console Launcher

To support NUnit addins for the [console launcher](teamcity-nunit-test-launcher.md) you need to provide the `/addins:<list of addins separated with ;>` command line option.

For example:

```XML
${teamcity.dotnet.nunitlauncher} /addin:../tools/addins/MyFirst.AddIn.dll;nunit-addins/MySecond.AddIn.dll

```

## MSBuild

This section is __applicable to NUnit versions prior to 3.0__.

To support NUnit addins for the MSBuild runner, specify the `Addins` property for the `NUnitTeamCity` task with the following format:

```XML
Addins="<list>"

```

where `<list>` is the list of addins separated by `;` or `,`.

For example:

```XML
<Project xmlns="http://schemas.microsoft.com/developer/msbuild/2003" DefaultTargets="build">
  <ItemGroup>
    <TestAssembly Include="$(MSBuildProjectDirectory)/MyTests.dll" />
  </ItemGroup>
  <Target Name="build">
    <NUnitTeamCity Assemblies="@(TestAssembly)" Addins="../tools/addins/MyFirst.AddIn.dll;nunit-addins/MySecond.AddIn.dll" />
  </Target>
</Project>

```

__ __