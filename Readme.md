ak-git-tools
============

This is a collection of tools I use with Git that make life easier.

Installing
----------

For a basic installation, run the installation script, like this:

    sudo ./install-ak-git-tools

Using the above command as-is would install these tools into `/usr/bin`.  The
installation prefix (by default `/usr`) can be overridden by passing something
as the first argument.

The installation also adds an uninstaller, called `__uninstall-ak-git-tools`.
Upon installation, the uninstaller is automatically ran first.  To uninstall
manually, simply use the generated uninstaller:

    __uninstall-ak-git-tools /usr

Additionally, some Git configurations can be set in the global domain.  To set
them, run:

    ./set-ak-global-config

There is no uninstaller for the configurations set by `set-ak-global-config`.

Contents
--------

### Navigating the commit graph

Note: Some of these commands are aliases.  Running `install-ak-git-tools` does
not include the aliases; run `set-ak-global-config` to set the aliases.

* `git graph ...`: (alias) A primitive version of `gitk` for use in a terminal.

### Working with the working directory

* `git scrub ...`: (alias) The same as `git clean`, except instead of running in
  the current working directory, this starts in the top-level of the
  superproject, and is recursive across all submodules.  To blow away all
  untracked files everywhere in the entire superproject, run:

        git scrub -xfd

* `git su ...`: (alias) Update all submodules to the commit ID at which they are
  intended to be checked out according to HEAD in the current repository.  To
  initialize all submodules recursively starting with the current repository,
  run:

        git su --init

### Running commands

* `git env ...`: Run the given command at the top of the current repository.
  Add `--recursive` to also run in all submodules.

* `git tenv ...`: Run the given command at the top of the superproject.  Add
  `--recursive` to also run in all submodules.

* `git exec-repo-tool ...`: Run the given command in the current working
  directory with the given arguments.  The command may live in any `tools`
  directory in any repository in the current superproject.

### Manipulating the objects database

* `git disconnect-alternates`: If the current repository is using alternates,
  run `git repack -a` and delete the `alternates` file.

### git-repocache

A collection of scripts for cloning faster.  See `git-repocache` (below).

`git-repocache`
---------------

The "repocache" is a single bare repository that contains many remotes, and is
used to speed up cloning by reducing the number of objects that must travel over
the network.  If you're interested in the technicalities, read up on the
`--reference` parameter to git-clone(1).

To use `git-repocache`, you must set the property `repocache.path` in Git's
global or system configuration.

The following is a list of all the commands supported by `git-repocache`:

* `git repocache`: Invoke the given Git command in the repocache.  Example:
  
        git repocache remote add git git://github.com/git/git.git

* `git repocache-clone`: Performs a normal clone, but uses the repocache.
  Example:

        git repocache-clone git://github.com/git/git.git

  Upon successful exit, you should then run `git disconnect-alternates` inside
  the newly cloned repository to prevent the repository from becoming corrupt.

* `git repocache-clone-by-name`: Performs a normal clone, but instead of
  providing the URI, you provide the name of the remote in the repocache.  This
  command then looks up the URI used in the repocache, and clones using that
  URI.  Example:

        git repocache-clone-by-name git

  Upon successful exit, you should then run `git disconnect-alternates` inside
  the newly cloned repository to prevent the repository from becoming corrupt.

* `git repocache-env`: Runs the given command inside the repocache.  Example:

        git repocache-env pwd

* `git repocache-init`: Runs `git init` inside the repocache.  Creates the
  directory first if necessary.

* `git repocache-path`: Retrieves the path to the repocache from Git and prints
  it to stdout.

* `git repocache-submodule`: Invokes the given `git-submodule(1)` command in the
  current working directory.  Example:

        git repocache-submodule update --init --recursive

* `git repocache-update`: Performs a fetch in the repocache.
