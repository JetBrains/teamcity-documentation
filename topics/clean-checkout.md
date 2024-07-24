[//]: # (title: Clean Checkout)
[//]: # (auxiliary-id: Clean Checkout)

_Clean Checkout_ (also referred to as _Clean Sources_) is an operation that ensures that the next build will get a copy of the sources fetched all over from the VCS. All the content of the [Build Checkout Directory](build-checkout-directory.md) is deleted, and the sources are refetched from the version control.

## Enforcing Clean Checkout

Clean checkout is recommended if the checkout directory content was modified by an external process by adding new, modifying or deleting existing files.

You can enforce cleaning the sources by doing the following:

* for a build configuration — from the __Build Configuration Home__ page, open the __Actions__ drop-down menu and choose **Enforce clean checkout...**.
* for an agent — from the __[Agent Details](viewing-build-agent-details.md)__ page, click the _Clean sources on this agent_ link under the __Miscellaneous__ section.

The action opens a list of agents/build configurations to clean sources for.

The _Clean Sources_ is a single action that, after triggered, is performed only once during the next build run of each selected configuration on each selected agent.

<note>

If you set a specific directory as the Build Checkout Directory (instead of using the default one), remember that all the content of this directory will be deleted during the clean checkout procedure.

</note>

TeamCity maintains an internal cache for the sources to optimize communications with the VCS server. The caches are reset during the [clean-up](teamcity-data-clean-up.md). To resolve problems with sources update, the caches may need to be reset manually using the __[Diagnostics | Caches](teamcity-monitoring-and-diagnostics.md#Caches)__ tab in the UI or by deleting the `<[TeamCity Data Directory](teamcity-data-directory.md)>/system/caches` directory.
{product="tc"}

TeamCity maintains an internal cache for the sources to optimize communications with the VCS server. The caches are reset during the [clean-up](teamcity-data-clean-up.md).
{product="tcc"}

## Automatic Clean Checkout

You can also enable automatic cleaning the sources before every build, if you check the option __Clean all files before build__ on the __[Create/Edit Build Configuration](creating-and-editing-build-configurations.md)&gt; [Version Control Settings](configuring-vcs-settings.md)__ page. If this option is checked, TeamCity performs a full checkout before each build. If clean checkout is not enabled, TeamCity updates the sources in the checkout directory incrementally to the required state. 

TeamCity tries to detect if the sources in the checkout directory are not corresponding to the expected state and triggers clean checkout in such cases to ensure sources are appropriate. This means that under certain circumstances TeamCity can detect clean checkout is necessary even if it is not enabled in the VCS settings and not requested by the user from web UI. In such cases, all the content of the checkout directory is deleted and it is repopulated by the sources from scratch. If any details are available on the decision, they are added into the build log before checkout-related logging.

Here is the summary of cases when TeamCity performs automatic clean checkout:
* if it is enabled using the __Clean all files in the checkout directory before the build__ option in the "Version Control Settings" of the build configuration
* build checkout directory was not found or is empty (either the build configuration is started on the agent for the first time or the directory has disappeared since the last build). This also covers the following: 
  * no builds were run in a specific checkout directory for a configured (or default) time and the directory became empty. See more at [automatic checkout directory cleaning](build-checkout-directory.md#Automatic+Checkout+Directory+Cleaning)
  * there was not enough [free space on disk](free-disk-space.md) in one of the earlier builds and the directory was deleted
* a user invoked "_Enforce clean checkout_" action from the web UI for a build configuration or agent
* the build was triggered via Custom Run Build dialog with "Clean all files in the checkout directory before build" option selected or by a trigger with the corresponding option
* the build was triggered by a Schedule trigger with "Clean all files in checkout directory before build" option enabled or as a part of a build chain where the topmost build was triggered with the setting in the schedule trigger while "apply to all snapshot dependencies" was also selected
* VCS settings of the build configuration were changed
* the previous build in this directory was of a build configuration with different VCS settings (can only occur if the same checkout directory is specified for several build configurations with individual VCS settings and VCS Roots)
* the previous build in this directory was built on more recent revisions than the current one (can only occur for [history builds](history-build.md))
* there was a critical error while applying or rolling back a patch during the previous build, so TeamCity cannot ensure that checkout directory contains known versions of files
* [Build Files Cleaner (Swabra)](build-files-cleaner-swabra.md) is enabled with corresponding options and it detected that clean checkout is necessary
* Custom checkout directory contains agent-specific parameters, such as `%\teamcity.agent.work.dir%` (pre-8.1)
