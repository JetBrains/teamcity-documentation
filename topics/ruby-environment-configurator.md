[//]: # (title: Ruby Environment Configurator)
[//]: # (auxiliary-id: Ruby Environment Configurator)
The _Ruby environment configurator_ build feature passes Ruby interpreter to all build steps. The build feature adds the selected Ruby interpreter and gems bin directories to the system PATH environment variable and configures other necessary environment variables in case of the [RVM](http://rvm.io/) interpreter. For example, in the [Command Line](command-line.md) build runner you will be able to directly use such commands as _ruby_, _rake_, _gem_, _bundle_, and so on. Thus, if you want to install gems before launching the [Rake](rake.md) build runner, you need to add the [Command Line](command-line.md) build step which launches a custom script, for example:



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

The path to Ruby interpreter. If not specified, the interpreter will be searched in the `PATH`. In this field you can use values of environment and system variables.   
For example:

```Plain Text
%env.I_AM_DEFINED_IN_BUILDAGENT_CONFIGURATION%

```


 

</td>
</tr>
<tr>


<td>

RVM interpreter 


</td>


<td>

Specify here the RVM interpreter name and optionally a gemset configured on a build agent.
Note, that the interpreter name cannot be empty. If gemset isn't specified, the default one will be used. 


This option can be used if you don't want to use the `.rvmrc` settings, for instance to run tests on different ruby interpreters instead of those hard\-coded in the `.rvmrc` file.

</td>
</tr>
<tr>


<td>

RVM with .rvmrc file 


</td>


<td>

Specify here the path to a `.rvmrc` file relative to the checkout directory. If the file is specified, TeamCity will fetch environment variables using the rvm\-shell and will pass it to all build steps.  


</td>
</tr>
<tr>


<td>

Fail build if Ruby interpreter wasn't found 


</td>


<td>

Check the option to fail a build if the Ruby environment configurator cannot pass the Ruby interpreter to the step execution environment because the interpreter wasn't found on the agent. 


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