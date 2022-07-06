# cheatsheet-git


## merge when parent branch has already been merged

branched "foo" from master, then branched "bar" from "foo"
and now foo has been merged into master WITH SQUASH

```
git checkout bar
git rebase --onto develop foo
git push --force origin bar
```

now bar is a child of master



## undo commit before it's been pushed

commited some changes, but haven't pushed

```
git reset HEAD~1
```

## remove untracked files and directories

```
git clean -fd
```
