## About This Bundle

### The Problem

You want to work with files in a TextMate project on the local filesystem (for speed, Subversion compatibility, and other reasons), but the files you're working on are useless unless they're on some remote machine. This is most common with web based projects where the code only works on a sever that has been configured in a particular way.

### Who This Bundle Is For

This bundle is meant to support a workflow where all (or almost all) changes are made locally using TextMate, while all (or almost all) viewing/running/testing of those changes will be done on a remote machine (such as a web server).

## Available Commands

### Upload Project Changes

This command will save all open files and push any local changes to the remote server.

It is listed first because it will be the most often used.

### Get Remote Project

This command will save all open files and pull any changes from the remote server.

Generally, you should only need to run this once. After a project is pulled down, you can make all changes locally and use "Upload Project Changes" to push changes to the remote server.

### Compare to Remote Project

This command will save all open files and tell you what differs between the local and remote copy of the project directory. Other than saving files locally, no changes are made on either machine.

This is a good way to verify that both copies are identical before you start making changes. In particular, you should make sure nothing has been changed on the remote end before making changes locally and copying them over.

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

## Keyboard Shortcut

The commands in this bundle all share the same key equivalent: ⌃⌘P. According to the manual, project related shortcuts should use ⌃⌘, but this has not been strictly followed. There are other commands using ⌃⌘P and since they have a more specific scope than these "remote project" commands, they will take precedence in those scopes. If this becomes a problem for you, I encourage you to contact those bundle developers that have ignored the Manual's conventions.

## Warnings

### Deleted Files

If you delete a file from your project in one location, the deletion won't be replicated. You need to either remove the file in both places manually, or if using Subversion `svn update` the other end after the removal has been committed to the repository. There is a `--delete` option in `rsync`, but I consider it too dangerous to enable by default. (It doesn't remove files that were deleted from the source end. It removes any files from the destination end that aren't present on the source end, which could simply be new files on the destination end.)

### Subversion

If you want to use this bundle to push/pull a Subversion working copy, `rsync` seems to handle this just fine. **Just make sure the working copies are kept in the same format on the remote machine as they are on your machine.** Generally, this means running the same version of the `svn` client in both places. For example, if you are using Subversion 1.4 on your local machine and run `svn update`, then push the updated copy to a remote machine running Subversion 1.3, you will no longer be able to use `svn` on the remote machine to manipulate that working copy (until it is upgraded).

Some people may with to forgo the "Upload Project Changes" command and instead use `svn update` to move changes from one copy to the other. That could work in some cases, but…

  * Changes have to be committed on one side before they can be used to update the other. Many people don't like to commit changes unless they are known to work, and in this context, changes have to be pushed to the remote machine and tested before that can be determined.
  * You would have to keep a connection open to the remote machine to run `svn` commands there.
