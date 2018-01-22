[Back to index](README.md)
# Deleting a branch
After you're done with a branch (your modifications have been pulled in to another branch, or you don't need them anymore) you should delete it to avoid lots of branches cluttering things up.

Firstly, deleting a branch locally is pretty simple:
```
git branch -d [branchName]
```

If the branch has been pushed to a remote repo, you'll want to delete the remote version of the branch too.  This can be done thus:
```
git push [remoteName] :[branchName]
```
In the above, `[remoteName]` will likely be `origin`, as that is the default alias set up in your local repo to track the remote repo it was cloned from.  That colon in front of the branch name actually tells Git to delete the branch.

Pruning deleted remote branches is probably something you'll want to do as well.  When others delete a branch remotely as described above, you're unlikely to want to keep it around on your local repo.  A `git pull` will *not* cause their remote references to be deleted on your local repo.  To do that, you need to run:
```
git remote prune [remoteName]
```

To avoid having to remember this command, you can setup a handy alias to prune branches for the remote name you usually (or always) use by adding a section like the following to your global Git config:

```
[alias]
	doprune = remote prune origin
```
