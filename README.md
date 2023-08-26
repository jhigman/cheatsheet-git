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
git restore -s <sha1> -- <filepath>
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
git branch -a --contains <commit>
```

## restore a file that was deleted

If the file (or directory) was deleted in commit sha <sha>:
```
git checkout <sha>~1 path/to/file.ext
git checkout <sha>~1 path/to/directory/
```
