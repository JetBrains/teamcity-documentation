[//]: # (title: Mapping External Links in Comments)
[//]: # (auxiliary-id: Mapping External Links in Comments)

TeamCity allows mapping patterns in VCS change comments to arbitrary HTML pieces using regular expression search and replace patterns. One of the most common usages is to map an issue ID mentioning into a hyperlink to the issue page in the issue tracking system.

To configure mapping:

1. Navigate to the file [`<TeamCity Data Directory>`](teamcity-data-directory.md)`\config\main-config.xml`.
2. Locate section `<comment-transformation>`, or create one under the `<server>` tag, if it doesn't exist (you may refer to the `main-config.dtd` file for the XML structure definition).
3. Specify the search and replace patterns. For example, you can use the following pattern for enabling JIRA integration:

```XML

<server>
...
   <comment-transformation>
     <transformation-pattern
       search="(>|\(|\s|^)([A-Z]+-\d+)(\b|$)"
       replace="$1&lt;a target=&quot;_blank&quot; title=&quot;Click to open this issue a new window&quot; href=&quot;
       https://www.jetbrains.net/jira/browse/$2&quot;&gt;$2&lt;/a&gt;$3"
       description="JetBrains Jira issue link" />
   </comment-transformation>
...
</server>

```

TeamCity can apply several patterns to a single piece of text, if they do not intersect (match different string segments).

<note>

Search &amp; replace patterns have [`java.util.regex.Pattern`](https://java.sun.com/j2se/1.5.0/docs/api/java/util/regex/Pattern.html) syntax.
</note>
