
# Merge Conflicts

> ch 3

Merge conflicts are the result of two different commits/histories modifying the same line, then attempting to merge them


## Manual resolution

Git prompts you to manually address merge conflicts
	
> at time of merge, will enter a 'conflict' state where you must address the conflicts before continuing
>
> running git status will show the status of the merge

inside files, changes will be delimited with:
```
<<<<<<< HEAD

(current-position's changes)

=======

(target-of-merge's changes)

>>>>>>> (name of merge target)
```

after addressing conflicts in each file, simply add and commit to finish the merge commit


> Merging first from main to the feature branch, then later from feature branch to main as an ff-merge, is an effective way to do merges without having to mess around with any rules of the main branch.


## Git's automatic resolution tools:

- `ours`: the branch you are on at merge
- `theirs`: the target branch of the merge

when in conflict status, you can checkout files as 'theirs' or 'ours' to see the automatic resolution
```
git checkout --theirs path/to/file

or

git checkout --ours path/to/file
```
