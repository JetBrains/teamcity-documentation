[//]: # (title: NUnit for NAnt Build Runner)
[//]: # (auxiliary-id: NUnit for NAnt Build Runner)

<tip>

Supported NUnit versions: __2.2.10__, __2.4.1__, __2.4.6__, __2.4.7__, __2.4.8__, __2.5.0__, __2.5.2__, __2.5.3__, __2.5.4__, __2.5.5__, __2.5.6__, __2.5.7__, __2.5.8__, __2.5.9__, __2.5.10__, __2.6.0__, __2.6.1__, __2.6.2__, __2.6.3__.
</tip>

This section assumes, that you already have a NAnt build script with the configured `nunit2` task in it, and want TeamCity to track test reports without making any changes to the existing build script. Otherwise, consider adding [NUnit build runner](nunit.md) as one of the steps for your build configuration.

To track tests defined in a NAnt build via the standard `nunt2` task, TeamCity provides a custom [task](http://nant.sourceforge.net/nightly/latest/help/tasks/nunit2.html) implementation and automatically replaces the original `<nunit2>` task with its own task. Thus when the build is triggered, TeamCity starts TeamCity NUnit Test Launcher using own implementation of `<nunit2>`. This allows you to leave your build script without changes and receive on-the-fly test reports in TeamCity.

If you don't want TeamCity to replace the original `nunit2` task, consider the following options:
* Use NUnit console with TeamCity Addin for NUnit.
* Import XML tests results via the XML Test Report plugin.
* Use command-line TeamCity NUnit Test Launcher.
* Configure reporting tests manually via service messages.
* To disable `nunit2` task replacement, set `teamcity.dotnet.nant.replaceTasks` [system property](configuring-build-parameters.md) to `false`.

The `nunt2` task implementation in TeamCity supports additional options that can be specified either as NAnt `<property>` tasks in the build script, or as __System Properties__ under __Build Configuration | Build Parameters__.

The following options are supported for the TeamCity `<nunit2>` task implementation:

<table><tr>

<td>

Property

</td>

<td>

Description

</td></tr><tr>

<td>

`teamcity.dotnet.nant.nunit2.failonfailureatend`

</td>

<td>

Run __all tests__ regardless of the number of failed ones, and fails if at least one test has failed.

</td></tr><tr>

<td>

`teamcity.dotnet.nant.nunit2.platform`

</td>

<td>

Sets desired runtime execution mode for .NET 2.0 on x64 machines. Supported values are __x86__, __x64__, and __ANY__(default).

</td></tr><tr>

<td>

`teamcity.dotnet.nant.nunit2.platformVersion`


</td>

<td>

Sets desired .NET Framework version. Supported values are __v1.1__, __v2.0__, __v4.0__. Default value is equal to NAnt target framework

</td></tr><tr>

<td>

`teamcity.dotnet.nant.nunit2.version`


</td>

<td>

Specifies which version of the NUnit runner to use. The value has to be specified in the following format: `NUnit-<version>`.

It is possible to have several versions of NUnit installed on an agent machine and use any of them in a build.

</td></tr><tr>

<td>

`teamcity.dotnet.nant.nunit2.addins`

</td>

<td>

Specifies the list of third-party NUnit addins used for NAnt build runner.

</td></tr><tr>

<td>

`teamcity.dotnet.nant.nunit2.runProcessPerAssembly`

</td>

<td>

Set __true__ if you want to run each assembly in a new process.

</td></tr></table>

TeamCity NUnit test launcher will run tests in the .NET Framework, which is specified by NAnt target framework, i.e. on .NET Framework 1.1, 2.0 or 4.0 runtime. TeamCity also supports test categories for `<nunit2>` task.

<note>

Adding the listed properties to the NAnt build script makes it TeamCity dependent. To avoid this, specify properties as __System Properties__ under __Build Configuration__, or consider adding `<if>` task.
</note>

<tip>

If you need TeamCity test runner to support third-party NUnit addins, refer to the [NUnit Addins Support](nunit-addins-support.md) section for the details.
</tip>

### Examples

Start tests form a single assembly files under x64 mode on .NET 2.0.

```XML
<property name="teamcity.dotnet.nant.nunit2.platform" value="x64" />
<nunit2>
    <formatter type="Plain" />
    <test assemblyname="MyProject.Tests.dll" />
</nunit2>

```

Run all tests from category C1, but not C2.

```XML
<nunit2 verbose="true" haltonfailure="false" failonerror="true">
         <formatter type="Plain" />
          <test>
            <assemblies>
              <include name="dll.dll" />
            </assemblies>
            <categories>
               <include name="C1" />
               <exclude name="C2"/>
            </categories>
          </test>
        </nunit2>

```

Explicitly specify the version on NUnit to run tests with.   
Note, that in this case, the following property should be added __before__ the `nunit2` task call.

```XML
<property name="teamcity.dotnet.nant.nunit2.version" value="NUnit-2.4.10" />
<nunit2> <!--....--> </nunit2>

```