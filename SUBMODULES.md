[Back to index](README.md)
# Creating and updating submodules
You may want to add submodules to a repo; a "submodule" is git jargon that basically refers to a git repo nested within another repo (termed "superproject").  The most common use for this is to have a repo containing code to be shared between multiple other repos.

For the sake of example, let's say our submodule is going to be one such repo, containing a project called (and outputting a DLL named) `MyUtilities`.

As it's shared code to be referenced from another project, it will probably want to be in a separate dependencies directory.  To create the directory and then add the submodule:
```
mkdir Dependencies
cd Dependencies
git submodule add https://github.com/[username]/MyUtilities.git
```

This will add a reference to the submodule in a file in the superproject's root dir called `.gitmodules`, and check out the contents of the repo/submodule `MyUtilities.git`.  As a side note, you should probably use `git clone --recursive` for any future clone you make of this superproject repo so that all submodules are recursively cloned as well as the superproject repo, or set up an alias in your global config to make the `clone` command do this by default.

If the `MyUtilities` project outputs a DLL which is to be copied into the `Dependencies` directory, you'll probably want to have source control ignore that DLL since the idea is to build the DLL from the submodule and copy it each time rather than to have it checked into source control, by adding something like the following to `.gitignore`:
```
# Ignore my utilities (should be built separately)
/Dependencies/MyUtilities.dll
```

Once the changes made above are committed, you'll have successfully added a reference to the `MyUtilities` repo as a submobule.  However, git always holds a reference to a particular commit in a submodule (it's stored directly in git's object database) - you can't just reference "the latest commit in master" or something.  This means that if you add a new commit to `master` in the submodule repo, you'll also need to update the submodule reference to point to that new commit.  Assuming there are no other changes in the staging area and you just want to add one commit with the submodule reference updates, use:
```
git submodule update --remote --merge
git commit -am "Updated submodule refs to latest changes."
```

This fetches the latest changes from upstream in each submodule, merges them in, and checks out the latest revision of the submodule.  In other words, if the submodule currently points to the `master` branch, it will get the latest remote commit from `master`, merge it into the superproject's (this repo's) copy of the submodule repo, and update the reference to that latest commit.  This is probably what you want to do each time you update the submodule and want access to the changes made.
