# Git Tutorial
Git is a distributed version control system.  This means that every person working on a repository will have their own copy, which has either been created by themselves through `git init`, or "cloned" from another repository using `git clone`.

One important thing to note about distributed version control systems is that technically, no one repository is the "central" or "definitive" version.  Each repository can be thought of as a peer, equal to each other.  For example, this diagram could describe how Alice and Bob work together on a project using Git for source control:

(diagram1)

Quite simply, they push and pull changes directly to and from each other's machines.  In practice this very rarely happens, but it illustrates the non-centralized nature of Git.  Often, a service such as [Github](https://github.com/) or [Bitbucket](https://bitbucket.org/) is used to host a Git repository.  While these services provide a lot of convenience and an agreed central place to share code to, they are fundamentally no more "central" than any other clone of the repository; it's just that teams tend to agree that they will treat the version of the repository stored on the Github/Bitbucket/etc. server as definitive.  So this diagram could describe how Alice and Bob work together on a project through Github or Bitbucket:

(diagram2)

However Alice could just as easily set up her Git configuration to pull updates from Bob's machine, rather than Github or Bitbucket.

# Cloning a repository
This document will assume that Github's copy is being considered by all team members as the definitive version of the repository being used for source control.  The exact means for initializing this repository on a team member's machine depends on which tool is being used, but fundamentally it requires "cloning" the Github repository.  This means that a complete copy of the Github repository will be taken and stored locally on the user's machine.  The way to do this on the Git command line is:

```
git clone https://github.com/[user]/[repoName].git
```

... where `[user]` is the Github username under which the repository is stored, and `[repoName]` is the name of the repository.  This command will create a directory with the same name as `[repoName]`.  Inside the directory will be a `.git` subdirectory containing the Git repository data, and a checkout of the currently active branch of the source repository (usually `master`).  The way to clone a repositry in Visual Studio is via a GUI of course but follows the same principle:

![VS clone GUI](vs-clone.png)

Once the repository is cloned from Github, Git will have automatically set it up to "push" to and "pull" from Github's repository.  This means that by default, a `git push` will try to update Github's repository with changes made to your local one, and a `git pull` will try to update your local repository with changes made to Github's one.  Again, Visual Studio follows the same principle with it's GUI allowing the user to "push" and "pull" changes:

![VS push/pull GUI](vs-pushpull.png)
