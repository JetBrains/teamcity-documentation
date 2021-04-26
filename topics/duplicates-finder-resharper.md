[//]: # (title: Duplicates Finder \(ReSharper\))
[//]: # (auxiliary-id: viewpage.actionpageId113084077;Duplicates Finder \(ReSharper\))

The _Duplicates finder (ReSharper)_ build runner, based on [ReSharper Command Line Tools](http://www.jetbrains.com/resharper/features/command-line.html), is intended to catch similar code fragments and provide a report on the discovered repetitive blocks of C# and Visual Basic .NET code in Visual Studio 2003, 2005, 2008, 2010, 2012, 2013, and 2015 solutions.

<note>

This runner requires .NET Framework 4.6.1 (or higher) to be installed on the agent where builds will run.
</note>

Refer to [Configuring Build Steps](configuring-build-steps.md) for a description of common build steps' settings. Refer to [Docker Wrapper](docker-wrapper.md) to learn how you can run this step inside a Docker container (in terms of TeamCity EAP 2021.1).

## Sources

<table><tr>

<td>

Option

</td>

<td>

Description

</td></tr><tr>

<td>

Include

</td>

<td>

Use newline-delimited Ant-like wildcards relative to the checkout root to specify the files to be included into the duplicates search.   
Visual Studio solution files are parsed and replaced by all source files from all projects within a solution.   
Example: `src\MySolution.sln`

</td></tr><tr>

<td>

Exclude

</td>

<td>

Enter newline-delimited Ant-like wildcards to exclude files from the duplicates search (for example, `*/generated{*}{}.cs`). The entries should be relative to the checkout root.

</td></tr></table>

## JetBrains ReSharper Command Line Tools Settings

<table><tr>

<td>

Option

</td>

<td>

Description

</td></tr><tr>

<td>

R# CLT Home Directory 

</td>

<td>

Select the ReSharper Command Line Tools version. You can check the installed JetBrains ReSharper Command Line Tools versions on the __[Administration | Tools](installing-agent-tools.md)__ page. If you want to run ReSharper duplicates using a specific ReSharper version (for example, to ensure it matches the version you have installed in Visual Studio), you can use this page to install another version of the tools and can change the default version to be used.

</td></tr>

<tr>

<td>

dupFinder Platform

</td>

<td id="ReSharperDupFinderPlatform" auxiliary-id="ReSharperDupFinderPlatform">

Select the platform bitness of the dupFinder tool. By default, x64. In terms of TeamCity 2021.1 EAP, the cross-platform duplicates finder is also supported in ReSharper 2020.2.1 or later.

</td></tr>

</table>

## Duplicate Searcher Settings

<table><tr>

<td>

Option

</td>

<td>

Description

</td></tr><tr>

<td id="fragComp">

Code fragments comparison

</td>

<td>

Use these options to define which elements of the source code should be discarded when searching for repetitive code fragments. Code fragments can be considered duplicated, if they are structurally similar, but contain different variables, fields, methods, types or literals. Refer to the samples below:

</td></tr><tr>

<td>

Discard namespaces

</td>

<td>

If this option is checked, similar contents with different _namespace specifications_ will be recognized as duplicates.

```Shell
NLog.Logger.GetInstance().Log("abcd");
A.Log.Logger.GetInstance().Log("abcd");

```

</td></tr><tr>

<td>

Discard literals

</td>

<td>

If this option is checked, similar lines of code with different literals will be recognized as duplicates.

```Shell
myStatusBar.SetText("Not Logged In");
myStatusBar.SetText("Logging In...");

```

</td></tr><tr>

<td>

Discard local variables

</td>

<td>

If this option is checked, similar code fragments with different local variable names will be recognized as duplicates.

```Shell
int a = 5; a += 6;
int b = 5; b += 6;

```

</td></tr><tr>

<td>

Discard class fields name

</td>

<td>

If this option is checked, the similar code fragments with different field names will be recognized as duplicates.

```Shell
Console.WriteLine(myFoo);
Console.WriteLine(myBar);
... where myFoo and myBar are declared in the containing class

```

</td></tr><tr>

<td>

Discard types

</td>

<td>

If this option is checked, similar content with different type names will be recognized as duplicates. These include all possible type references (as shown below):

```csharp
Logger.GetInstance("text");
OtherUtility.GetInstance("text");
... where Logger and OtherUtility are both type names (thus GetInstance is a static method in both classes)
 
Logger a = (Logger) GetObject("object");
OtherUtility a = (OtherUtility) GetObject("object");
 
public int SomeMethod(string param);
public void SomeMethod(object[] param);

```

</td></tr><tr>

<td>

Ignore duplicates with complexity lower than

</td>

<td>

Use this field to specify the lowest level of complexity of code blocks to be taken into consideration when detecting duplicates.

</td></tr><tr>

<td>

Skip files by opening comment

</td>

<td>

Enter newline-delimited keywords to exclude files that contain the keyword in the file's opening comments from the duplicates search.

</td></tr><tr>

<td>

Skip regions by message substring

</td>

<td>

Enter newline-delimited keywords that exclude regions that contain the keyword in the message substring from the duplicates search. Entering "generated code", for example, will skip regions containing "Windows Form Designer generated code".

</td></tr><tr>

<td id="debug">

Enable debug output

</td>

<td>

Check this option to include debug messages in the build log and publish the file with additional logs (`dotnet-tools-dupfinder.log`) as an artifact.

</td></tr><tr>

<td id="cmdArgs">

Additional dupFinder parameters

</td>

<td>

Specify newline-separated command line parameters to add to calling `dupFinder.exe`.

</td></tr>
</table>

### Build Failure Conditions

If a build has too many duplicates, you can configure it to fail by setting a [build failure condition](build-failure-conditions.md).