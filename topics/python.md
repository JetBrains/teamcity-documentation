[//]: # (title: Python)
[//]: # (auxiliary-id: Python)

The Python build runner automatically detects Python on agents and allows running Python scripts on Windows, Linux, and macOS.

>Since TeamCity 2020.2, this bundled runner replaces the obsolete [Python Runner plugin](https://plugins.jetbrains.com/plugin/9042-python-runner). The new runner offers support for virtual environments, Docker, Kotlin DSL, and provides extra features like full test coverage.

Refer to [Configuring Build Steps](configuring-build-steps.md) for a description of common build steps' settings. Refer to [](container-wrapper.md) to learn how you can run this step inside a Docker container.

<video src="https://youtu.be/DOLY1K5e8hQ"
title="New in TeamCity 2020.2: Python Build Runner"/>

## Сommand settings
{id="pythonCommandSettings" auxiliary-id="Python command settings"}

You can choose one of the following Python commands:

<table>

<tr>

<td width="100">

Command

</td>

<td>

Description

</td>

<td>

Example input

</td>

</tr>

<tr>

<td>

File

</td>

<td>

Runs a Python script via the provided path.

</td>

<td>

`scripts/test.py`

</td>

</tr>

<tr>

<td>

Module

</td>

<td>

Runs a Python [module](https://docs.python.org/3/tutorial/modules.html) via the provided name (equally to adding `-m` before the module name).

</td>

<td>

`example-module`

</td>

</tr>

<tr>

<td>

Custom script

</td>

<td>

Allows entering a Python script's body in the UI form.

</td>

<td>

```python
ips = {}

fh = open("/var/log/nginx/access.log", "r").readlines()
for line in fh:
    ip = line.split(" ")[0]
    if 6 < len(ip) <=15:
        ips[ip] = ips.get(ip, 0) + 1
print ips
```

</td>

</tr>

<tr>

<td>

Unittest

</td>

<td>

Launch the [unittest](https://docs.python.org/3/library/unittest.html) framework. The unit test results will be displayed on the __[Tests](build-results-page.md#Tests+Tab)__ tab of __Build Results__.

To filter the scope of processed files, you can specify the path to unit test file(s) in the additional arguments.

</td>

<td>

Optionally, enter `tests/*.py` as an argument to launch all Python files in the `tests` directory via the unittest framework.

</td>

</tr>

<tr>

<td>

Pytest

</td>

<td>

Launch the [pytest](https://docs.pytest.org/en/stable/index.html) framework. The test results will be displayed on the __[Tests](build-results-page.md#Tests+Tab)__ tab of __Build Results__.

To filter the scope of processed files, you can specify the path to pytest file(s) in the additional arguments.

</td>

<td>

Optionally, enter `tests/*.py` as an argument to launch all Python files in the `tests` directory via the pytest framework.

</td>

</tr>

<tr>

<td>

Flake8

</td>

<td>

Launch the [Flake8](https://pypi.org/project/flake8/) wrapper. The code inspection results will be displayed on the __[Code Inspection](build-results-page.md#Code+Inspection+Tab)__ tab of __Build Results__.

To filter the scope of processed files, you can specify the path to Python file(s) in the additional arguments.

</td>

<td>

Optionally, enter `docs/conf.py` as an argument to check the `conf.py` file for errors.

</td>

</tr>

<tr>

<td>

Pylint

</td>

<td>

Launch the [Pylint](https://pypi.org/project/pylint/) tool. The code inspection results will be displayed on the __[Code Inspection](build-results-page.md#Code+Inspection+Tab)__ tab of __Build Results__.

To filter the scope of processed files, specify the path to Python file(s) in the additional arguments.

</td>

<td>

Enter `scripts/*.py` as an argument to check all Python files in the `scripts` directory for errors.

</td>

</tr>

<tr>

<td>

Custom

</td>

<td>

Arguments of the Python interpreter (for example, `python3 arg1 arg2`). Use this command if you want to enter a custom set of arguments.

If the step is launched in a virtual environment, these arguments are applied to the `python` command inside the environment (for example, `python3 -m pipenv run python arg1 arg2`).

</td>

<td>

`arg1 arg2`

</td>

</tr>


</table>

The set of available settings depends on the selected command. This table describes all command settings:

<table>

<tr><td>

Setting

</td><td>

Description

</td></tr>

<tr><td>

File

</td><td>

Path to a Python script file.

</td></tr>

<tr><td>

Module

</td><td>

Path to a Python module file.

</td></tr>

<tr><td>

Install tool

</td><td>

Select this option to automatically install the selected tool package (Pytest, Flake8, or Pylint) if it is missing on the build agent.

</td></tr>

<tr><td>

Script or module arguments

</td><td>

Arguments that will be passed to the user script or module after their call.

</td></tr>

<tr><td>

Script

</td><td>

The custom script body.

</td></tr>

<tr><td>

Test reporting

</td><td>

Enables test reporting via the automatically installed [`teamcity-messages`](https://pypi.python.org/pypi/teamcity-messages) package. TeamCity receives test reports via [service messages](service-messages.md) and displays them in the build log.

</td></tr>

<tr><td>

Coverage

</td><td>

Enable code coverage collecting via [Coverage.py](https://coverage.readthedocs.io/en/coverage-5.3/). TeamCity displays the produced test report on the __[Code Coverage](build-results-page.md#Code+Coverage+Tab)__ tab.

</td></tr>

<tr><td>

Python arguments

</td><td>

Arguments that will be passed to the Python interpreter if a custom command is selected.

</td></tr>


</table>

## Python executable settings
{id="pythonVersion" auxiliary-id="Python autodetection"}

In this block of settings, you can choose a Python version to run.

The Python runner automatically detects Python versions installed on a [build agent](build-agent.md). The runner checks the following locations (in the same order as listed below):

On Windows:
   
   1. The default installation paths
   2. The system registry
   3. The `PATH` variable

On Linux and macOS:
   
   1. The default installation paths
   2. The `PATH` variable


The runner sets the first detected versions of Python 2.x and 3.x as the agent's configuration parameters. Alternatively, you can provide a path to any installed version manually.

<img src="dk-python-custompath.png" width="706" alt="Custom Python path"/>

You can also specify arguments that will be passed to the interpreter in every Python run of this build step (for example, a custom environment tool run or reporting run).

## Environment tool settings
{id="pythonEnvTool" auxiliary-id="Python environment tool settings"}

Optionally, you can run a Python build step in a virtual environment. The Python runner supports the following tools:
* [Pipenv](#Pipenv+settings)
* [Poetry](#Poetry+settings)
* [Venv](#Venv+and+virtualenv+settings)
* [Virtualenv](#Venv+and+virtualenv+settings)

### Pipenv settings

Optionally, enter [install run arguments](https://pipenv.pypa.io/en/latest/cli/#pipenv-install).

### Poetry settings

Optionally, enter [install run arguments](https://python-poetry.org/docs/cli/#install) and a custom executable path to Poetry installed on a build agent.

The `poetry install` command will be run for this environment tool. It will resolve dependencies specified in the `pyproject.toml` file, located in the [working directory](build-working-directory.md). To be parsed correctly, this file should contain the `tool.poetry` section.

### Venv and virtualenv settings

>If TeamCity finds the `requirements.txt` file when [autodetecting build steps](configuring-build-steps.md#Autodetecting+Build+Steps) from a project repository, it chooses venv as a tool for these settings by default. You can manually change it to virtualenv if necessary.   
>[Read more](https://docs.python.org/3/installing/index.html#key-terms) about the differences between these tools.

[Venv](https://docs.python.org/3/library/venv.html) and [virtualenv](https://virtualenv.pypa.io/en/latest/) have the following settings:

<table>

<tr><td>

Setting

</td><td>

Description

</td></tr>

<tr><td>

Requirement files

</td><td>

Path to the requirements file or a new-line separated list of files.

</td></tr>

<tr><td>

Pip install run arguments

</td><td>

Additional arguments for the `pip install run` command.

</td></tr>

<tr><td>

Environment name

</td><td>

Name of the virtual environment.

</td></tr>

<tr><td>

Venv / virtualenv create arguments

</td><td>

Additional venv / virtualenv arguments to create a new `env` command.

</td></tr>

</table>

<seealso>
        <category ref="admin-guide">
            <a href="configuring-build-steps.md">Configuring Build Steps</a>
            <a href="container-wrapper.md">Container Wrapper</a>
        </category>
</seealso>