[//]: # (title: File Content Replacer)
[//]: # (auxiliary-id: File Content Replacer)

_File Content Replacer_ is the [build feature](adding-build-features.md) which processes text files by performing regular expression replacements before a build. After the build, it restores the file content to the original state.

<tip>

_File Content Replacer_ should be used with the [automatic checkout](vcs-checkout-mode.md) only: after this build feature is configured, it will run __before the first build step__. TeamCity will first perform replacement in the specified files found in the build checkout directory and then run your build. 
</tip>
 

The common case of using File Content Replacer is replacing one attribute at a time in particular files, for example, it can be used to patch files with the build number.   

You can add more than one File Content Replacer build feature if you wish to:
* replace more than one attribute
* replace one and the same attribute in different files/projects with different values   

This feature extends the capabilities provided by [AssemblyInfo Patcher](assemblyinfo-patcher.md).

Check the [Adding Build Features](adding-build-features.md) section for notes on how to add a build feature.


## File Content Replacer Settings

You can specify the values manually or use value presets for replacement, which can be edited if needed.

<table><tr>

<td width="250">

Option


</td>

<td>

Description


</td></tr><tr>

<td>

Template (optional)


</td>

<td>

<anchor name="Template"/>

File Content Replacer provides a template for every attribute to be replaced. Clicking the __Load Template__ button displays the combobox with templates containing value presets for replacement. The templates can be filtered by _language_ (e.g. `C#`), _file_ (e.g. `AssemblyInfo`) or _attribute_ (e.g. `AssemblyVersion`) by typing in the combobox. When a template is selected, the settings are automatically filled with predefined values. See the [section below](#Templates) for template details.


</td></tr><tr>

<td>

Process files


</td>

<td>

<anchor name="Wildcards"/>

Click __Edit file list__ and specify paths to files where the values to be replaced will be searched. Provide a newline- or comma-separated set of rules in the form of `+|-:[path relative to the checkout directory]`.   
[Ant-like wildcards](wildcards.md#Antlike+Wildcards) are supported, for example, `dir/**/*.cs`.

<include src="branch-filter.md" include-id="OR-syntax-tip"/>

_If a [pre-defined template](file-content-replacer.md#Templates) is selected, the files associated with that template will be used._


</td></tr><tr>

<td>

Fail build if no files match pattern

</td>

<td>

Enabled by default. Disable this option to prevent build failure even if no files match the specified pattern.

</td>


</tr>


<tr>

<td>

File encoding


</td>

<td>

<anchor name="Fileencoding"/>


By default, TeamCity will auto\-detect the file encoding. To specify the encoding explicitly, select it from the drop\-down. When specifying a _custom_ encoding, make sure it is [supported](https://docs.oracle.com/javase/8/docs/technotes/guides/intl/encoding.doc.html) by the agent.     

_If a [pre-defined template](file-content-replacer.md#Templates) is selected, the file encoding associated with that template will be used._


</td></tr><tr>

<td>

Find what


</td>

<td>

 <anchor name="Pattern"/>

Specify a pattern to search for, in the [regular expression](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Pattern.html#sum) format.    
The [MULTILINE](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Pattern.html#MULTILINE) mode is on by default.       
_If a [pre-defined template](file-content-replacer.md#Templates) is selected, the pattern associated with that template will be used._

You can disable the MULTILINE mode by adding "(?\-m)" at the start of the pattern string.


</td></tr><tr>

<td>

Match case


</td>

<td>

<anchor name="matchCase"/>

By default, the comparison is case\-sensitive. Uncheck for case\-insensitive languages.    
_If a [pre-defined template](file-content-replacer.md#Templates) is selected, the comparison associated with that template will be used._


</td></tr><tr>

<td>

Regex mode

</td>

<td>

<anchor name="Regex"/>

The box is checked by default and applies to both the search and replacement strings. Uncheck to use fixed strings.

<anchor name="RegexMixed"/>

<note>

If you use [versioned settings](storing-project-settings-in-version-control.md) (_XML_ or _Kotlin DSL_), in addition to the default `REGEX` and non\-default `FIXED_STRINGS` mode, the `REGEX_MIXED` mode is available. In this mode, the search pattern is interpreted as a regular expression, but the replacement text will be [quoted](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Matcher.html#quoteReplacement-java.lang.String-) (the `\`and `$` characters have no special meaning).

See a sample _File Content Replacer_ configuration for settings in [Kotlin](kotlin-dsl.md):


```Shell
features {
    replaceContent {
        fileRules = "**/*"
        pattern = "(?iu)the\h+pattern\h+to\h+search\h+for"
        regexMode = FileContentReplacer.RegexMode.REGEX_MIXED
        replacement = """%teamcity.agent.work.dir%\nd_r\bin\isf"""
    }
}
```

</note>

</td></tr><tr>

<td>

Replace with

</td>

<td>

 <anchor name="Replacement"/>

Type the text to be used for replacing the characters in the __Find what__ box. To delete the characters in the __Find what__ box from your file, leave this box blank.

$N sequence references N\-th capturing group. All backslashes (`\`) and dollar signs (`$`) without a special meaning should be quoted (as `\\` and `\$`, respectively).

</td></tr></table>

<anchor name="Pre-defined templates"/>

### Templates

This section lists the available replacement templates.

#### .NET templates

The templates for replacing the following [Assembly attributes](https://msdn.microsoft.com/en-us/library/4w8c1y2s(v=vs.110).aspx?cs-save-lang=1&amp;cs-lang=csharp#code-snippet-2) are provided (see [this section](file-content-replacer.md#Templates) for comparison with [AssemblyInfo Patcher](assemblyinfo-patcher.md)):

* [`AssemblyTitle`](http://msdn.microsoft.com/en-us/library/system.reflection.assemblytitleattribute.aspx)
* [`AssemblyDescription`](http://msdn.microsoft.com/en-us/library/system.reflection.assemblydescriptionattribute.aspx)
* [`AssemblyConfiguration`](http://msdn.microsoft.com/en-us/library/system.reflection.assemblyconfigurationattribute.aspx)
* [`AssemblyCompany`](http://msdn.microsoft.com/en-us/library/system.reflection.assemblyproductattribute.aspx)
* [`AssemblyProduct`](http://msdn.microsoft.com/en-us/library/system.reflection.assemblyproductattribute.aspx)
* [`AssemblyCopyright`](http://msdn.microsoft.com/en-us/library/system.reflection.assemblycopyrightattribute.aspx)
* [`AssemblyTrademark`](http://msdn.microsoft.com/en-us/library/system.reflection.assemblytrademarkattribute.aspx)
* [`AssemblyCulture`](http://msdn.microsoft.com/en-us/library/system.reflection.assemblycultureattribute.aspx)
* [`AssemblyVersion`](http://msdn.microsoft.com/en-us/library/system.reflection.assemblyversionattribute.aspx)
* [`AssemblyFileVersion`](http://msdn.microsoft.com/en-us/library/system.reflection.assemblyfileversionattribute.aspx)
* [`AssemblyInformationalVersion`](http://msdn.microsoft.com/en-us/library/system.reflection.assemblyinformationalversionattribute.aspx)
* [`AssemblyKeyFile`](http://msdn.microsoft.com/en-us/library/system.reflection.assemblykeyfileattribute.aspx)
* [`AssemblyKeyName`](http://msdn.microsoft.com/en-us/library/system.reflection.assemblykeynameattribute.aspx)
* [`AssemblyDelaySign`](http://msdn.microsoft.com/en-us/library/system.reflection.assemblydelaysignattribute.aspx)
* [`CLSCompliant`](http://msdn.microsoft.com/en-us/library/system.clscompliantattribute.aspx)
* [`ComVisible`](http://msdn.microsoft.com/en-us/library/system.runtime.interopservices.comvisibleattribute.aspx)
* [`Guid`](http://msdn.microsoft.com/en-us/library/system.runtime.interopservices.guidattribute.aspx)
* [`InternalsVisibleTo`](http://msdn.microsoft.com/en-us/library/system.runtime.compilerservices.internalsvisibletoattribute.aspx)
* [`AllowPartiallyTrustedCallers`](http://msdn.microsoft.com/en-us/library/system.security.allowpartiallytrustedcallersattribute.aspx)
* [`NeutralResourcesLanguageAttribute`](http://msdn.microsoft.com/en-us/library/system.resources.neutralresourceslanguageattribute.aspx)

#### .Net Core [csproj](https://docs.microsoft.com/en-us/dotnet/articles/core/tools/csproj) templates

* `AssemblyName`
* `AssemblyTitle`
* `AssemblyVersion`
* `Authors`
* `Company`
* `Copyright`
* `Description`
* `FileVersion`
* `PackageId`
* `PackageVersion`
* `Product`
* `Title`
* `Version`
* `VersionPrefix`
* `VersionSuffix`

#### MFC templates

The templates for replacing the following __MFC C\+\+ resource keys__ are provided:
* `FileDescription`
* `CompanyName`
* `ProductName`
* `LegalCopyright`
* `FileVersion*`
* `ProductVersion*`

<note>

In __MFC\*.rc__ files, both `FileVersion` and `ProductVersion` occur twice, once in a _dot\-separated_ (e.g. `1.2.3.4`) and once in a _comma\-separated_ (e.g. `1,2,3,4`) form. If your `%build.number%` parameter has a format of `1.2.3.{0}`, using two build parameters with different delimiters instead of a single `%build.number%` is recommended.
</note>

#### Xcode templates

The templates for replacing the following [Core Foundation Keys](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html) in `Info.plist` files are provided:
* `CFBundleVersion`
* `CFBundleShortVersionString`
* or both `CFBundleVersion` and `CFBundleShortVersionString` at the same time

## Examples

### Extending an attribute value with a custom suffix

Suppose you do not want to replace your `AssemblyConfiguration` with a fixed literal, but want to preserve your `AssemblyConfiguration` from `AssemblyInfo.cs` and just extend it with a custom suffix, for example,: `[assembly: AssemblyConfiguration("${AssemblyConfiguration} built by TeamCity")])`.

Do the following: change the default replacement `$1MyAssemblyConfiguration$7` to `$1$5 built by TeamCity$7`.

[//]: # (Internal note. Do not delete. "File Content Replacerd143e622.txt")    


For changing complex regex patterns, [this external tool](https://regex101.com/) might be useful.

### Patching only specified files

The default `AssemblyInfo` templates follow the usual _Visual Studio_ project/solution layout; but a lot of information may be shared across multiple projects and can reside in a shared file (e.g. `CommonAssemblyInfo.cs`).

Suppose you want to patch this shared file only; or you want to patch `AssemblyInfo.cs` files on a per\-project bassis.

Do the following:
1. Load the `AssemblyInfo` template corresponding to the attribute you are trying to process (e.g. `AssemblyVersion`)
2. Change the list of file paths in the [Look in](#Wildcards) field from the default `*/Properties/AssemblyInfo.cs` to `*/CommonAssemblyInfo.cs ` or list several files comma\- or new\-line separated files here, e.g. `myproject1/Properties/AssemblyInfo.cs, myproject2/Properties/AssemblyInfo.cs` .

### Specifying path patterns which contain spaces

Spaces are usually considered a part of the pattern, unless they follow a comma, as the comma is recognised as a delimiter.

Note that the TeamCity server UI trims leading and trailing spaces in input fields, so a single\-line pattern like `<spaces>foo.bar` will become `foo.bar` upon save. The following workarounds are available:

[//]: # (Internal note. Do not delete. "File Content Replacerd143e694.txt")    

### Changing only the last version part / build number of the AssemblyVersion attribute:

Suppose, your `AssemblyVersion` in `AssemblyInfo.cs` is `Major.Minor.Revision.Build` (set to `1.2.3.*`), and you want to replace the `Build` portion (following the last dot (the `*`) only.

Load the `AssemblyVersion` in AssemblyInfo (C#) template and change the default pattern:

```Shell
(^\s*\[\s*assembly\s*:\s*((System\s*\.)?\s*Reflection\s*\.)?\s*AssemblyVersion(Attribute)?\s*\(\s*@?\")(([0-9\*])+\.?)+(\"\s*\)\s*\])

```

to

```Shell

(^\s*\[\s*assembly\s*:\s*((System\s*\.)?\s*Reflection\s*\.)?\s*AssemblyVersion(Attribute)?\s*\(\s*@?\")(([0-9\*]+\.)+)[0-9\*]+(\"\s*\)\s*\])

```

and change the default replacement:

```Shell

$1\%build.number%$7

```

to

```Shell
$1$5\%build.number%$7

```


<note>

Make sure your `%build.number%` [format](configuring-general-settings.md) contains just a decimal number without any dots, or you may end up with a non-standard version like `1.2.3.4.5.6.2600` (i.e. `%build.counter%` _is_ a valid value here while `4.5.6.%build.counter%` is _not_).
</note>

__ __