[//]: # (title: Search Settings)
[//]: # (auxiliary-id: Search Settings)

You can change the search mode used on the server in the __Builds Search__ section of the Root project's settings.

The Lucene search syntax, supported for both modes, is described in [this article](search.md).

## Local Search

By default, TeamCity stores the search index locally, in [`<TeamCity Data Directory>`](teamcity-data-directory.md)`/system/caches`. In the __Builds Search__ section, you can control the indexing and see its statistics.

## Elasticsearch
{id="ElasticSearchSettings" auxiliary-id="ElasticSearchSettings"}

TeamCity provides an alternative search mode, based on [Elasticsearch](https://www.elastic.co/). The new mode has two advantages: (1) it saves disk space on the TeamCity server machine and (2) it is better for the TeamCity performance. It is especially effective for multinode installations, as nodes spend fewer resources on maintaining a single remote index than on multiple local indexes.

To connect to your Elastic host or cluster, enter its URL and credentials. You can also set a custom name for a TeamCity index.

After you save the new settings, TeamCity will spend some time reindexing entries. The exact duration depends on the size of your server. You can track or control the progress in the _Diagnostics_ table.

<seealso>
        <category ref="user-guide">
            <a href="search.md">Search</a>
        </category>
</seealso>
