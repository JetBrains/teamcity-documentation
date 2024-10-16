[//]: # (title: Importing Arbitrary Coverage Results to TeamCity)
[//]: # (auxiliary-id: Importing Arbitrary Coverage Results to TeamCity)

TeamCity comes bundled with the following coverage engines: [IntelliJ IDEA, Emma, JaCoCo for Java](configuring-java-code-coverage.md) and [dotCover, NCover, PartCover for .NET](configuring-.net-code-coverage.md). If you use these platforms, TeamCity will provide a code coverage automatically.

It is possible to achieve a similar experience with tools that are not supported out of the box. There are two options:
* __Publish a coverage HTML report as a TeamCity [build artifact](build-artifact.md)__.  
  Most of the tools produce coverage reports in HTML — you can publish such a report as an artifact and configure the __[Report](including-third-party-reports-in-the-build-results.md)__ tab to display it in TeamCity. The coverage should be published to an `index.html` file inside the `coverage.zip` archive and put in the artifacts root directory. In this case, the __Report__ tab will appear automatically.
* __Extract and publish statistics__  
  You can extract coverage statistics from a coverage report and publish [statistical values](custom-chart.md#Default+Statistics+Values+Provided+by+TeamCity) to TeamCity using [service messages](service-messages.md#Reporting+Build+Statistics). In this case, the coverage chart will be displayed on the build configuration __Statistics__ tab.  
  This approach also allows failing a build based on [build failure condition](build-failure-conditions.md) on a metric change (for example, you can fail build if the coverage drops).

>Avoid publishing values like `CodeCoverageB`, `CodeCoverageL`, `CodeCoverageM`, `CodeCoverageC` standing for a block/line/method/class coverage percentage. TeamCity will calculate these values using their absolute parts. For example, `CodeCoverageL` will be calculated as `CodeCoverageAbsLCovered` divided by `CodeCoverageAbsLTotal`. Such values will be published without their decimal parts, which might make them useless.
> 
{style="warning"}

See also [how to integrate TeamCity with an arbitrary test reporting tool](how-to.md#Integrate+with+Build+and+Reporting+Tools).
