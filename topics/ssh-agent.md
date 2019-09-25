[//]: # (title: SSH Agent)
[//]: # (auxiliary-id: SSH Agent)

The _SSH Agent_ [build feature](adding-build-features.md) runs an SSH agent with the selected [uploaded SSH key](ssh-keys-management.md) during a build. When your build script runs an SSH client, it uses the SSH agent with the loaded key.

Check [SSH Keys Management](ssh-keys-management.md) for SSH key upload notes.

## Agent Setup

_TeamCity SSH Agent_ uses a native SSH agent from the OpenSSH included with Linux and Mac OS X, so the feature works out of the box for these OSs. For Windows, OpenSSH needs to be installed (for example, as a part of CygWin, MinGW or a part of Git distribution for Windows).

The SSH agent must be added to `$PATH` on Unix-like OSs and to `%PATH%` on Windows.

For each TeamCity build agent a separate ssh agent is started, so it is possible to use this feature if several build agents are installed on the same machine.

## Disabling SSH host key checking

The first time you connect to a remote host, the SSH client asks if you want to add a remote host's fingerprint to the known hosts database at `~/.ssh/known_hosts`.

To avoid such prompts during a build, you need to configure the known hosts database beforehand. If you trust the hosts you are connecting to, you can disable known hosts checks:
* either __for all connections__ by adding something like this in `~/.ssh/config`:   
   ```Shell
   Host *
       StrictHostKeyChecking no

   ```

* or __for an individual command__ by running an SSH client with the `-o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no` options.

See more information in the man pages for [ssh](http://linux.die.net/man/1/ssh), [ssh-agent](http://linux.die.net/man/1/ssh-agent) and [ssh-add](http://linux.die.net/man/1/ssh-add) commands.