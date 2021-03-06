# hyn/git-helpers

This package is meant to be used in your terminal. And allows
working with lots of subdirectories containing git repositories.

## Installation

```bash
composer global require hyn/git-helpers
```

> Make sure to place the ~/.composer/vendor/bin directory in your PATH so the gh executable can be located by your system.

## Usage

You can now globally use the command `gh`, eg:

```bash
gh status
```

## Use cases

This package is especially helpful in case you manage your development packages
with a [repository path configuration](https://getcomposer.org/doc/05-repositories.md#path) in composer.

If you create a workbench directory containing all your development packages. You can
set your composer file to point to these packages:

```json
{
    "require": {
        "vendor/package": "*@dev"
    },
    "repositories": [
        {
            "type": "path",
            "url": "workbench/*/"
        }
    ]
}
```

The above will symlink your development packages from workbench to the vendor folder
automatically.

> Please note in the above repositories path configuration packages are expected immediately in the workbench directory, eg: `workbench/package`. A composer.json file should exist in `workbench/package/composer.json`.

### gh status

Now you can use the `gh status` command in the folder workbench to 
check the status of your git remote and local changes immediately.

The following is taken into account:

- A title consisting of:
    - The name of the package (in case of a json file).
    - The latest version (based on the latest tag).
- Open changes (uncommitted, new etc).
- Number of commits since last tag.
- Number of commits not pushed.

### gh pull

The command `gh pull` allows you to mass pull all subdirectories to get you the latest changes.

You can use the optional argument to match subdirectories against a certain (regular expression) match; eg:

```bash
gh pull ^feature-
```
To pull only directories starting with `feature-`.

Please note this command will not pull changes from the remotes if uncommitted changes have been detected. 
Unless you:

- force pulling using the `--force` or shorthand `-f` command option. In most cases this is a very bad idea!
- stash and unstash your changes using `--stash` or `-s` command option.

### gh tag

The command `gh tag` can be used to add new tags to your repositories. When commits are detected since the latest
tag, you'll be asked to provide a new tag. You can also skip by leaving the input empty.

As with the pull command, you can use the optional argument to match only specific directories.
 
With the option `--up` you can specify what part of the previous version you'd like to see in the suggested input,
choose between major, minor or patch.

### gh changelog

With the `gh changelog` command you are able to generate changelogs between versions quite easily. Please do understand
this is a simple parsed commit history between two versions.

The command will loop through all your repositories, find all tags and will auto generate a diff log file in a specified
sub folder (use `--path` option to override the default value "changelogs"). Please note any existing files will be removed
before writing into them!

As with some of the other commands you can use the argument to filter the directories and use `-f` to force creating changelogs
if you have open changes in the directory.

### gh commit

The `gh commit` command allows you to commit similar changes across all repositories. A commit message may be specified using the `-m` command.

As with other commands, you can use the optional argument to match only specific directories.

### gh push

The `gh push` command pushes the current branch in all repositories to the remote.

As with other commands, you can use the optional argument to match only specific directories.
