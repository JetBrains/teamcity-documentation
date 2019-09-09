[//]: # (title: AssemblyInfo Patcher)
[//]: # (auxiliary-id: AssemblyInfo Patcher)
The _AssemblyInfo Patcher_ [build feature](adding-build-features.md) allows setting a build number to an assembly automatically, without having to patch the `AssemblyInfo.cs` files manually. When adding this build feature, you only need to specify the version format. Note that you can use TeamCity parameter references here.

 AssemblyInfo Patcher should be used with the [automatic checkout ](vcs-checkout-mode.md)only: after this build feature is configured, it will run __before the first build step__. TeamCity will first perform replacement in the files found in the build checkout directory and then run your build.

TeamCity searches for all AssemblyInfo (including GlobalAssemblyInfo) files: `.cs`, `.cpp`, `.fs`, `.vb` in their standard locations under the checkout directory and replaces the parameter for the `AssemblyVersion`, `AssemblyFileVersion` and `AssemblyInformationalVersion` attributes with the build number you have specified in the TeamCity web UI.

At the end of the build the files are reverted to the initial state.

Note that this feature will work only for standard projects, i.e. created by means of the Visual Studio wizard, so that all the `AssemblyInfo` files and content have a standard location.

<note>

For replacing a wider range of values in a larger number of files, consider using the [File Content Replacer](file-content-replacer.md) build feature.
</note>

