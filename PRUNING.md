[Back to index](README.md)
# Pruning / housekeeping
Every now and then, you'll want to go through a 3-stage process of pruning/housekeeping feature branches, bugfix branches, old tags, etc. that you don't need anymore and want to remove in order to reduce cruft building up in the repo.

It is a little involved, and it's best to do it in the following 3 stages, in the following order:

## 1. Prune remote branches and tags
Your remote repo provider might provide a handy web UI to just list branches and tags, and delete the ones you don't want from a web browser.  If so, that's the easiest way to do this.  Failing that, you can do it from the commandline.  First, `git pull` to get the latest remote branches and tags.  Then, list the remote branches and tags, and push deletes for the ones you want to get rid of:

```
$ git branch -r
  origin/master
  origin/my-new-feature

$ git tag
Release1.88(28)
Release1.90(30)

$ git push origin --delete "my-new-feature"
To https://bitbucket.org/my/repo.git
 - [deleted]           my-new-feature

 $ git push origin --delete "Release1.88(28)"
To https://bitbucket.org/my/repo.git
 - [deleted]           Release1.88(28)
```

## 2. Prune local tags and tracking branches
The pruning of local tags and tracking branches can be automated and set as a handy alias, `git doprune` (see [Deleting a branch](DELETEBRANCH.md) for more info on this).  Run this command.

## 3. Prune local non-tracking branches
If you've checked out any local non-tracking branches, for example in order to make modifications to that branch locally and push the changes, you'll need to manually delete these local branches; for example, assuming this was done with the `my-new-feature` branch whose remote branch was deleted and pruned above:

```
$ git branch
* master
  my-new-feature <-- (non-tracking branch still here!)

$ git branch -d my-new-feature
Deleted branch my-new-feature (was d175c79).

$ git branch
* master
```

Note that using `git branch -d` instead of `git branch -D` here will keep things safe because the branch will only be deleted if git detects it's been merged.  If you really want to delete the branch anyway, you can use `git branch -D [branchname]`.

A relatively safe automated way of deleting your local non-tracking branches is first to checkout `master`:

```
$ git checkout master
Switched to branch 'master'
Your branch is up to date with 'origin/master'.
```

Then to run the following command from bash shell (it works even in Windows' git bash shell):

```
$ git branch --merged | egrep -v "(^\*|master|release)" | xargs git branch -d
```

This will soft-delete non-tracking branches (ie. only delete ones that are fully merged), except for ones that are filtered out by the `egrep` part; these are "long-lived" branches you want to keep around.  You may have a few of these - such as `release` in the above example - and of course you'll always have `master`, which should be kept around.
