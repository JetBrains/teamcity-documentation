[//]: # (title: Search Settings)
[//]: # (auxiliary-id: Search Settings)

You can change the [search](search.md) mode used on the server in the __Search__ section of the Root project's settings.

## Local Search

By default, TeamCity stores a search index locally, in the [Data Directory](teamcity-data-directory.md). In the __Search__ section, you can control the indexing and see its statistics.

## Elastic-based Search

In terms of TeamCity 2021.1 EAP, TeamCity provides an alternative search mode, based on [Elastic](https://www.elastic.co/). It allows storing a global search index on your Elastic host, which is most optimal for TeamCity [multinode installations](multinode-setup.md), as it saves nodes' resources on maintaining own local indexes and works faster.

To connect to your Elastic host or cluster, enter its URL and credentials. You can also set a custom name for a TeamCity index.

After you save the new settings, TeamCity will spend some time reindexing entries. The exact duration depends on the size of your server. You can track or control the progress in the _Diagnostics_ table.

Note that in terms of EAP, results provided by the Elastic search can differ from those of the local search. Search by build log is currently supported only for the local index, but we plan to support it in the Elastic search as well.

<seealso>
        <category ref="user-guide">
            <a href="search.md">Search</a>
        </category>
</seealso>