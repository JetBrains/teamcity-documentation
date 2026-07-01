[//]: # (title: Pass Data in a Chain)

<show-structure for="chapter" depth="2"/>

Members of a [build chain](chains-topic-1.md) can exchange two kinds of data:

* **Files** — through [artifact dependencies](#Artifact+Dependencies). An upstream build publishes files; a downstream build downloads them before it starts.
* **Values** — through [output parameters](#Parameters). A name-value pair set in one object is read by another further down the chain.

Both mechanisms flow in the direction of the chain: data moves from upstream to downstream, never the reverse.

## Artifact Dependencies

An _artifact dependency_ reuses the output ([artifacts](build-artifact.md)) of one build in another. When configured, TeamCity downloads the required files to the agent before the downstream build starts.

Artifacts can be taken from:

* A build of an upstream object in the same chain.
* A build of a configuration that is not part of the chain.
* A previous build of the same configuration.

> An artifact dependency only transfers files — it does not enforce build order or revision synchronization. To guarantee that artifacts come from a build on the same sources, pair the artifact dependency with a [snapshot dependency](chains-topic-2.md#Snapshot+Dependencies) and choose the **Build from the same chain** source option.
>
{style="tip"}

### Configuring Artifact Dependencies

For build configurations, add an artifact dependency on the **Dependencies** page of configuration settings.

<img src="dk-add-artifact-dependency.png" width="706" alt="Add artifact dependency"/>

The dialog has the following key settings:

<img src="dk-artifact-dependency-dialog.png" width="706" alt="Artifact dependency dialog"/>

<deflist type="full">
<def title="Depend on">

The configuration whose artifacts you want to use.

</def>
<def title="Get artifacts from">

Which build of the source configuration to take artifacts from:

* **Latest successful build** / **Latest pinned build** / **Latest finished build** — the most recent matching build.
* **Build from the same chain** — the same-chain build. Use this together with a snapshot dependency. If the source is outside the chain, the build fails.
* **Build from the same chain or last finished** — same as above, but falls back to the latest finished build when the source is not in the chain.
* **Build with specified build number** / **Latest finished build with specified tag** — a specific build, identified by number or tag.

> When pinning to a specific build, account for your [clean-up policy](teamcity-data-clean-up.md): regular builds may be deleted, leaving the dependency pointing at a non-existent build. A build referenced by number is protected from clean-up.
>
{style="note"}

</def>
<def title="Artifact rules">

Which files to download and where to place them. See [Artifact Rules](#Artifact+Rules) below.

</def>
<def title="Clean destination paths before downloading artifacts">

Deletes the contents of the destination directories before downloading, applied to all inclusive rules.

</def>
</deflist>

In pipelines, artifact dependencies are configured in YAML. This involves two separate pipelines, each with its own configuration file.

First, a job in the **upstream** pipeline publishes the file via a `files-publication` block:

```yaml
# Upstream pipeline
jobs:
  Build:
    steps:
      - type: gradle
        tasks: clean build
    files-publication:
      - path: ./build/libs/todo.jar
        share-with-jobs: true
        publish-artifact: true
```

Then a job in the **downstream** pipeline imports it via a `download-artifacts` block, referencing the upstream pipeline declared in its `dependencies`:

```yaml
# Downstream pipeline
jobs:
  Docker:
    steps:
      - type: script
        script-content: docker build -t myapp:%build.number% .
    download-artifacts:
      - UpstreamPipeline_ID:    # same ID as in the 'dependencies' block
          from: dependency
          artifact-rules: todo.jar=>./build/libs
          clean-destination: true
dependencies:
  - UpstreamPipeline_ID:
      reuse: none
```

### Artifact Rules

An artifact rule specifies which artifacts to download and where to store them. Each rule goes on its own line and uses the following syntax:

```
[+:|-:|?:]SourcePath[!ArchivePath][=>DestinationPath]
```

<deflist type="medium">
<def title="Prefix">

* `+:` — include (default; `mylib.dll` and `+:mylib.dll` are identical).
* `-:` — exclude a file or pattern from the download.
* `?:` — optional include. If the file is missing, the build continues with a warning instead of failing.

</def>
<def title="SourcePath">

Path relative to the source build's artifacts directory. Accepts a file, a directory, or [Ant-like wildcards](wildcards.md). The source directory structure is preserved starting from the first wildcard.

</def>
<def title="ArchivePath">

Extracts files from a downloaded archive (`zip`, `7z`, `jar`, `tar`, `tar.gz`). For example, `release.zip!*.dll` extracts the `.dll` files from the archive root.

</def>
<def title="DestinationPath">

Target directory on the agent, relative to the build checkout directory. If omitted, artifacts go to the checkout root. Ignored for `-:` rules.

</def>
</deflist>

Examples:

```
# Download a directory tree into lib/ (a/b/c/file.txt → lib/c/file.txt)
a/b/**=>lib

# Download all text files, preserving structure
**/*.txt=>lib

# Extract all DLLs from matching archives
release-*.zip!*.dll=>dlls

# Download everything except one file
**/*.txt=>texts
-:bad/exclude.txt

# Optional file — build continues even if it is missing
?:output.txt
```

The `?:` prefix is especially useful in [partial chains](chains-topic-4.md#Partial+Chain+Execution): if an upstream build that normally provides a file is skipped, an optional rule keeps the downstream build from failing.

## Parameters

Chain members exchange values through [build parameters](configuring-build-parameters.md). This section is a quick overview — for the full syntax, examples, and edge cases, see [](use-parameters-in-build-chains.md).

A value is shared down the chain by declaring it as an **output parameter** in the upstream object. Input parameters stay private to their owner; output parameters are visible to downstream objects.

<deflist type="full">
<def title="Read an upstream value">

A downstream object reads an upstream output parameter via `dep.<upstream-ID>.<param-name>`.

</def>
<def title="Read another job's value">

Within a single pipeline, a job reads a preceding job's parameter via `job.<job-ID>.<param-name>`.

</def>
<def title="Override an upstream value">

A downstream object writes back to an upstream input parameter via `override.dep.<upstream-ID>.<param-name>`. This is the only case where data flows against the chain direction, and it is resolved before the upstream build starts.

</def>
</deflist>

Output parameters are designed to be read by *other* objects, not by the one that declares them. If a configuration or pipeline references its own output parameter, the reference does not resolve: TeamCity treats the unresolved `%...%` as an [implicit agent requirement](configuring-agent-requirements.md#Implicit+Requirements), and the build fails to start with a "no compatible agents" error. The snippet below demonstrates this.

<include from="pipeline-settings.md" element-id="output-param-in-self"/>

For build configurations, the equivalent declaration uses `params` and `outputParams` blocks. See [](use-parameters-in-build-chains.md) for complete examples of all three patterns above, the `*` wildcard for overriding multiple objects at once, and conflict resolution rules.
