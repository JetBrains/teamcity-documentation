[//]: # (title: Eclipse Plugin)
[//]: # (auxiliary-id: Eclipse Plugin)

## Plugin Features

TeamCity integration with Eclipse provides the following features:
* [Remote Run](remote-run.md) and [Pre-Tested (Delayed) Commit](pre-tested-delayed-commit.md) for Subversion, Perforce, CVS, and Git
* customizing parameters for personal builds
* monitoring the projects status in the IDE
* exploring changes introduced in the source code and comparing the local version with the latest version in the project repository
* navigating from build logs opened in Eclipse to files referenced in the build log
* viewing failed tests of a particular build
* navigating to the TeamCity web interface
* starting investigation of a build failure
* viewing server\-provided code coverage results run on TeamCity using the IDEA or EMMA code coverage engine: `<Main Menu>/TeamCity/Code Coverage Data...`
* comparing personal patch content with workspace resources
* viewing compilation errors
* downloading a patch to IDE from the TeamCity server
* shelving changes
* re\-running tests failed on the TeamCity agent locally,
* support for P4Eclipse up to 2015.1 and Eclipse EGit 2.0\+

## Installing the Plugin

The TeamCity Eclipse plugin version must correspond to the version of the TeamCity server it connects to. Connections to TeamCity servers with different versions are generally not supported.

* __[Subversive](http://www.eclipse.org/subversive/) or [Subclipse](http://subclipse.tigris.org/) plugins__: to enable [Remote Run](remote-run.md) and [Pre-tested Commit](pre-tested-delayed-commit.md) for the Subversion Version Control System.   
Quick links: Subversive [download page](http://www.eclipse.org/subversive/downloads.php). Subclipse [installation instructions](https://github.com/subclipse/subclipse/wiki).
* __[P4Eclipse](http://www.perforce.com/product/components/eclipse_plugin) plugin__: to enable [Remote Run](remote-run.md) and [Pre-tested Commit](pre-tested-delayed-commit.md) for the Perforce Version Control System. Make sure you initialize Perforce support (for example, perform project update) after opening the project before using TeamCity Remote Run.
* __[CVS plugin for Eclipse](http://www.eclipse.org/eclipse/platform-cvs)__ to enable Remote Run and Pre\-tested Commit for CVS.
* __[EGit plugin for Eclipse](http://www.eclipse.org/egit/)__ to support Remote Run and Pre\-tested Commit for Git version control.
* __JDK 1.6\-1.8 (JDK 1.8 is recommended)__: Eclipse must be run under JDK 1.6\-1.8 for the TeamCity plugin to work.


[//]: # (Internal note. Do not delete. "Eclipse Plugind131e119.txt")    

__To install the TeamCity plugin for Eclipse:__
1. In the top right corner of the TeamCity web UI, click the arrow next to your username, and select __My Settings &amp; Tools__.
2. Locate the __TeamCity Tools__ section on the right.
3. Under the __Eclipse plugin__ header, copy the __update site link__ URL. For example, in Internet Explorer you can right\-click the link and choose __Copy shortcut__ from the context menu.
4. In Eclipse, click __Help | Install New Software...__ on the main menu. The __Install__ dialog appears.
5. Enter the URL copied above (`http://<your TeamCity Server address>/update/eclipse/`) into the URL field of the new update site in Eclipse, and click __Enter__.
6. Select the required features of the TeamCity Eclipse Plugin.        ![Eclipse.png](Eclipse.png)
7. Click __Next__ and follow the installation instructions.
For detailed instructions on how to work with the plugin, refer to the TeamCity section of the Eclipse help system after the plugin installation.

__  __

__See also:__

__Troubleshooting__: [Logging in TeamCity Eclipse plugin](reporting-issues.md)

__ __