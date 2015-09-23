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
```
