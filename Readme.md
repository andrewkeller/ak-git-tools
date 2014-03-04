ak-git-tools
============

This is a collection of tools I use with Git that make life easier.

Contents
--------

### Running commands

* `git env ...`: Run the given command at the top of the current repository.
  Add `--recursive` to also run in all submodules.

* `git tenv ...`: Run the given command at the top of the superproject.  Add
  `--recursive` to also run in all submodules.

### Manipulating the objects database

* `git disconnect-alternates`: If the current repository is using alternates,
  run `git repack -a` and delete the `alternates` file.

### git-repocache

A collection of scripts for cloning faster.  See `git-repocache` (below).

`git-repocache`
---------------

The "repocache" is a single bare repository that contains many remotes, and is
used to speed up cloning by reducing the number of objects that must travel over
the network.  The path to the repocache is stored in one of any configuration
file owned by Git.

If you're interested in the technicalities, read up on the `--reference`
parameter to git-clone(1).

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

* `git repocache-exec`: Runs the given command inside the repocache.  Example:

        git repocache-exec pwd

* `git repocache-init`: Runs `git init` inside the repocache.  Creates the
  directory first if necessary.

* `git repocache-path`: Retrieves the path to the repocache from Git and prints
  it to stdout.

* `git repocache-submodule`: Invokes the given `git-submodule(1)` command in the
  current working directory.  Example:

        git repocache-submodule update --init --recursive

* `git repocache-update`: Performs a fetch in the repocache.
