# cheatsheet-git


## merge when parent branch has already been merged

branched "foo" from master, then branched "bar" from "foo"
and now foo has been merged into master WITH SQUASH

```
git checkout bar
git rebase --onto master foo
git push --force origin bar
```

now bar is a child of master

OR

find common ancestor of branches, and use that to rebase onto:

I have one feature branch (feature-a). I push some commits to that branch. Then I create another feature branch off of that (feaure-b-off-a). Then I squash-merge feature-a into main, I'll see duplicate commits in the PR of feature-b-off-a into main.

To fix this, we can use a rebase --onto. But the manual work of that is finding C (I refer to common-ancestor below). Rewritten another way:

```
git rebase --onto main common-ancestor feature-b-off-a
```

Take all the commits between common-ancestor and feature-b-off-a and rebase them onto main

Fortunately, git has a way of finding the common ancestor between two branches:

```
git merge-base feature-b-off-a feature-a
```

from https://stackoverflow.com/a/70994400/9598220




## undo commit before it's been pushed

commited some changes, but haven't pushed

```
git reset HEAD~1
```

## undo commit AFTER it's been pushed

commited some changes to branch1, and pushed - but the changes should have been made in branch2

```
git checkout branch2
git cherry-pick <sha of bad commit>
git push origin branch2
```

then fix up branch1 if necessary

```
git checkout branch1
git revert <sha of bad commit>
git push origin branch1
```


## remove untracked files and directories

```
git clean -fd
```

## edit commit message

committed some changes, but haven't pushed - and there's a typo in the commit message:

```
git commit --amend
```

## change the name of a branch (local and remote)

change the name and replace the remote branch 

```
git checkout <old_name>
git branch -m <new_name>
git push origin -u <new_name>
git push origin --delete <old_name>
```

## fetch a file from a specific branch or commit

made some change in this branch, but want to revert to a file as seen in another branch or commit

```
git checkout master -- <filepath>
```
or
```
git restore -s <commit-sha> -- <filepath>
```

## upload an existing project

project exists on the local machine but isn't in a repo yet

create the empty repo in github, and grab the <url>

```
git init
git add -A
git commit -m 'Added my project'
git remote add origin <url>
git push -u -f origin main
```

## delete branch locally and remotely

```
git branch -d localBranchName // delete branch locally
git push origin --delete remoteBranchName // delete branch remotely
```

## stash single file

```
git stash -- <filename>
```

## find what branch a commit came from

```
git branch -a --contains <commit-sha>
```

## restore a file that was deleted

Find the commit where a file was deleted and restore it:

```
git log --diff-filter=D --summary
git checkout <commit-sha>~1 path/to/file.ext
git checkout <commit-sha>~1 path/to/directory/
```

## remove all files deleted by a branch but still in master

When changes have been made to the master version of files, which have then been deleted in our branch (and we want to keep the deletions):

```
git diff --name-only --diff-filter=U | xargs git rm
```

## merge after a branch-from-a-branch, and the first branch has been squash-merged

See: https://stackoverflow.com/questions/22593087/merging-a-branch-of-a-branch-after-first-branch-is-squashed-when-merged-to-maste

Replays all commits, starting at feature_branch exclusive, through dependent_feature inclusive, onto master

```
git rebase --onto master feature_branch dependent_feature
```

## checkout remote version of branch replacing local (changed) branch

```
git reset --hard origin/master
```

## create patch of commit and apply to another directory

```
git format-patch -1 <commit-sha>
git apply -v --directory=<new-dir> 0001-some-changes.patch
```

## revert a commit that was a squash merge

```
git revert -m1 <commit-sha>
```

and then push


## split a PR into separate PRs

If too many changes are in the PR, branch from the feature branch and then reset to main - then adjust the changes for this branch

```
git checkout -b <new-smaller-branch>
git reset --soft master
git checkout main -- <path/to/file1/to/drop>
git checkout main -- <path/to/file2/to/drop>
git stash -p
```

DON'T COMMIT YET.. you're just staging changes back to master for some files..

```
git reset --soft <new-smaller-branch>
git commit -am "Rolled back stuff.."
git push
``

You should end up with the new-smaller-branch with just the changes that you didn't remove by stashing or restoring..
