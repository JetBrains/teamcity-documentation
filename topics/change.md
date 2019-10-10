[//]: # (title: Change)
[//]: # (auxiliary-id: Change)
Any modification of the source code which you introduce. If a change has been committed to the version control system, but not yet included in a build, it is considered pending for a certain build configuration.

TeamCity suggests several ways to view changes:
* The [Changes](viewing-your-changes.md) page shows the list of changes made by TeamCity users and how they have affected different builds. By default, your own changes are displayed. The page has a user selector enabling you to view changes made by any other TeamCity user the same way you see your own changes.
* Pending changes are accessible from the __Projects__ page, build configuration page, or the [build results](working-with-build-results.md) page.

Viewing and analyzing changes involves the following possibilities:
* Observing actual changes that are already included in the build using the __Changes__ link on the __Projects__ page or on the __Changes__ tab of the [build results](working-with-build-results.md#Changes).
* Observing pending changes on the __Pending Changes__ tab of the Build Configuration Home Page.
* Viewing the [revision](revision.md) of the sources corresponding to each change.
* Modifying the change comment in TeamCity, available to project administrators by default (we recommend changing the comment in the VCS as well, for consistency).
* Navigating to the related issues in a bug tracking system.
* Navigating to the source code and viewing differences.
* [Starting an investigation](investigating-and-muting-build-failures.md) of a failed build, if your changes have caused a build failure.
 
__  __

__See also:__

__Concepts__: [Revision](revision.md) | [Build Configuration](build-configuration.md)   
__User's Guide__: [Investigating and Muting Build Failures](investigating-and-muting-build-failures.md)

__ __