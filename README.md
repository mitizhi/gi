# Git Issues

This is a minimalist distributed issue management system based on Git.
It has the following advantages over other systems.

* **No backend**
  You can install and use _gi_ with a single shell script.
  There's no need for a server or a database back-end, and the corresponding
  problems and requirements for their administration.
* **Distributed asynchronous management**
  Anyone can add, comment, and edit issues without requiring online access
  to a centralized server.
  There's no need for online connectivity; you can pull and push issues
  when you're online.
* **Transparent text file format**
  Issues are stored as simple text files, which you can view, edit, share, and
  backup with any tool you like.
  There's no risk of loosing access to your issues because a server has
  failed.
* **Git based**
  Issues are changed and shared through Git.
  This provides _gi_ with a robust, efficient, portable, and widely available
  infrastructure.
  It allows you to reuse your Git credentials and infrastructure, allows
  the efficient merging of work, and also provides a solid audit trail
  regarding any changes.
  You can even use Git and command-line tools directly to make sophisticated
  changes to your issue database.

## Installation
Simply copy the `gi` shell script somewhere in the system's path.
If you have administrative access you can install it with
`sudo install gi.sh /usr/local/bin/gi`.
For your personal use,
assuming that the directory `~/bin` exists and is in your path,
you can install it with `install gi.sh ~/bin/gi`.
You can even put `gi` in your project's current directory and run it from there.

The `gi` script has been tested on: Debian GNU/Linux, FreeBSD, Mac OS X, and
Cygwin.
If you're running `gi` on another system, run the `test.sh` script to verify
its operation, and (please) update this file.

## Use
You use _gi_ with the following sub-commands.

* `gi init`: Create a new issues repository in the current directory.
* `gi clone`: Clone the specified remote repository.
* `gi new`: Create a new open issue with the specified summary.
* `gi list`: List the issues with the specified tag.
  By default this lists issues that are tagged as `open`.
* `gi show`: Show specified issue (and its comments with `-c`).
* `gi comment`: Add an issue comment.
* `gi tag`: Add (or remove with `-r`) a tag.
* `gi assign`: Assign (or reassign) an issue to a person.
  The person is specified with his/her email address.
  The form `@name` or `name@` can be used as a shortcut, provided it
  uniquely identifies an existing assignee or committer.
* `gi attach`: Attach (or remove with `-r`) a file to an issue.
* `gi watcher`: Add (or remove with `-r`) an issue watcher.
* `gi close`: Remove the `open` tag from the issue, marking it as closed.
* `gi edit`: Edit the specified issue's summary or comment.
* `gi log`: Output a log of changes made
* `gi push`: Update remote repository with local changes.
* `gi pull`: Update local repository with remote changes.
* `gi git`: Run the specified Git command on the issues repository.

Issues and comments are specified through the SHA hash associated with the
commit that opened them.

## Internals
* All data are stored under `.issues`.
* A `.git` directory contains the Git data associated with the issues.
* A `config` file with configuration data.
* A `templates` directory with message templates.
* An `issues` directory contains the individual issues.
* Each issue is stored in a directory named `issues/xx/xxxxxxx...`,
  where the x's are the SHA of the issue's initial commit.
* Each issue can have the following elements in its directory.
  * A `description` file with a one-line summary and a description of the issue.
  * A `comments` directory where comments are stored.
  * An `attachments` directory where issue's attachments are stored.
  * A `tags` file containing the issue's tags, one in each line.
  * A `watchers` file containing the emails of persons to be notified when the issue changes (one per line).
  * An `assignee` file containing the email for the person assigned to the issue.

## Project status
This is work in progress.
The system has achieved the status of a minimal viable prototype:
it can be used to manage issues.

## Contributing
Contributions are welcomed through pull requests.
Before working on a new feature please look at open issues, and if no
corresponding issue is open, create one to claim priority over the task.
Issues are managed with gi through the [gi-issues](git@github.com:dspinellis/gi-issues.git) repo.

