
# Rebase conflicts

> following merge_conflicts.md, so notes will likely be relative to it

As opposed to merging, rebasing can rewrite git history. If you screw up, you can lose work. But, when done right, rebasing can create a neater commit history

> arise in much the same way as merge conflicts, just when rebasing instead

`Remember`: Do not rebase from main in most cases. Do not want to rewrite history of shared branches


## delimiting conflicts

with rebase conflicts, the delimiters are flipped:
```
>>>>>>> HEAD

(content from the target branch)

=======

(content from the 'current' branch, or branch rebasing-from)

<<<<<<< (name of rebase-from)
```

This is reversed because, when rebases are performed, they first switch to the target branch, then replay changes
> with git branch, status, or log you can see that you are not currently on either branch, as the rebase takes place

### theirs vs ours

as a result of ^, `--theirs` and `--ours` are also "reversed"


## resolving rebase

unlike merge conflicts, you do not complete the rebase by comitting. Instead use:
```
git rebase --continue
```
> after still *staging* the changes


## deleted commits?

sometimes rebase can result in a commit being deleted. This happens when the commit's changes no longer exist as a result of how the rebase conflict was resolved.


## RERERE

stands for "reuse recorded resolution"

Essentially, rerere remembers how you resolved previous conflicts, and if the same conflict arises again, it will automatically resolve it.
> how exactly does it determine "the same conflict"

### rerere in progress

when rerere is enabled, you can see msgs about recording or using recorded conflict resolutions during the rebase

> you still need to stage & --continue the reused resolutions yourself, so you get a chance to check over it


## OOPS! accidental commit?

if you accidentally commit instead of `git rebase --continue`:
```
git reset --soft HEAD~1
```

keeps changes, but moves back to before the accidental commit. Just --continue from here as normal
