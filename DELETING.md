[Back to index](README.md)
# Deleting file(s) from a repo's history
Obviously deleting a file in a commit is straightforward, but deleting a file from the entire history of a Git repo may be necessary if the file is too big and you want it removed to reduce the repo size or to allow it to be checked into Github (for example) if the file is over 100MB big.

This should be a relatively rare operation, and it requires going through every commit in the repo tree and performing an operation to remove the file(s).  Each commit that is modified will have a new commit hash, so after the operation, the repo will need to be force-pushed if it's in a public location and others working on it will need to clone it from scratch.

So, to do this operation you'll want to use the `git filter-branch` command.  The following will get rid of a particular ZIP file from the entire repo history, all the way up to where `HEAD` is pointing (if it's pointing at the latest `master` then it will be removed at all points along the tree that `master` is a descendant of):

```
git filter-branch --tree-filter 'rm -f ./SomeDirectory/FileToDelete.zip' --prune-empty HEAD
```

You'll see something like this for each commit being worked on:

```
Rewrite c429a72e109a20a583fe7a7c039c44bd50e3d5e6 (143/1885)
```

... and finally the message `Ref 'refs/heads/master' was rewritten`.  Each commit is checked out starting from the first commit in the tree, and the `rm` command is run from the root of the currently checked-out commit each time; then that commit is re-committed with any changes the command has made to it.  In this case, `rm` will have removed that file to delete in each commit.  The `-f` argument needs to be used with `rm` assuming the file didn't exist in every commit; for the commits where the file hadn't yet been checked in and therefore didn't exist, `-f` will stop `rm` from failing because the file doesn't exist, and will just carry on.  The `--prune-empty` param is worth adding as it means that if any of the commits end up being empty (for example, a commit that just adds `FileToDelete.zip` will now be an empty commit), those commits will be pruned.  Probably desirable instead of having empty commits sitting around.

Note that the above is very slow, as it requires checking out each commit to the filesystem, resulting in lots of I/O activity.  It's more useful if you need to *modify* the contents of file(s) on each commit for some reason.  For something as simple as just deleting file(s), it's quicker to use `--index-filter` which can just operate on the staging cache of each commit rather than checking that cache out to the filesystem for each commit.  So let's try deleting a whole directory of unwanted files using `--index-filter`:

```
git filter-branch --index-filter 'git rm -rf --cached --ignore-unmatch ./Unwanted/Directory' --prune-empty HEAD
```

Again you'll see the likes of:

```
Rewrite ecc2932c66c848f849398ed47d9689903d2f0579 (14/1885)
```

... for each commit being worked on.  This operation should go significantly faster than the `--tree-filter` one (though it may still take its time, depending on how many commits there are in the repo).  By the end of the operation, the entire repo history should be wiped clean of anything that added, modified, or deleted files to or from the unwanted directory, and the directory should be gone.  Note that if the most recent commit would become an empty commit because of this operation (if it just added a file to the unwanted directory for instance), the `--prune empty` param will even prune that commit and move the `master` pointer to the commit before it.

This time round, in the `--index-filter` command, we're using Git's `git rm` command as we're working with its staging cache and not the regular filesystem.  Telling Git to operate on the staging cache is also why we need to use the `--cached` argument with `git rm`.  In addition `--ignore-unmatch` allows `git rm` to succeed even if no files matched the file patch specified (ie. the directory didn't exist yet in the currently checked-out commit being worked on).

# Cleaning up refs
As this is potentially a dangerous operation, Git doesn't actually completely remove the old commits when you do this; it stores them in `.git/refs/original`.  Once you've inspected the results of the `filter-branch` operation and you're happy with them, you'll want to remove the backup "original" ref(s) with something like the following (in the case of the above example):

```
git update-ref -d refs/original/refs/heads/master
```

... for the master branch.  Or, if you did `filter-branch` on multiple refs/branches, and you want to wipe out all the backups:

```
git for-each-ref --format="%(refname)" refs/original/ | xargs -n 1 git update-ref -d
```

Note that this command will spit out `update-ref` "usage" info if there aren't actually any refs sitting in `refs/original`.

So that removes the references to the old commits.  There's still one last thing potentially to do.  The data for those large files is still there in the `.git` directory.  In time it will automatically be garbage collected by Git but if you want to immediately see the reduced space taken up having deleted the large files from the repo's history, as again you may wish to do in the above example, you need to forcibly expire and prune the content with:

```
git reflog expire --expire=now --all
git gc --prune=now
```
