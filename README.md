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

## remove untracked files and directories

```
git clean -fd
```
