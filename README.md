# proj

A multi-project workspace helper.

You can use proj to manage workspaces that have several projects, allowing you
to handle many projects with only one command.

- Update all your projects with one command
- Use different project configuration for the same workspace

# Installing

Clone this project:

```sh
git clone https://github.com/msramos/proj.git $HOME/.proj
```

Then add `$HOME/.proj` to your `PATH`:

# Concepts

**Specs**

They describe how to handle a project in the workspace. The name of the project
is the name of the spec folder. Each property is stored in one file.

```
specs
+- firefox
|  +-- repo
|  +-- branch
+ linux
  +-- repo
```

First you configure your specs and then `sync` so changes will be made on the
workspace.

**Workspace**

This is the directory where your source code lives, where it will be cloned and
updated.

# Dynamic configuration and specs

By default, `proj` will store all configuration and specs on `$HOME/.proj`, but
you can change this behavior by creating a `.proj` on any directory you want to.
When doing this, `proj` will store the configs and specs in this new path.

# Usage

## proj config
Configures a `proj` property.

**proj config workspace**
Set the value for a property.

```sh
# Example
$ proj config workspace /home/john_snow/Workspace
Key 'workspace' set to '/home/john_snow/Workspace'
```

Available properties are: `workspace`

**proj config rm|del \<key\>**
Deletes a `proj` property
```sh
# Example
$ proj config rm workspace
Key 'workspace' deleted
```

## proj spec

Manage project specs.

**proj spec add \<git repository\> \<name\>**

Add a new project spec using the specified git repository. You can optionally
give it a custom name.

```sh
# Example
$ proj spec add https://github.com/msramos/proj.git
Project 'proj' added using remote 'https://github.com/msramos/proj.git'
```

**proj spec ls**

List all available specs

```sh
# Example
$ proj spec ls
linux		firefox	  jdk
```

**proj spec del|rm \<spec name\>**

Deletes a spec

```sh
# Example
$ proj spec rm my_project_name
Spec for project 'my_project_name' deleted!
```

**proj spec import**

Reads all projects inside a directory and for each one, check if they are
versioned using git and that a spec with the same name does not exist. If these
two criteria matches, create a new spec for the project using its remote URL.

```sh
# Example
$ proj spec import ~/GitProjects
Imported 'some' (https://github.com/some_user/some.git)
Imported 'random' (https://github.com/some_user/some.git)
Imported 'project' (https://github.com/some_user/project.git)
```

## proj sync

Synchronizes the workspace. Any action from this command will perform changes on
the source code present in the workspace. It reads the project specs before 
performing changes.

A spec synchronization can be:

- `git clone`, if the project does not exist
- `git checkout` and a `git pull`, if the project already exists

**proj sync**

Synchronize all projects that already exist on the workspace.

```sh
# Example
$ proj sync
# lots of git output here
```

**proj sync \<project name\>**

Synchronizes the specified project spec.

```sh
# Example
$ proj sync firefox
# lots of git output here
```

**proj sync --all**

Synchronizes ALL project specs.

```sh
# Example
$ proj sync --all
# lots of git output here
```

## proj branch

Manage spec branches. This will NOT make any changes on the workspace, it only
updates the spec branch.

**proj branch set \<project name\> \<branch\>**

Changes the spec branch.

```sh
# Example
$ proj branch set linux 5.3.2
Project spec 'linux' is configured to use branch '5.3.2'.
```

The actual branch of the project (in the workspace) will only change after a
synchronization.

**proj branch reset \<project name\>**

Resets the spec branch to `master`

```sh
# Example
$ proj branch reset firefox
Project spec 'firefox' is configured to use branch 'master'.
```

The actual branch of the project (in the workspace) will only change after a
synchronization.


**proj branch show \<project name\>**

Displays the current configured branch for the given project spec

```sh
# Example
$ proj branch show firefox
Project spec firefox is on 'testing'
```

**proj branch show-all**

Displays the current configured branch for all existing specs

```sh
# Example
$ proj branch show-all
firefox:testing
linux:5.3.2
```