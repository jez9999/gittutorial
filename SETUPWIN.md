[Back to index](README.md)
# Setting up Git for Windows
First, download the official Git for Windows client from the official website: https://git-scm.com/

During installation, it's recommended that you "Use Git from Git Bash only", which will avoid accidental running of Git commands from a non-Git commandline (generally you only need to run Git commands from the GIt commandline).  Also select MinTTY as the terminal emulator.

Once Git is installed, run "Git Bash", which will open up a Git commandline window (Bash shell) using the MinTTY terminal emulator.  You'll want to configure your Git global settings and it's highly recommended to set up merge and diff tools.

# Main config
Run the following command in the terminal emulator to get access to the Git global configuration:
```
git config -e --global
```
The default editor that will be used to edit this configuration is "Vim".  You'll need to press "i" to enter "insert mode" which allows you to insert/delete characters normally.  When finished editing, press "esc" to exit insert mode, then type `:wq` and hit enter which `w`rites the config, then `q`uits the program and returns to the commandline.

Your initial config should look pretty empty.  You'll want to set it up to look something like this:
```
[user]
	name = Joe Bloggs
	email = joebloggs@[yourDomain].com
[push]
	default = current
[core]
	autocrlf = input
```
... obviously putting your actual name and e-mail address into the `[user]` section.

The `[push]` setting will mean that only the branch you're currently on gets pushed on a `git push` by default, which is almost always the desired behaviour, and the `[core]` setting will mean that `LF`-style newlines get checked into the repo, enforcing newline consistency.  The only editor that doesn't support these line endings is Windows Notepad, so never use that.  It's terrible, anyway.

# Merge and diff tools
The built-in merge and diff tooling in Git is commandline-based and very basic.  It's a good idea to set up merge and diff tools for performing these operations in Git.

Git comes with various merge and diff tools preconfigured, and if you're using one of these tools all you need to do is tell Git in a config setting which tool you're using.  One such tool is Perforce's P4Merge, which is highly recommended as it has a nice GUI and supports 3-way merging.

Firstly, install P4merge itself.  An installer that is compatible with 64-bit Windows 7 or later can be found in this repo at `setups/p4vinst64.exe`.

In order to just install P4Merge and not other unnecessary components, when installing the "Perforce Visual Components for 64-bit systems", deselect every feature ("This feature will not be installed...") except for "Visual Merge Tool (P4Merge)".

# Credentials
TODO: write...

# Interacting with Git
Once Git for Windows is set up, there are a number of ways to interact with Git:
- Git bash shell

  The commandline shell that we've just been using is the main way to interact with Git.  The `git` command has lots of functionality which is widely documented on the web.  You can also get help from the built-in documentation with `git help` or for a particular command with `git help [command]`.
- Git GUI

  Git for Windows comes bundled with a basic UI for creating and managing repositories which can be started with the command `git gui`.
- Gitk

  Also bundled is a command that can be run from the Git bash shell, `gitk`.  This opens a GUI that shows the history of the current branch and provides commands for checking out a specific version.  It has to be run from within the repository you want to show history for.
- Visual Studio Git integration

  This obviously isn't part of Git for Windows, but from Visual Studio 2013 onwards, Visual Studio itself comes with Git functionality bundled into its user interface, which can be used for creating and managing repositories if you'd prefer.
