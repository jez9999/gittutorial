[Back to index](README.md)
# Cherry-picking a commit
It can sometimes be very useful to 'cherry-pick' a commit from one branch and apply it to another.  So if you cherry-pick commit 123 from branch 'foo', and you're on branch 'bar', the changes made in commit 123 will be applied to branch 'bar' in a new commit that will be added to branch 'bar'.

## From another local repo
A common use-case for this functionality is that you have one local repo where you've applied some changes, and you want to apply them to another repo.  To do this, first go to the repo where you want to apply the changes.  Then, add the other local repo as a remote so you're able to cherry-pick from it:

```
$ cd newrepo

$ git remote -v
origin  https://github.com/user/newrepo.git (fetch)
origin  https://github.com/user/newrepo.git (push)

$ git remote add oldrepo /c/Development/oldrepo/.git

$ git remote -v
oldrepo C:/Development/oldrepo/.git (fetch)
oldrepo C:/Development/oldrepo/.git (push)
origin  https://github.com/user/newrepo.git (fetch)
origin  https://github.com/user/newrepo.git (push)
```

Now that the other local repo has been added (this repo is 'newrepo', and the one that was added which we're cherry-picking from is 'oldrepo'), fetch it.  Even though both repos are local, git sees them as entirely separate and so the remote still has to be fetched to the local.  You'll then see its branches in the local repo ('newrepo'):

```
$ git branch -a
* develop
  remotes/origin/HEAD -> origin/develop
  remotes/origin/develop

$ git fetch oldrepo
remote: Enumerating objects: 66, done.
remote: Counting objects: 100% (66/66), done.
remote: Compressing objects: 100% (28/28), done.
remote: Total 44 (delta 34), reused 17 (delta 14), pack-reused 0
Unpacking objects: 100% (44/44), 5.20 KiB | 98.00 KiB/s, done.
From C:/Development/oldrepo/
 * [new branch]        develop    -> oldrepo/develop

$ git branch -a
* develop
  remotes/oldrepo/develop
  remotes/origin/HEAD -> origin/develop
  remotes/origin/develop
```

If you want, you can now find the commit hash you want to cherry-pick by looking at the log of 'oldrepo':

```
$ git log remotes/oldrepo/develop
commit a16ccbffde0edb18f65dba9f6d214404b5213a93 (oldrepo/develop)
Author: Jack Smith <jack@smith.com>
Date:   Tue Nov 1 11:14:36 2022 +0000

    Added some extra stuff.

commit 648f6906ccc7f0dad107f3405cbe02ad32442b53
Author: Joe Bloggs <joe@bloggs.com>
Date:   Sun Oct 30 22:22:01 2022 +0100

    Did some important work.

```

Say you want to cherry-pick the 'Did some important work.' commit into the current repo/branch.  You can now do this with the following, as you're easily able to copy/paste the commit hash for the commit you've identified you want to cherry-pick:

```
$ git cherry-pick 648f6906ccc7f0dad107f3405cbe02ad32442b53
[develop 648f6906] Did some important work.
 Author: Joe Bloggs <joe@bloggs.com>
 Date: Sun Oct 30 22:22:01 2022 +0100
 10 files changed, 191 insertions(+), 1 deletion(-)
```
