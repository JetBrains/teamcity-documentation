[//]: # (title: Code Inspection)
[//]: # (auxiliary-id: Code Inspection)

TeamCity comes with code analysis tools capable of inspecting your source code on the fly, finding and reporting common problems and anti-patterns.

The following inspections tools are bundled with TeamCity:
* [Inspections (IntelliJ IDEA)](inspections.md): runs code analysis based on [IntelliJ IDEA inspections](http://www.jetbrains.com/idea/documentation/inspections.jsp). More than 600 Java, HTML, CSS, JavaScript inspections are performed by this runner.
* [Inspections (ReSharper)](inspections-resharper.md): gathers results of [JetBrains ReSharper](http://www.jetbrains.com/resharper) [Code Analysis](http://www.jetbrains.com/resharper/webhelp/Code_Analysis__Index.html) in your C#, VB.NET, XAML, XML, ASP.NET, JavaScript, CSS, and HTML code.   
Inspection results are reported in the __[Code Inspection](working-with-build-results.md#Code+Inspection+Results)__ tab of the __Build Results__ page.

TeamCity can also be [integrated with external reporting tools](how-to.md#Integrate+with+Build+and+Reporting+Tools).

 <seealso>
        <category ref="concepts">
            <a href="build-runner.md">Build Runner</a>
        </category>
        <category ref="admin-guide">
            <a href="inspections.md">Inspections (IntelliJ IDEA)</a>
            <a href="inspections-resharper.md">Inspections (ReSharper)</a>
        </category>
</seealso>