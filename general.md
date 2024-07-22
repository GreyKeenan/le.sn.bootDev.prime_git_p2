
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
1. [Stash](#stash) *
	1. [targeting a specific stash](#targeting-a-specific-stash)
	1. [stash conflicts](#stash-conflicts)
1. [Revert](#revert) *
	1. [revert vs reset](#revert-vs-reset)
1. [Diff](#diff) *
1. [Cherry Pick](#cherry-pick)
1. [Bisect](#bisect)
	1. [The Seven Steps of a Bisect](#the-seven-steps-of-a-bisect)
	1. [Automating the Bisect](#automating-the-bisect)


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


## Stash

'git stash' is a way to stire the current state of the working directory & staging area, for later use.
> useful to temporarily store a set of changes you are working on while you navigate the repo to something else, then return to it later

```
git stash
```
stashes the current state, and resets files to the HEAD commit

```
git stash list
```
lists stashes

```
git stash pop
```
un-stashes the most recent stash. It is no longer stored, & its changes/state are applied to the working dir

```
git stash -m "message"
```

```
git stash apply
```
apply changes like pop, but without removing the stash

```
git stash drop
```
remove stash entry, without applying it


### targeting a specific stash
	
```
stash@{#}
```
where 0 = most recent stash


### stash conflicts

when popping/applying, if there are conflicts, they are addressed similarly to merge conflicts or rebase conflicts, just without the final steps of committing/rebasing

--ours = pre-stash-pop/apply
--theirs = the new stash being applied


## Revert

revert is a safer and/or more precise tool than git reset

Revert is an 'anti commit'. Rather than remove a commit, like reset, it creates a new commit which undoes the changes of the commit being reverted. That way, the history is preserved

### use

```
git revert <commitish>
```
This will open txt editor for a commit msg for the new revert commit.


### revert vs reset

generally:
- reset when working on your own branch or undoing something youve already committed
- revert when undoing changes from a shared branch. Not rewriting history is preferred to not screw over other contributors


## Diff

'git diff' shows the difference/changes between two given commits

```
git diff
```
btwn working tree and HEAD commit

```
git diff <commitish>
```
btwn working tree and given commit

```
git diff <commitish1> <commitish2>
```
btwn two given commits
> as in, changes applied to get from 1 to 2


## Cherry Pick

A way to grab a commit from another branch, without grabbing the whole branch

```
git cherry-pick <commitish>
```
this grabs that commit and applies it after the current commit 

> I suppose there cant really be conflicts, since its just one commit.


## Bisect

a tool for finding where bugs originated in a repo
> or finding any abritrary change, not just a bug

searching for the change in a list of all commits becomes O(log n) instead of O(n)


### The seven steps of a bisect

1. `git bisect start`
2. select a "good" commit
> this is a commit where you can be sure the bug is not present
>
> ` git bisect good <commitish> `

3. select a bad commit
> a commit where you know the bug is present
>
> ` git bisect bad <commitish> `

4. git automatically selects a commit btwn good/bad, where you can check if the bug is present
5. execute `git bisect good` or `git bisect bad` to identify current commit as bad/good
6. git will loop back to step 4 until it identifies where the bug was introduced
7. exit with `git bisect reset`

> so essentially, it handles picking commits in an optimal order to ensure that you can check each one most efficiently. The actual checking, though, is done manually. That makes sense! ty binary searches


### automating the bisect

a script can be given to bisect which will automatically do the testing part of the bisect
from man:
```
Bisect run
If you have a script that can tell if the current source code is good or bad, you can
bisect by issuing the command:

   $ git bisect run my_script arguments

Note that the script (my_script in the above example) should exit with code 0 if the
current source code is good/old, and exit with a code between 1 and 127 (inclusive),
except 125, if the current source code is bad/new.
```

after step 3, do the command shown above to automate it. That command will execute for every bisect selection