## Example session
You can view a video of the session on [YouTube](https://youtu.be/zrPM5kNQcKU).

```
$ gi init # Initialize issue repository
Initialized empty Issues repository in /home/dds/src/gi/.issues
$ gi new -s 'New issue entered from the command line'
Added issue e6a95c9
$ gi new # Create a new issue (opens editor window)
Added issue 7dfa5b7
$ gi list # List open issues
7dfa5b7 An issue entered from the editor
e6a95c9 New issue entered from the command line
$ gi comment e6a95c9 # Add an issue comment (opens editor window)
Added comment 8c0d5b3
$ gi tag e6a9 urgent # Add tag to an issue
Added tag urgent
$ gi tag e6a9 gui crash # Add two more tags
Added tag gui
Added tag crash
$ gi tag -r e6a9 urgent # Remove a tag
Removed tag urgent
$ gi assign e6a9 joe@example.com # Assign issue
Assigned to joe@example.com
$ gi watcher e6a9 jane@example.com # Add issue watcher
Added watcher jane@example.com
$ gi list gui # List issues tagged as gui
e6a95c9 New issue entered from the command line
$ # Push issues repository to a server
$ gi git remote add origin git@github.com:dspinellis/gi-example.git
$ gi git push -u origin master
Counting objects: 60, done.
Compressing objects: 100% (50/50), done.
Writing objects: 100% (60/60), 5.35 KiB | 0 bytes/s, done.
Total 60 (delta 8), reused 0 (delta 0)
To git@github.com:dspinellis/gi-example.git
 * [new branch]      master -> master
Branch master set up to track remote branch master from origin.


$ # Clone issues repository from server
$ gi clone git@github.com:dspinellis/gi-example.git my-issues
Cloning into '.issues'...
remote: Counting objects: 60, done.
remote: Compressing objects: 100% (42/42), done.
remote: Total 60 (delta 8), reused 60 (delta 8), pack-reused 0
Receiving objects: 100% (60/60), 5.35 KiB | 0 bytes/s, done.
Resolving deltas: 100% (8/8), done.
Checking connectivity... done.
Cloned git@github.com:dspinellis/gi-example.git into my-issues
$ gi list # List open issues
7dfa5b7 An issue entered from the editor
e6a95c9 New issue entered from the command line
$ gi new -s 'Issue added on another host' # Create new issue
Added issue abc9adc
$ gi push # Push changes to server
Counting objects: 7, done.
Compressing objects: 100% (6/6), done.
Writing objects: 100% (7/7), 767 bytes | 0 bytes/s, done.
Total 7 (delta 0), reused 0 (delta 0)
To git@github.com:dspinellis/gi-example.git
   d6be890..740f9a0  master -> master
$ gi show 7dfa5b7 # Show issue added on the other host
issue 7dfa5b7f4591ecaa8323716f229b84ad40f5275b
Author: Diomidis Spinellis <dds@aueb.gr>
Date:   Fri, 29 Jan 2016 01:03:24 +0200
Tags:   open

    An issue entered from the editor

    Here is a longer description.
$ gi show -c e6a95c9 # Show issue and coments
issue e6a95c91b31ded8fc229a41cc4bd7d281ce6e0f1
Author: Diomidis Spinellis <dds@aueb.gr>
Date:   Fri, 29 Jan 2016 01:03:20 +0200
Tags:   open urgent gui crash
Watchers:       jane@example.com
Assigned-to: joe@example.com

    New issue entered from the command line

comment 8c0d5b3d77bf93b937cb11038b129f927d49e34a
Author: Diomidis Spinellis <dds@aueb.gr>
Date:   Fri, 29 Jan 2016 01:03:57 +0200

    First comment regarding the issue.


$ # On the original host
$ gi pull # Pull in remote changes
remote: Counting objects: 7, done.
remote: Compressing objects: 100% (6/6), done.
remote: Total 7 (delta 0), reused 7 (delta 0), pack-reused 0
Unpacking objects: 100% (7/7), done.
From github.com:dspinellis/gi-example
   d6be890..740f9a0  master     -> origin/master
Updating d6be890..740f9a0
Fast-forward
 issues/ab/c9adc61025a3cb73b0c67470b65cefc133a8d0/description | 1 +
 issues/ab/c9adc61025a3cb73b0c67470b65cefc133a8d0/tags        | 1 +
 2 files changed, 2 insertions(+)
 create mode 100644 issues/ab/c9adc61025a3cb73b0c67470b65cefc133a8d0/description
 create mode 100644 issues/ab/c9adc61025a3cb73b0c67470b65cefc133a8d0/tags
$ gi list # List open issues
7dfa5b7 An issue entered from the editor
abc9adc Issue added on another host
e6a95c9 New issue entered from the command line
```
## Related work
* [deft](https://github.com/npryce/deft) developed in 2011 is based on
  the same idea.
  It requires Python and offers a GUI.
* [Bugs Everywhere](http://www.bugseverywhere.org/), also written in Python, supports many version control backends and offers a web interface.
* [git-appraise](https://github.com/google/git-appraise) is a distributed
  code review system for Git repos based again on Git.
* [Fossil](http://fossil-scm.org/) is a distributed version control software that also supports issue tracking and a wiki. It runs as a single executable.
