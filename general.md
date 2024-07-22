
# general notes for misc or small sections

## index

1. [Forks](#forks)
1. [commitish](#commitish)
1. [HEAD](#head)
	1. [reflog](#reflog) *
		1. [Recovering Deleted Work](#recovering-deleted-work)
1. [Squashing](#squashing)
	1. [caution](#caution)
	1. [squashing with rebase](#squashing-with-rebase)


## Forks

A fork is not a git feature. It is a feature of many git hosting services, like github. A fork is just a copy of the original repository, related through metadata and convention


### general steps of forkinging

1. fork repo to your acct
1. create local clone of the forked repo
1. create a new branch for your modifications
1. make changes
1. commit changes to remote of that new branch
1. pull request to original repo from remote-new-branch


## commitish

a commitish is something that functions similarly to a commit hash in most cases, but is not a hash
> like HEAD~1 or a branch name


## HEAD

HEAD represents where you are currently in the repo

Check the head using:
```
cat .git/head
```


### reflog

for "reference log"

```
git reflog
```
displays a log of where the HEAD has been previously

` HEAD@{#} ` where # is the number of positions to move back on the reflog, 0 being current position, is a way to reference these positions in other cmds


### Recovering Deleted Work

can see where head was previously with 'git reflog'. Then, checkout those commits even if you've deleted the branch
> Because, as established in p1, branches are just named ptrs to locations in the commit chain. Deleting a branchdoesnt delete that commit.

> In tutorial, first showed using cat-file -p on the lost commit->tree->blob to find the deleted file directly.
>
> You can also just git checkout HEAD@{#} to the commit from the deleted branch

You can also just merge to the HEAD@{#} directly


## Squashing

squashing takes the changes from a series of commits and puts them all together in a single commit

> I already know generally how to squash, so idk how thorough these notes will be


### caution

squashing is a destructive action. Backups & performing squashing on temporary branches is recommended

also, using --force is dangerous so uh yeah


### squashing with rebase

The primary way to squash is by using the 'rebase' cmd

> this is because rebase is about replaying changes. Squashing is just re-applying that set of changes, but in a single commit

```
git rebase -i <commitish>
```

opens editor for an interactive squash. Yada yada. Can select which commits to squash or change in other ways. Guide is in the editor


### forcing

if rewriting history of a remote with a squash, need to push to that remote with the --force flag so git doesnt alert you and stop it

