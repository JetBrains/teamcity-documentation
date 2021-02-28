[//]: # (title: Bugzilla)
[//]: # (auxiliary-id: Bugzilla)

## Converting Strings into Links to Issues

When [enabling issue tracker integration](integrating-teamcity-with-issue-tracker.md#Enabling+Issue+Tracker+Integration) in addition to general settings, you need to specify which strings are to be recognized as references to issues in your tracker.   
For Bugzilla, you need to specify the __Issue Id Pattern__: a [Java Regular Expression](http://java.sun.com/j2se/1.5.0/docs/api/java/util/regex/Pattern.html) pattern to find the issue ID in the text. The matched text (or the first group if there are groups defined) is used as the issue number.  The most common case seems to be `#(\d+)` - this will extract 1234 as issue ID from text "Fix for #1234".

## Requirements

If the username and password are specified, you need to have Bugzilla XML-RPC interface switched on. This is not required if you use anonymous access to Bugzilla without the username and password.

## Known Issues

There are several known issues in Bugzilla regarding XMLs generated for the issues, which makes it hard to communicate with it. However, this can usually be fixed by tweaking the Bugzilla configuration.
	
* If you see the _path/to/bugzilla.dtd not found_ error, this means that the issue XML contains the relative path to the `bugzilla.dtd` file, and not the URL. To fix that, set the server URL in Bugzilla.
* Sometimes you may see a `SAXParseException` saying that _Open quote is expected for attribute type_id associated with an element type flag_. This happens because the generated XML does not correspond to the bundled `bugzilla.dtd`. To fix it, make the `type_id` attribute `#IMPLIED` (optional) in the bugzilla.dtd file. The issue and the workaround are described in detail [here](http://jake.murzy.com/post/2661770569/errors-while-performing-validation-against-bugzilla-dtd).

 <seealso>
        <category ref="concepts">
            <a href="supported-platforms-and-environments.md">Supported Platforms and Environments</a>
        </category>
        <category ref="admin-guide">
            <a href="integrating-teamcity-with-issue-tracker.md">Integrating TeamCity with Issue Tracker</a>
        </category>
</seealso>