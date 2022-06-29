# cheatsheet-git


## merge when parent branch has alreasdy been merged

i.e. branched as "foo" from master, then branched as "bar" from "foo"
and now foo has been merged into master WITH SQUASH

```
git checkout bar
git rebase --onto develop foo
git push --force origin bar
```
