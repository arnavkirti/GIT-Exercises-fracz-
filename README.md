# GIT-Exercises-fracz-
This is my set of solutions to the 23 git exercises posted on https://gitexercises.fracz.com/  

# Solutions

## Exercise 1: master
```shell
$> git start
$> git verify
```

## Exercise 2: commit-one-file 
```shell
$> git add A.txt
# or 
$> git add B.txt
$> git commit -m "add one file"
$> git verify 
```

## Exercise 3: commit-one-file-staged
```shell
$> git reset HEAD A.txt
$> git commit -m "file removed from staging index"
$> git verify
```

## Exercise 4: ignore-them
(nano is a text editor in linux and can be used to edit files. Here we edit .gitignore.)
```shell
$> touch .gitignore
$> nano .gitignore
```
```text
*.exe
*.o
*.jar
libraries/
```
```shell
$> git add .
$> git commit -m "commit useful files"
$> git verify
```

## Exercise 5: chase-branch
```shell
$> git checkout chase-branch
$> git merge escaped
$> git verify
```

## Exercise 6: merge-conflict
```shell
$> git checkout merge-conflict
$> git merge another-piece-of-work
$> nano equation.txt
```
```text
2 + 3 = 5
```
```shell
$> git add equation.txt
$> git commit -m "merge and resolve"
$> git verify
```

## Exercise 7: save-your-work
To learn about stashing and cleaning https://git-scm.com/book/en/v2/Git-Tools-Stashing-and-Cleaning (THIS IS THE OFFICIAL GIT DOCUMENTATION)
```shell
$> git stash
$> nano bug.txt # in the text editor delete the bug line
$> git commit -am "remove bug"
$> git stash apply
$> nano bug.txt # in the text editor add the line "Finally, finished it!" to the end
$> git commit -am "done"
$> git verify
```

## Exercise 8: change-branch-history
To learn about git rebase https://git-scm.com/docs/git-rebase
```shell
$> git checkout change-branch-history
$> git rebase hot-bugfix
$> git verify
```

## Exercise 9: remove-ignored
Solution and explanation https://stackoverflow.com/questions/1274057/how-to-make-git-forget-about-a-file-that-was-tracked-but-is-now-in-gitignore
```shell
$> git rm ignored.txt
$> git commit -am "untrack ignored.txt"
$> git verify
```

## Exercise 10: case-sensitive-filename
```shell
$> git reset HEAD^
$> mv File.txt file.txt
$> git add file.txt
$> git commit -m "file.txt"
$> git verify
```

## Exercise 11: fix-typo
Note: **--amend** replaces the  tip of the current branch by creating a new commit.
```shell
$> nano file.txt
# here fix typo in the file
$> git commit -a --amend
# here fix the typo in commit message
$> git verify
```

## Exercise 12: forge-date (most useless exercise, but shows that git is not a monolith)
```shell
$> git commit --amend --no-edit --date="1987-01-01"
$> git verify
```

## Exercise 13: fix-old-typo
```shell
$> git rebase -i HEAD^^
# change "pick" to "edit" where the typo is in the commit message
# fix the typo in the file
$> git add file.txt
$> git rebase --continue
# fix the rebase conflict
$> git add file.txt
$> git rebase --continue
$> git verify
```

## Exercise 14: commit-lost
```shell
$> git reflog
$> git reset --hard HEAD@{1}
$> git verify
```

## Exercise 15: split-commit
```shell
$> git reset HEAD^
$> git add first.txt
$> git commit -m "First.txt"
$> git add second.txt
$> git commit -m "Second.txt"
$> git verify
```

## Exercise 16: too-many-commits
```shell
$> git rebase -i HEAD~4
# replace "pick" with "squash" for the commit with the message "Crap, I have forgotten to add this line." 
# leave a commit message same as the one with which the marked commit is getting squashed with i.e.,
# "Add file.txt"
$> git verify
```

## Exercise 17: executable
Under the hood details https://stackoverflow.com/questions/40978921/how-to-add-chmod-permissions-to-file-in-git
```shell
$> git update-index --chmod=+x script.sh
$> git commit -m "make executable"
$> git verify
```

## Exercise 18: commit-parts
(The task 1 lines and task 2 lines are a little confusing in this) so first when you split it, git will make 4 hunks and then you do n, y, n, y and then for the remaining task 2 you do y, y
```shell
$> git add --patch file.txt
# split the hunks with 's'
# Stage this hunk [y,n,q,a,d,/,j,J,g,s,e,?]?
# select each hunk with 'y' or 'n'
$> git commit -m "task 1"
$> git commit -am "rest"
$> git verify
```

## Exercise 19: pick-your-features
```shell
$> git log --oneline --decorate --graph --all -8
$> git checkout pick-your-features

$> git cherry-pick feature-a 

$> git cherry-pick feature-b 

$> git merge --squash feature-c
# resolve conflict
$> git commit -am "Complete Feature C"
$> git verify
```

## Exercise 20: reabse-complex
Explanation from git-book https://git-scm.com/book/en/v2/Git-Branching-Rebasing (VERY IMPORTANT TO UNDERSTAND THE SOLUTION)
```shell
$> git rebase --onto your-master issue-555 rebase-complex 
$> git verify
```

## Exercise 21: invalid-order
```shell
$> git rebase -i HEAD~4
# reorder the commit messages as needed
$> git verify
```

## Exercise 22: find-swearwords
```shell
$> git log -S shit
# make a note of the commits where a word "shit" was introduced
$> git rebase -i HEAD~105
# replace 'pick' with 'edit' for those commits

# check which files were modified
$> git log -p -1
# replace 'shit' with 'flower' in list.txt
$> git add list.txt
$> git commit --amend
$> git rebase --continue

# check which files were modified
$> git log -p -1
# replace 'shit' with 'flower' in words.txt
$> git add words.txt
$> git commit --amend
$> git rebase --continue

# check which files were modified
$> git log -p -1
# replace 'shit' with 'flower' in words.txt
$> git add words.txt
$> git commit --amend
$> git rebase --continue

$> git verify
```

## Exercise 23: find-bug
```shell
$> git checkout find-bug
$> git bisect start
$> git bisect bad
$> git bisect good 1.0
$> git bisect run sh -c "openssl enc -base64 -A -d < home-screen-text.txt | grep -v jackass"
# you will get a commit id when the word jackass was first introduced
$> git push origin (COMMIT_ID):find-bug
