[//]: # (title: Xcode Project)
[//]: # (auxiliary-id: Xcode Project)
The _Xcode Project Build Runner_ supports Xcode 3 (target\-based build), Xcode 4 (scheme\-based build), Xcode 5\-9.

The runner provides structured build log based on Xcode build stages, detects compilation errors, reports tests from the `xcodebuild` utility, adds automatic agent requirements for the appropriate version of tools installed (Xcode, SDKs, and so on) and reporting tools via agent properties.

<note>

To run an Xcode build, you need to have one or more build agents running Mac OS X with installed Xcode.
</note>

<anchor name="Xcode 7.x Support"/>

## Working with different Xcode setups:
*  If only one Xcode version is installed on the agent machine, it will be used by default. The agent restart is required if Xcode was installed/updated.
*  If several Xcode versions are installed, perform __one of the following__:
   * specify the path to the required version in the "Path to Xcode" setting (see below) of the Xcode Project build step settings
   * select the default XCode distribution using the `xcode-select` tool (details on [Apple.com](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man1/xcode-select.1.html)).   
   Path to Xcode: `/Applications/Xcode.app/Contents/Developer`.   
   Command to switch: `sudo xcode-select -s path_to_xcode_distribution`.

<tip>

Note that if you use `xcode-select`, the agent must be restarted, as it only detects Xcode distributions and reports them to the server during startup.
</tip>

 

__Xcode Project Runner Settings__

<table><tr>

<td width="250">

Setting


</td>

<td width="250">

Build


</td>

<td>

Description


</td></tr><tr>

<td>

Path to the project or workspace


</td>

<td>

 


</td>

<td>

The path to a (`.xcodeproj`) project file or a (`.xcworkspace`) workspace file, should be relative to the checkout directory. For Xcode 3 build, only the path to the project file is supported.


</td></tr><tr>

<td>

Working directory


</td>

<td>

 


</td>

<td>

Specify the [build working directory](build-working-directory.md).


</td></tr><tr>

<td>

Path to Xcode


</td>

<td>

 


</td>

<td>

Specify the path to Xcode on the agent. The build will be run using this Xcode.


</td></tr><tr>

<td>

Build


</td>

<td>

 


</td>

<td>

Select either a target\-based (for project) or scheme\-based (for project and workspace) build. Depending on the selection, the settings displayed will vary.


</td></tr><tr>

<td>

Scheme


</td>

<td>

Scheme\-based    


</td>

<td>

Xcode scheme to build. The list of available schemes is formed by parsing your project/workspace files in the VCS. Make sure your __Path to the project or workspace__ is set correctly and click the __Check/Reparse Project__ button to show/refresh the schemes list. Note that a scheme must be shared to be shown in the list (to check if your scheme is shared, verify that it is located under the `xcshareddata` folder and not under the `xcuserdata` one, and that the `xcshareddata` folder is committed to your VCS. To check the latter, use the VCS tree popup next to the __Path to the project or workspace__ field). More information on managing Xcode schemes is available in the [Apple documentation](http://developer.apple.com/library/ios/#recipes/xcode_help-scheme_editor/Articles/SchemeManage.html).


</td></tr><tr>

<td>

Build output directory


</td>

<td>

Scheme\-based


</td>

<td>

Check the __Use custom__ box to override the default path for the files produced by your build. Specify the custom path relative to the checkout directory.


</td></tr><tr>

<td>

Target


</td>

<td>

Target\-based


</td>

<td>

Xcode target to execute. The list of available targets is formed by parsing your project files in the VCS. Make sure your __Path to the project__ is set correctly and click the __Check/Reparse Project__ button to show/refresh the targets list.


</td></tr><tr>

<td>

Configuration


</td>

<td>

Target\-based


</td>

<td>

Xcode configuration. The list of available configurations is formed by parsing your project files in the VCS. Since the configuration depends on the target, you must choose the target first. Make sure your __Path to the project__ is set correctly and click the __Check/Reparse Project__ button to show/refresh the configurations list.


</td></tr><tr>

<td>

Platform


</td>

<td>

Target\-based


</td>

<td>

Select from the __default__, __iOS__, __Mac OS X__ or __Simulator \- iOS__ or any other platform (if it is provided by the agent) to build your project on.


</td></tr><tr>

<td>

SDK


</td>

<td>

Target\-based


</td>

<td>

You can choose a SDK to build your project with (the list of available SDKs is formed according to the SDKs available on your agents for the platform selected.


</td></tr><tr>

<td>

Architecture


</td>

<td>

Target\-based


</td>

<td>

You can choose an architecture to build your project with (the list of available architectures is formed according to the architectures available on your agents for the platform selected).


</td></tr><tr>

<td>

Build action(s)


</td>

<td>

 


</td>

<td>

Xcode build action(s). The default actions are `clean` and `build`. A space\-separated list of the following actions is supported: `clean`, `build`, `test`, `archive`, `installsrc`, `install`; the order of actions will be preserved during execution.

It is not recommended to change this field unless you are sure you want to change the number or order of actions.

<tip>

If your project is built under Xcode 5\+, checking the __Run test__ option below automatically adds the `test` build action to the list (unless the option is already explicitly specified in the current field).
</tip>


</td></tr><tr>

<td>

Run tests


</td>

<td>

 


</td>

<td>

Сheck this option if want to run tests after your project is built.


</td></tr><tr>

<td>

Additional command line parameters


</td>

<td>

 


</td>

<td>

Other command line parameters to be passed to the `xcodebuild` utility.


</td></tr></table>




__  __

__See also:__


__Concepts__: [Build Runner](build-runner.md)
