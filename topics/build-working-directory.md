[//]: # (title: Build Working Directory)
[//]: # (auxiliary-id: Build Working Directory)

The _build working directory_ is the directory set as current for the build process. By default, this is the same directory as the [build checkout directory](build-checkout-directory.md).

If a build script needs to run from a location other than the checkout directory, you can specify the location explicitly using the __Working Directory__ field of the _Advanced options_ in the build step settings.

>Not all build runners provide the working directory setting.
>
{style="note"}

The path entered in the __Working Directory__ field can be either absolute or relative to the [build checkout directory](build-checkout-directory.md). When using this option, all the other paths should still be entered relative to the checkout directory.

The path to the build working directory is available as the `teamcity.build.workingDir` [predefined parameter](predefined-build-parameters.md).

<seealso>
        <category ref="concepts">
            <a href="build-checkout-directory.md">Build Checkout Directory</a>
        </category>
</seealso>