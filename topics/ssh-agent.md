[//]: # (title: SSH Agent)
[//]: # (auxiliary-id: SSH Agent)

The _SSH Agent_ [build feature](adding-build-features.md) runs an SSH agent with the selected [uploaded SSH key](ssh-keys-management.md) during a build. When your build script runs an SSH client, it uses the SSH agent with the loaded key.

Check [SSH Keys Management](ssh-keys-management.md) for SSH key upload notes.

## Agent Setup

The TeamCity SSH agent uses a native SSH agent from the OpenSSH included with Linux and macOS, so the feature works out of the box for these OSs. For Windows, OpenSSH needs to be installed (for example, as a part of CygWin, MinGW or a part of Git distribution for Windows).

The SSH agent must be added to `$PATH` on Unix-like OSs and to `%PATH%` on Windows.

For each TeamCity build agent, a separate SSH agent is started, so it is possible to use this feature if several build agents are installed on the same machine.

## Disabling SSH Host Key Checking

The first time you connect to a remote host, the SSH client asks if you want to add a remote host's fingerprint to the known hosts database at `~/.ssh/known_hosts`.

To avoid such prompts during a build, you need to configure the known hosts database beforehand. If you trust the hosts you are connecting to, you can disable known hosts checks:
* either __for all connections__ by adding something like this in `~/.ssh/config`:   
   ```Shell
   Host *
       StrictHostKeyChecking no

   ```

* or __for an individual command__ by running an SSH client with the `-o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no` options.

See more information in the man pages for [`ssh`](http://linux.die.net/man/1/ssh), [`ssh-agent`](http://linux.die.net/man/1/ssh-agent) and [`ssh-add`](http://linux.die.net/man/1/ssh-add) commands.

## Using Multiple Keys in One Build

If a build needs to authenticate in several external systems, it can use more than one SSH key.

To use multiple SSH keys in a build:
* On the project level: add the keys on the __[SSH Keys](ssh-keys-management.md)__ page.
* On the build configuration level: add multiple _SSH Agent_ [build features](adding-build-features.md), one per each key.

When a build starts, it downloads the SSH keys from the server and runs an SSH agent that distributes the keys on demand of other build steps.

<note>

__SSH agent cannot use multiple keys when connecting to the same host__

When authenticating to a host, an SSH agent remembers and henceforth uses the very first applicable key. Since each build launches a single SSH agent, it is not able to use different keys for one host.

However, some setups might require using more than one key when authenticating to the same host: for example, if there are several GitHub repositories on one host that require different [deploy keys](https://developer.github.com/v3/guides/managing-deploy-keys/#deploy-keys). In such cases, we suggest that you create different build configurations for each repository and connect them into a [build chain](build-chain.md).

</note>

__ __