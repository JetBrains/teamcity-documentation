[//]: # (title: Rake)
[//]: # (auxiliary-id: Rake)
<tip>

TeamCity Rake runner supports the Test::Unit, Test\-Spec, [Shoulda](http://github.com/thoughtbot/shoulda), [RSpec](http://rspec.info/), [Cucumber](http://cukes.info/) test frameworks. It is compatible with Ruby interpreters installed using Ruby Version Manager (MRI Ruby, JRuby, IronRuby, REE, MacRuby, and so on) with Rake 0.7.3 gem or higher.
</tip>

## Prerequisites

Make sure to have Ruby interpreter (MRI Ruby, JRuby, IronRuby, REE, MacRuby, and so on) with rake 0.7.3 gem or higher (mandatory) and all necessary gems for your Ruby (or ROR) projects and testing frameworks installed on at least one build agent. You can install several Ruby interpreters in different folders. On Linux/MacOS it is easier to configure using [RVM](http://rvm.io/) or [rbenv](http://github.com/sstephenson/rbenv). It is possible to install Ruby interpreter and necessary Ruby gems using the [Command Line](command-line.md) build runner step. If you want to automatically configure agent requirements for this interpreters, you need to register its paths in the build agent configuration properties and then refer to such property name in the [Rake build runner configuration](#Rake+Runner+Settings). To install a gem, execute:


```Shell
gem install <gem_name>

```

You can refer to the [Ruby Gems Manuals](http://docs.rubygems.org/) for more information.

Instead of the `gem` command, you can install gems using the [Bundler gem](http://bundler.io/).

<note>

If you use Ruby 1.9 for Shoulda, Test\-Spec and Test::Unit frameworks to operate, the `test-unit` gem must be installed.
</note>

<note>

To use the [minitest](https://rubygems.org/gems/minitest) framework, the `minitest-reporters` gem must be installed. See details in the [RubyMine webhelp](http://www.jetbrains.com/ruby/webhelp/minitest.html).
</note>

## Important Notes
* Ruby's _pending specs_ are shown as __Ignored Tests__ in the __Overwiew__ tab.
* Rake Runner uses its own unit tests runner and loads it using the `RUBYLIB` environment variable. You need to ensure your program doesn't clear this environment variable, but you may append your paths to it.
* If you run RSpec with the `--color` option enabled under Windows OS, RSpec will suggest you install the __win32console__ gem. This warning will appear in your build log, but you can ignore it. TeamCity Rake Runner doesn't support coloured output in the build log and doesn't use this feature.
* Rake Runner runs spec examples with a custom formatter. If you use additional console formatter, your build log will contain redundant information.
* `Spec::Rake::SpecTask.spec_opts` of your rakefile is affected by `SPEC_OPTS` command line parameter. Rake Runner always uses `SPEC_OPTS` to set up its custom formatter. Thus you should set up Spec Options in Web UI. The same limitation exists for Cucumber tests options.
* To include HTML reports into the Build Results, you can add the corresponding [report tab](including-third-party-reports-in-the-build-results.md) for them.

## Rake Runner Settings

### Rake Parameters

<table><tr>

<td>

Option


</td>

<td>

Description


</td></tr><tr>

<td>

Path to a Rakefile file


</td>

<td>

Enter a Rakefile path if you don't want to use the default one. The specified path should be relative to the [Build Checkout Directory](build-checkout-directory.md).


</td></tr><tr>

<td>

Rakefile content


</td>

<td>

Type in the Rakefile content instead of using the existing Rakefile. The new Rakefile will be created dynamically from the specified content before running Rake.


</td></tr><tr>

<td>

Working directory


</td>

<td>

Optional. Specify if differs from the [Build Checkout Directory](build-checkout-directory.md).


</td></tr><tr>

<td>

Rake tasks


</td>

<td>

Enter space\-separated tasks names if you don't want to use the `default` task.  For example, `test:functionals` or `mytask:test mytask:test2`.


</td></tr><tr>

<td>

Additional Rake command line parameters


</td>

<td>

Specified parameters will be added to `rake` command line. The command line will have the following format:


```Shell
ruby rake <Additional Rake command line parameters>
<TeamCity Rake Runner options, e.g TESTOPTS> <tasks>

```




</td></tr></table>

### Ruby Interpreter

<table><tr>

<td>

Option


</td>

<td>

Description


</td></tr><tr>

<td>

Use default Ruby


</td>

<td>

Use Ruby interpreter settings defined in the [Ruby environment configurator](ruby-environment-configurator.md) build feature settings or the interpreter will be searched in the `PATH`.


</td></tr><tr>

<td>

Ruby interpreter path


</td>

<td>

The path to Ruby interpreter. The path cannot be empty. This field supports values of environment and system variables. For example:


```Shell
%env.I_AM_DEFINED_IN_BUILDAGENT_CONFIGURATION%

```


</td></tr><tr>

<td>

RVM interpreter


</td>

<td>

Specify here the RVM interpreter name and optionally a gemset configured on a build agent.  Note, the interpreter name cannot be empty. If gemset isn't specified, the default one will be used.


</td></tr></table>

### Launching Parameters

<table><tr>

<td>

Option


</td>

<td>

Description


</td></tr><tr>

<td>

Bundler: bundle exec


</td>

<td>

If your project uses the [Bundler requirements](http://gembundler.com) manager and your Rakefile doesn't load the bundler setup script, this option will allow you to launch rake tasks using the `bundle exec` command emulation. If you want to execute `bundle install` command, you need to do it in the [Command Line](command-line.md) step before the _Rake runner_ step. Also, remember to set up the [Ruby environment configurator](ruby-environment-configurator.md) build feature to automatically pass Ruby interpreter to the command line runner.


</td></tr><tr>

<td>

Debug


</td>

<td>

Check the __Track invoke/execute stages__ option to enable showing _Invoke_ stage data in the build log.


</td></tr></table>

### Tests Reporting

<table><tr>

<td>

Option


</td>

<td>

Description


</td></tr><tr>

<td>

Attached reporters


</td>

<td>

If you want TeamCity to display the test results on a dedicated [Tests tab](working-with-build-results.md#All+Tests) of the __Build Results__ page, select here the testing framework you use: Test::Unit, Test\-Spec, Shoulda, RSpec or Cucumber.

<note>

If you're using RSpec or Cucumber, make sure to specify here the user options defined in your build script, otherwise they will be ignored.
</note>


</td></tr></table>

## Known Issues
* If your Rake tasks or tests run in parallel in the scope of one build, the build output and tests results will be inaccurate.
* If you are using RVM, it is recommended to start TeamCity agent when the current rvm sdk isn't set or to invoke `rvm system` at first.

## Additional Runner Options

These options can be configured using system properties in the [Build Parameters](configuring-build-parameters.md) section.

<table><tr>

<td width="300">

Option


</td>

<td>

Description


</td></tr><tr>

<td>

`system.teamcity.rake.runner.gem.rake.version`


</td>

<td>

Allows specifying which rake gem to use for launching a rake build.


</td></tr><tr>

<td>

`system.teamcity.rake.runner.gem.testunit.version`


</td>

<td>

If your application uses the test\-unit gem version other than the latest installed (in Ruby sdk), specify it here. Otherwise the Test::Unit test reporter may try to load the incorrect gem version and affect the runtime behavior. If the test\-unit gem is installed but you application uses Test::Unit bundled in Ruby 1.8.x SDK, set the version value to 'built\-in'.


</td></tr><tr>

<td>

`system.teamcity.rake.runner.gem.bundler.version`


</td>

<td>

Launches bundler emulation for the specified bundler gem version (the gem should be already installed on an agent.


</td></tr><tr>

<td>

`system.teamcity.rake.runner.custom.gemfile`


</td>

<td>

Customizes Gemfile if it isn't located in the checkout directory root.


</td></tr><tr>

<td>

`system.teamcity.rake.runner.custom.bundle.path`


</td>

<td>

Sets `BUNDLE_PATH` if TeamCity doesn't fetch it correctly from `<Gemfile_containing_directory>/.bundle/config`.


</td></tr></table>

## Development Links

Rake support is implemented as an open\-source plugin. For development links refer to the [plugin's page](https://confluence.jetbrains.com/display/TW/Rake+Runner).

__ __