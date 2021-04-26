[//]: # (title: Perforce Streams as feature branches)
[//]: # (auxiliary-id: Perforce Streams as feature branches)

## How to enable

In the [Perforce VCS root](perforce.md) settings, select the "_Enable feature branches support_" option next to the parent stream name. After that, all streams which have the specified main stream as a parent will be included into the [feature branches](working-with-feature-branches.md).

It is possible to specify some mapping to include only specific streams into the feature branches set, like `+://stream-depot/*`. In this case, only streams under depot __stream-depot__ will be included for changes collection/build triggering.

## Task streams

Task streams are supported, but new task streams are not detected by TeamCity until there is a non-merge commit into this stream.

## Remote run

Remote run from IDEA is possible only in a stream which was already detected by TeamCity. TeamCity remote run plugin tries to deduce the correct stream according to the depot paths of the files in the IDE working copy. 

For instance, if a file path in the working copy starts with `//depot/stream1/some/path`, TeamCity will try finding `//depot/stream1` stream and start remote run there.

But if you modified a file from another stream (imported into the working copy) and want to enforce build in a particular stream, you should specify a configuration parameter `teamcity.build.branch` when triggering the remote run.

<img src="perforce-stream.png" width="367" alt="Remote run of Perforce stream"/>
