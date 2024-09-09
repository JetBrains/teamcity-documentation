[//]: # (title: Ruby Environment Configurator)
[//]: # (auxiliary-id: Ruby Environment Configurator)

The _Ruby environment configurator_ [build feature](adding-build-features.md) passes the Ruby interpreter to all build steps. It (1) adds the selected Ruby interpreter and gems bin directories to the system `PATH` environment variable and (2) configures other necessary environment variables in case of the [RVM](https://rvm.io/) interpreter.

## Example Use Case

For example, this feature allows using commands like `ruby`, `rake`, `gem`, `bundle` in the [Command Line](command-line.md) build runner. Thus, if you want to install gems before launching the [Rake](rake.md) build runner, you need to add the [Command Line](command-line.md) build step which launches a custom script. Example:

```Shell
gem install rake --no-ri --no-rdoc
gem install bundler --no-ri --no-rdoc

```

## Ruby Environment Configurator Settings

<table>
<tr>

<td>

Option

</td>

<td>

Description

</td>
</tr>
<tr>

<td>

Ruby interpreter path

</td>

<td>

The path to the Ruby interpreter. If not specified, the interpreter will be searched in `PATH`. In this field, you can use values of environment and system variables.

Example:

```
%\env.I_AM_DEFINED_IN_BUILDAGENT_CONFIGURATION%

```

</td>
</tr>
<tr>

<td>

RVM interpreter

</td>

<td>

The RVM interpreter name and, optionally, a gemset configured on a build agent.

The interpreter name cannot be empty. If gemset is not specified, the default one will be used.

This option can be used if you do not want to use the `.rvmrc` settings: for instance, to run tests on different Ruby interpreters instead of those defined in the `.rvmrc` file.

</td>
</tr>
<tr>

<td>

RVM with .rvmrc file

</td>

<td>

The path to the `.rvmrc` file relative to the checkout directory. If specified, TeamCity will fetch environment variables using rvm-shell and pass them to all build steps.

</td>
</tr>
<tr>

<td>

Fail build if Ruby interpreter wasn't found

</td>

<td>

Enabling it will fail a build in case the Ruby environment configurator cannot pass the Ruby interpreter to the step execution environment, because the interpreter hasn't been found on the agent.

</td>
</tr>
</table>

<seealso>
        <category ref="admin-guide">
            <a href="configuring-build-steps.md">Configuring Build Steps</a>
            <a href="command-line.md">Command Line</a>
            <a href="rake.md">Rake</a>
        </category>
</seealso>