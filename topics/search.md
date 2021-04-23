[//]: # (title: Search)
[//]: # (auxiliary-id: Search)

After you have installed and started running TeamCity, it collects the information on builds, tests and so on and indexes it. You can search builds by build number, tag, build configuration name, and other different parameters specifying one or several keywords. You can also search for builds by text in build logs, and by the [external ID](identifier.md#External+IDs) of a build configuration.

By default, TeamCity stores a search index locally. In terms of TeamCity 2021.1 EAP, you can also switch to an [Elastic-based search mode](search-settings.md).

## Search Query

In TeamCity you can search for builds using the [Lucene query syntax](http://lucene.apache.org/). Note that there is a major difference with the default Lucene syntax: by default, TeamCity uses the "prefix search" — not the exact matching like Lucene. For example, if you search for `c:main`, TeamCity will find all builds of the build configuration whose name starts with the `main` string.

As Lucene, TeamCity uses the `OR` operator in a query by default: if there is no Boolean operator between the two terms, `OR` will be used. The `OR` operator links two terms and finds a matching entry if either of these terms exist in it.

To narrow your search and get more precise results, use the available search fields — indexed parameters of each build. For complete list of available search fields (keywords), refer to the [related section](#search-fields-list).

[//]: # (Internal note. Do not delete. "Searchd278e44.txt")

### Performing Fuzzy Search

You also have a possibility to perform fuzzy search using the tilde (`~`) symbol at the end of a single word term to search for items with similar spelling.

### Boolean Operators and Wildcards Support

You can combine multiple terms with Boolean operators to create more complex search queries. In TeamCity, you can use `AND`, `+`, `OR`, `NOT`, and `-`.

When using Boolean operators, type them ALL CAPS.
* `AND` (same as a plus sign). All words that are linked by the `AND` are included in the search results. This operator is used by default.
* `NOT` (same as minus sign in front of a query word). Exclude a word or phrase from search results.
* `OR` operator helps you to fetch the search terms that contain either of the terms you specify in the search field.

TeamCity also supports the `*` and `?` wildcards in a query. It is not recommended using the asterisk (`*`) at the beginning of the search term as it may require a significant amount of time for TeamCity to search its database. For example, the `*onfiguration` search term is incorrect.

## Complete List of Available Search Fields, Shortcuts, and Keywords
{id="search-fields-list"}

### Search Fields

When using search keywords, use the following query syntax:

```JSON
<search field name>:<value to search>

```

<table><tr>

<td>

Search Field

</td>

<td>

Shortcut

</td>

<td>

Description

</td>

<td>

Example

</td></tr><tr>

<td>

agent

</td>

<td>

</td>

<td>

Find all builds that were run on the specified agent.

</td>

<td>

`agent:unit-77`, or `agent:agent14*`

</td></tr><tr>

<td>

build

</td>

<td>

</td>

<td>

Find all builds that include changes with the specified string.

</td>

<td>

`build:254` or `build:failed`

</td></tr><tr>

<td>

buildLog

</td>

<td>

</td>

<td>

Find all builds that include certain text in build logs. It is [disabled](#Using+Double-Colon) by default.

</td>

<td>

`buildLog: "NUnit report"`

</td></tr><tr>

<td>

changes

</td>

<td>

</td>

<td>

Find all builds that include changes with the specified string.

</td>

<td>

`changes:(fix test)`

</td></tr><tr>

<td>

committers

</td>

<td>


</td>

<td>

Find all build that include changes committed by the specified developer.

</td>

<td>

`committers:ivan_ivanov`

</td></tr><tr>

<td>

configuration

</td>

<td>

c

</td>

<td>

Find all builds from the specified build configuration.

</td>

<td>

`configuration:IPR`

`c:(Nightly Build)`


</td></tr><tr>

<td>

labels

</td>

<td>

l

</td>

<td>

Find all builds that include changes with the specified VCS label.

</td>

<td>

`labels:EAP`

`l:release`

</td></tr><tr>

<td>

pin_comment

</td>

<td>

</td>

<td>

Find all builds that were pinned and have the specified word (string) in the pin comment.

</td>

<td>

`pin_comment:publish`

</td></tr><tr>

<td>

project

</td>

<td>

p

</td>

<td>

Find all builds from the specified project.

</td>

<td>

`project:Diana`

`p:Calcutta`

</td></tr><tr>

<td>

revision

</td>

<td>

</td>

<td>

Find all builds that include changes with the specified revision (for example, you can search for builds with a specific changelist from Perforce, or revision number in Subversion).

</td>

<td>

`revision:4536`

</td></tr><tr>

<td>

stamp

</td>

<td>

</td>

<td>

Find all builds that started at the specified time (search by timestamp).

</td>

<td>

`stamp:200811271753`

</td></tr><tr>

<td>

status

</td>

<td>

</td>

<td>

Find all builds with the specified text in the build status text.

</td>

<td>

`status:"Compilation failed"`

</td></tr><tr>

<td>

tags

</td>

<td>

t

</td>

<td>

Find all builds with the specified tag.

</td>

<td>

`tags:buildserver`

`t:release`

</td></tr><tr>

<td>

tests

</td>

<td>

</td>

<td>

Find all builds that include specified tests.

</td>

<td>

`tests:`

</td></tr><tr>

<td>

triggerer

</td>

<td>

</td>

<td>

Find all builds that were triggered by the specified user.

</td>

<td>

`triggerer:ivan.ivanov`

</td></tr><tr>

<td>

vcs

</td>

<td>

</td>

<td>

Find builds that have the specified VCS.

</td>

<td>

`vcs:perforce`

</td></tr><tr>

<td>

build problem

</td>

<td>

</td>

<td>

Find builds with the specified build problem

</td>

<td>

`buildProblem:Compilation failed`

</td></tr></table>

### Shortcuts

In addition to above mentioned search fields, you can the following shortcuts in your query:

<note>

Note that when you use these shortcuts, do not insert the colon after it. That is, the query syntax is as follows: `<shortcut><value to search>`.

</note>

<table><tr>

<td>

Shortcut

</td>

<td>

Description

</td>

<td>

Example

</td></tr><tr>

<td>

`#`

</td>

<td>

Search for the specified build number.

</td>

<td>

`#<number>`, for example, `#1234`

</td></tr><tr>

<td>

`@`

</td>

<td>

Find all builds that were run on the specified agent.

</td>

<td>

`@<agent's name>`, for example, `@buildAgent1`

</td></tr></table>

#### Using Double-Colon

You can use the double-colon sign (`::`) to search for a project and/or build configuration by name:
* `pro::best` — search for builds of configurations with the names starting with "best", and in the projects with the names starting with "pro".
* `mega::` — search for builds in all projects with names starting with "mega".
* `::super` — search for builds of build configurations with names starting with "super".

### "Magic" Keywords

TeamCity also provides "magic" keywords (see table below for the complete list). These magic keywords are formed with the `$` sign and a word itself. The word can be shortened up to one (first) syllable, that is, the `$labeled`, `$l`, and `$lab` keywords will be equal in a query. For example, to search for pinned builds of the "Nightly build" configuration in the "Mega" project you can use any of the following queries:
* `configuration:nightly project:Mega $pinned`
* `c:nigh p:mega $pin`
* `M::night $pin`

<table><tr>

<td width="250">

Magic word

</td>

<td>

Description

</td></tr><tr>

<td>

`$tagged`

</td>

<td>

Search for builds with tags. For example, `Calcutta::Master $t` query will result in a list of all builds marked with any tag of build configurations whose name starts with "Master" from projects with names beginning with `Calcutta`.

</td></tr><tr>

<td>

`$pinned`

</td>

<td>

Search for pinned builds.

</td></tr><tr>

<td>

`$labeled`

</td>

<td>

Search for builds that have been labels in VCS. For example, to find labeled builds of the Main project you can use following queries: `p:Main $labeled`, or `project:Mai $l`, or `m:: $lab`, and so on.

</td></tr><tr>

<td>

`$commented`

</td>

<td>

Search for builds that have been commented.

</td></tr><tr>

<td>

`$personal`

</td>

<td>

Search for personal builds. For example, using `-$p` expression in your query will exclude all personal builds from search results.

</td></tr></table>

## Search by Build Log

By default, TeamCity does not search for builds by a certain text in build logs.

To enable search by the build logs, perform the following:
1. Since the logic will increase server memory usage, you need to increase [memory size](installing-and-configuring-the-teamcity-server.md#Setting+Up+Memory+settings+for+TeamCity+Server) in the `-Xmx` JVM option on at least 5 Gb (more if you have large build logs/many builds).
2. Set the `tc.search.indexBuildLog=true` TeamCity [internal property](configuring-teamcity-server-startup-properties.md#TeamCity+internal+properties).
3. [Reset](#Resetting+Search+Index) the search index.

After reindexing, TeamCity will be able to perform searching by specified text in the build logs and will list the relevant builds.

## Resetting Search Index

The search uses an "index" cached on the disk. Only the data previously added to the index is searchable and appears in the search results. Typically, data updates (such as new builds) cause only incremental updates to the index. This means that when some indexing setting is changed and the cached index on the disk gets out of sync, the index needs to be reset to reindex all the builds anew.

To reset the cached search index, click `reset` for the "search" entry on the __Administration | Server Administration | Diagnostics__, __Caches__ tab or manually delete files from \<[TeamCity Data Directory](teamcity-data-directory.md)\>\system\caches\search while the server is not running. After that, reindexing will start which is a resource-intensive operation on the server and can take hours (depending on the number and "size" of builds, as well as the server machine and database performance). You can monitor the progress in the UI on the Search page or in the server logs.


[//]: # (Internal note. Do not delete. "Searchd278e621.txt")

<seealso>
        <category ref="admin-guide">
            <a href="search-settings.md">Storing Project Settings in Version Control</a>
        </category>
</seealso>