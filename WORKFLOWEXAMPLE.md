[Back to index](README.md)
# Simple workflow example
The following is an example of a simple workflow that should minimize merge conflict issues.  It assumes that there is just one main `master` branch, and uses the principle that any (significant) changes will be made in a separate local branch, rather than committed directly to `master`.  The commandline for this workflow should look something like this:

```
# Update master to latest before starting work
$ git fetch origin master:master

# Branch off locally for our changes
$ git checkout -b myCoolFeature

# [... make some changes to implement our new feature and commit them to this branch ...]
$ git add -A
$ git commit -m "Implemented X"
# ...
$ git add -A
$ git commit -m "Implemented Y"
# ...
$ git add -A
$ git commit -m "Implemented Z"
```

At this point, we'll assume that the changes have all been committed, so we need to merge them back into the `master` branch.  The first thing to do is make sure the changes are based off the latest version of the master branch, and a good way to do this is to `rebase` the `myCoolFeature` branch on the latest `master` branch:

```
# Update master to latest before rebasing
$ git fetch origin master:master

# Rebase this branch on master
$ git rebase master
```

At this point, if there are any merge conflicts, they will need to be be resolved.  This can be done most easily with a good merge tool, by typing `git mergetool` if/when the conflict arises at the commandline (see [this section on setting up merge and diff tools](SETUPWIN.md#merge-and-diff-tools)).  Once the conflict is resolved, type `git rebase --continue` to continue the rebase.

Once the rebase is done, the feature branch just needs to be merged back into the master branch.  One way to do this is to merge them locally and then push the local master branch to remote, but if something like Github is being used, it may be easier to use Github's web interface to do this.  Simply push the feature branch to Github:

```
$ git push
```

Then login to [Github's web interface](https://github.com/) and navigate to the repo you're working with.  You should see a notification that your feature branch was recently updated, and a button to create a "pull request".  Pull requests are often used to discuss the changes being made before merging them in, but they can just be used to merge the changes immediately, too.  Create the pull request, then (assuming Github says that "Merging can be performed automatically"), just click "Merge pull request".  This will merge the changes back into the `master` branch and close the pull request.

Finally, to reflect the updated `master` branch on your local repo as well as delete the feature branch (now that it's been merged it's no longer needed), go back to the commandline and type:

```
# Update master to latest
$ git checkout master
$ git pull

# Delete old feature branch locally, and remotely
$ git branch -d myCoolFeature
$ git push origin :myCoolFeature
```
