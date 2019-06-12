#### [<< readme.md](../README.md) 
# GIT Commands
Following are commands that can be used in creation, configuration, and branching of a git repository.

#### Map a computer to a Git Account
```sh
git config --global user.email "<email>"
git config --global user.name "<name>"
```

#### Setup vscode as default editor
```
git config --global core.editor "code --wait"
```

#### Create a local git repo.
```sh
rem 'Inialize a local git repository from the current directory' 
git init
```

#### Add file or directory to git repo
```sh
rem 'add a <file or directory> file to the repository'
git add <file or directory>
```

#### View pending changes to branch
```sh
git status
```

#### Commit Changes
```sh
rem 'commit --all adds and changes to the branch with a message -m'
git commit --all -m '<a message>'
```

#### Connect a local repository to a remote repository (vso, github).
```sh
rem 'set the remote repository in which to connect your local repo to'
git remote add origin https://github.com/rjminchuk/Programming-Notes.git
git push -u origin master
```

#### Change the origin from http to ssh
```
git remote set-url origin ssh@ssh.richminchuk.io:core
```

#### Set an upstream branch for the local branch.
```sh
git push --set-upstream origin [local branch]
```

#### OR Clone a remote repository to your local machine
```sh
git clone https://github.com/rjminchuk/Programming-Notes.git
```

#### Git branch commands
```sh
rem 'view all branches'
git branch --all

rem 'create a new branch with -b'
git checkout <branch_name_to_branch_from>
git checkout -b <new_branch_name>

rem 'create a new branch with -b from another branch' 
git checkout -b <new_branch_name> <branch_name_to_branch_from>

rem 'checkout existing branch'
git checkout <branch_name>

rem 'push branch to remote (vso, github)'
git push origin <branch_name>

rem 'delete remote branch'
git push --delete <remote_name> <branch_name>

rem 'merge a branch into another branch'
git checkout [branchToBeMergedInto]
git merge <branchToMergeFrom>

rem 'merge a branch into another branch with a squash merge'
git checkout <branchToBeMergedInto>
git merge --squash <branchToMergeFrom>

rem 'abort a conflicted merge'
git merge --abort

rem 'sift through a conflicted merge'
git status
git reset head <file>

rem 'when remote is not reflecting what is actually on remote'
git remote update

rem 'clean up branches on your local. ("prune" not "purge")'
git remote prune origin 

rem 'OR manually delete local branch'
git branch --all
git branch --delete --force <branch_name ie feature/rich/lpl.9999>
```

#### Resolving Push/Sync conflicts.
If you have committed changes to a local branch and your local branch was not up to date
with origin, pulling can become an ordeal. Here is how to resolve.

```sh
git checkout <branchToPush>
git branch --unset-upstream
git pull origin <branchToPush>
git commit -m 'merge'
git branch --set-upstream-to origin/feature/timers.lpl3189
```

#### Resolving Rebase issue.
When VS2017 hangs on rebase, exec the command below, then go to Sync tab in the 
Team Explorer and hit pull. Resolve conflicts, commit merge with note "rebase", then push.

```sh
git rebase --skip
```

#### Remove Directory or file from local repository
```sh
git rm -r <directory to remove>
git commit --all -m 'remove <directory to remove>'
```

#### Stash commands
Stash changes for later  
```sh
git stash
```

View all stashed changes
```sh
git stash list
```

Un-Stash changes on current branch
```sh
git stash apply
```

Un-Stash specific changes
```sh
git stash list
git stash apply stash@{0}
```

drop the top stashed change
```sh
git stash drop
```

drop (clear) all stashes
```sh 
git stash clear
```

#### undo a commit
[Undoing a commit is a little scary if you don't know how it works...](https://stackoverflow.com/questions/927358/how-do-i-undo-the-last-commits-in-git)

use git log to copy your commit notes
```sh
git log
```

reverts the last commit. can be run sequentially for undoing and staging multiple commits
```sh
git reset --soft HEAD~
```

#### Git Smudge, Clean, and .gitattribute

[Customizing-Git-Git-Attributes](https://git-scm.com/book/en/v2/Customizing-Git-Git-Attributes)
