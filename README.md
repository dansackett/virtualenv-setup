# Virtaulenv and Virtualenvwrapper Setup

When starting a new Python project, it's best to keep your project dependencies
isolated from your host and other potential projects. For Python, two of our
favorite tools to handle this are
[virtualenv](https://pypi.python.org/pypi/virtualenv/14.0.5) and
[virtualenvwrapper](https://pypi.python.org/pypi/virtualenvwrapper/4.7.1).

## How does it work?

The main concepts to remember with this setup are:

- All projects exist in one root directory
- All dependencies are centralized in another directory

Setting things up this way keeps us both organized and isolated. When we
want to work on a project, we can access the project directory and view the
project source without worrying about the dependencies. If we do want to view the
project dependencies, we can easily navigate to the dependencies directory and
view them.

This isolation doesn't just extend to dependencies. With this setup we can also
handle environments with a different Python version. This is possible because
each environment gets its own python interpreter installed with it. Again, this
is complete isolation from your host and other projects.

In most cases, you don't need virtualenvwrapper. Virtualenv alone can provide
isolation for your projects and will keep things simple for you.
Virtualenvwrapper gives us a number of commands that make the process much
easier. As well, it provides a set of hooks that we can use to do different
tasks during an environment's lifecycle. It is these hooks that we really take
advantage of in our setup.

The following steps will help us get things setup.

## Installation

The best way to install virtualenv and virtualenvwrapper is from PyPI via
[pip](https://pip.pypa.io/en/stable/):

```
$ pip install virtualenv virtualenvwrapper
```

Once installed, we can setup our two main directories. We like to call them
`projects` and `.virtualenvs` but theoretically you can call them whatever you
want as long as you reference them correctly.

```
$ mkdir ~/.virtualenvs ~/projects
```

The next step for setup involves editing your `~/.bashrc` file and adding the
following lines:

```
export WORKON_HOME=~/.virtualenvs
export PROJECT_HOME=~/projects
export PIP_VIRTUALENV_BASE=~/.virtualenvs
source /usr/local/bin/virtualenvwrapper.sh
```

If you chose different names for the directories you created then remember to
use those names in the .bashrc file. Our final step is to install the hooks.
You can find the relevant hooks in the `.virtualenvs/` directory in this
repository. If you cloned this locally, you can run:

```
$ cp /path/to/this/repo/.virtualenvs/* ~/.virtualenvs
```

## Hooks

While most of the hook names are self explanatory, I'll outline the lifecycle
below so you can get an idea of when these files are called:

| Hook | Args | Description |
| --- | --- | --- |
| **get_env_details** | env name | Called when `workon` is run with no arguments and displays the output in an env list |
| **initialize** | - | Called when `virtualenvwrapper.sh` is sourced from the `.bashrc` file and can be used to adjust global settings |
| **postactivate** | - | Called AFTER an environment is enabled |
| **postdeactivate** | - | Called AFTER an environment is disabled |
| **postmkproject** | - | Called AFTER a project is created |
| **postmkvirtualenv** | - | Called AFTER a virtualenv is created |
| **postrmvirtualenv** | env name | Called AFTER a virtualenv is deleted |
| **preactivate** | env name | Called BEFORE an environment is enabled |
| **predeactivate** | - | Called BEFORE an environment is disabled |
| **premkproject** | name of new project | Called BEFORE a project is created |
| **premkvirtualenv** | name of new env | Called BEFORE a virtualenv is created |
| **prermvirtualenv** | env name | Called BEFORE a virtualenv is deleted |

## Commands

With virtualenvwrapper, we get access to the following commands:

| Command | Args | Description | Reference |
| --- | --- | --- | --- |
| **mkvirtualenv** | env name | Create a new virtualenv | [Link](https://virtualenvwrapper.readthedocs.io/en/latest/command_ref.html#mkvirtualenv) |
| **mktmpenv** | - | Create a new temporary virtualenv | [Link](https://virtualenvwrapper.readthedocs.io/en/latest/command_ref.html#mktmpenv) |
| **lsvirtualenv** | - | List all virtualenvs available | [Link](https://virtualenvwrapper.readthedocs.io/en/latest/command_ref.html#lsvirtualenv) |
| **showvirtualenv** | env name | Show details about a virtualenv | [Link](https://virtualenvwrapper.readthedocs.io/en/latest/command_ref.html#showvirtualenv) |
| **rmvirtualenv** | env name | Remove a virtualenv | [Link](https://virtualenvwrapper.readthedocs.io/en/latest/command_ref.html#rmvirtualenv) |
| **cpvirtualenv** | env name, target env name | Copy a virtualenv to another | [Link](https://virtualenvwrapper.readthedocs.io/en/latest/command_ref.html#cpvirtualenv) |
| **allvirtualenv** | command | Run a command in all virtualenvs | [Link](https://virtualenvwrapper.readthedocs.io/en/latest/command_ref.html#allvirtualenv) |
| **workon** | env name | Change working virtualenv | [Link](https://virtualenvwrapper.readthedocs.io/en/latest/command_ref.html#workon) |
| **deactivate** | - | Change back into host environment | [Link](https://virtualenvwrapper.readthedocs.io/en/latest/command_ref.html#deactivate) |
| **cdvirtualenv** | - | Change directories to the virtualenv path | [Link](https://virtualenvwrapper.readthedocs.io/en/latest/command_ref.html#cdvirtualenv) |
| **cdsitepackages** | - | Change directories to the virtualenv site-packages path | [Link](https://virtualenvwrapper.readthedocs.io/en/latest/command_ref.html#cdsitepackages) |
| **lssitepackages** | - | List virtualenv installed site-packages | [Link](https://virtualenvwrapper.readthedocs.io/en/latest/command_ref.html#lssitepackages) |
| **add2virtualenv** | Directory1, Directory2, ... | Add directories to the virtualenv Python path | [Link](https://virtualenvwrapper.readthedocs.io/en/latest/command_ref.html#add2virtualenv) |
| **mkproject** | project name | Create new virtualenv and associate a project directory to it | [Link](https://virtualenvwrapper.readthedocs.io/en/latest/command_ref.html#mkproject) |
| **setvirtualenvproject** | virtualenv path, project path | Associate a project directory to a virtualenv | [Link](https://virtualenvwrapper.readthedocs.io/en/latest/command_ref.html#setvirtualenvproject) |
| **cdproject** | - | Change directory to the project path | [Link](https://virtualenvwrapper.readthedocs.io/en/latest/command_ref.html#cdproject) |
| **wipeenv** | - | Remove packages installed in virtualenv | [Link](https://virtualenvwrapper.readthedocs.io/en/latest/command_ref.html#wipeenv) |
| **virtualenvwrapper** | - | View all commands available | [Link](https://virtualenvwrapper.readthedocs.io/en/latest/command_ref.html#virtualenvwrapper) |

## General Workflow

Our typical workflow with this setup begins by creating a new project.

```
$ mkproject NEW_APP
```

Once created we are moved into the new project directory by default. We can
exit this virtualenv and move back to our home directory with:

```
$ deactivate
```

When we want to work on the project again we run:

```
$ workon NEW_APP
```

The thing to remember when we do this is that activating a virtualenv will
place you in the isolated environment. All packages installed via pip will
exist only in this virtualenv. The Python version and specific interpreter will
exist only in this virtualenv as well. Deactivating it will go back to using
the host packages.

## Project Hooks

You may be wondering why some of the hooks we provided have pre-written scripts
in them. This is mainly gives us the ability to create project specific hooks.
You'll see specifically in the `postactivate` hook that we call a file called
`bin/postactivate` in the project if it exists after enabling the virtualenv.
This allows us to run any bash script after successfully entering the
environment.

For Django projects, this can be a perfect way to set environment variables
such as secret keys, passwords, or the setting module. For instance, a sample
`bin/postactivate` hook in a project could be:

```
export DJANGO_SETTINGS_MODULE=my_app.settings
```

Now when we do `workonn my_app` we will call our script after entering the
environment and we have access to the `django-admin` command instead of using
`python manage.py` to run Django commands.

In our `bin/postdeactivate` hook we can then unset the settings module like so:

```
unset DJANGO_SETTINGS_MODULE
```

This allows us to clean up after ourselves.

## Projects That Don't Use Python

While this is specific to Python packages we tend to use this workflow from
projects in Go, PHP, and anything else. It provides a sane way to call scripts
before and after lifecycle events settings environment variables, starting
services, or anything else we may need.

## Virtualenvwrapper Templates

One last thing we will mention is that virtualenvwrapper has the ability to call
templates from `mkproject`. While this can be advanced to develop, we borrowed
from the official Django template hook to create our own project template hook
[here](https://github.com/codezeus/virtualenvwrapper.codezeus). To learn more
about project template, read on the virtualenvwrapper
[docs](http://virtualenvwrapper.readthedocs.io/en/latest/developers.html?highlight=template#creating-a-new-template).
