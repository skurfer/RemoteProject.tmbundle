## About This Bundle

The Problem: You want to work with files in a TextMate project from the local filesystem (for speed, Subversion compatibility, and other reasons), but the files you're working on are useless unless they're on some remote machine. This is most common with web based projects where the code only works on a sever that has been configured in a particular way.

## Available Commands

### Upload Project Changes

This command will save all open files and push any local changes to the remote server.

### Get Remote Project

This command will save all open files and pull any remote changes from the remote server.

### Compare to Remote Project

This command will save all open files and tell you what differs between the local and remote copy of the project directory. No changes are made on either machine.

### Help

You're looking at it.

## Requirements

This bundle relies on `rsync`. `rsync` is installed by default under Mac OS X, but won't necessarily be on every remote machine. The following should be considered before using these commands.

  * Make sure a recent version of `rsync` is installed on the remote machine.
  * Make sure you can log in to the remote machine via `ssh` (or `rsh`) without being prompted for a password (using public key authentication, for example). By extension, you should then be able to `rsync` things with the remote machine without being prompted.
  * If the remote machine is also a Mac, you might consider adding the -E option to the `rsync` commands to be sure metadata is included. It's not there by default because many remote machines are running Linux or Solaris and -E makes `rsync` on Solaris poop in its pants.

## Getting Started

Here are two ways to turn a directory on a remote machine into a local TextMate project:  
(Note the trailing '/' on the `rsync` "source" path. It matters.)

  * Create the project and get the files

      1. Create and open the project in TextMate

            mkdir local_copy
            touch local_copy/blah.txt
            mate local_copy

      2. Set `WHERE_I_CAME_FROM` in the project to `user@server:/remote/path/`
      3. Click on `blah.txt` to open it (the current version of TextMate requires a file to be open before you can run a command)
      4. Run the "Get Remote Project" command (⌃⌘P → 2)
      5. Watch your files appear in the drawer
      6. Close and delete `blah.txt`
      7. Save the project

  * Get the files and create the project

      1. Make a local copy

            mkdir local_copy
            rsync -au user@server:/remote/path/ local_copy

      2. Open the directory as a project in TextMate

            mate local_copy

      3. Set `WHERE_I_CAME_FROM` in the project to `user@server:/remote/path/`
      4. Save the project

The first way is more fun.

## Warnings

### Deleted Files

If you delete a file from your project in one location, the deletion won't be replicated. You need to either remove the file in both places manually, or if using Subversion `svn update` the other end after the removal has been committed to the repository. There is a `--delete` option in `rsync`, but I consider it too dangerous to be on by default.

### Subversion

`rsync` seems to handle `.svn` directories just fine. Make sure they're the same version.

## Keyboard Shortcut

The commands in this bundle all share the same key equivalent: ⌃⌘P. According to the manual, project related shortcuts should use ⌃⌘, but I don't think this has been strictly followed which made it hard to find something. There are other commands using ⌃⌘P and since they have a more specific scope than these "remote project" commands, they will win in those scopes.
