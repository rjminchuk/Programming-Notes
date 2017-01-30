
# GIT Commands
Following are commands that can be used in creation, configuration, and branching of a git repository.

#### Create a local git repo.
```sh
rem 'Inialize a local git repository from the current directory' 
git init
```

#### Add file or directory to git repo
```sh
rem 'add a [file or directory] file to the repository'
git add [file or directory]
```

#### View pending changes to branch
```sh
git status
```

#### Commit Changes
```sh
rem 'commit --all adds and changes to the branch with a message -m'
git commit --all -m '[a message]'
```

#### Connect a local repository to a remote repository (vso, github).
```sh
rem 'set the remote repository in which to connect your local repo to'
git remote add origin https://github.com/rjminchuk/Programming-Notes.git
git push -u origin master
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
git checkout -b [branch_name]

rem 'checkout existing branch'
git checkout [branch_name]

rem 'push branch to remote (vso, github)'
git push origin [branch_name]

rem 'delete remote branch'
git push origin --delete [branch_name]

rem 'clean up branches on your local.'
git remote prune origin 

rem 'OR manually delete local branch'
git branch --all
git branch --delete --force [branch_name ie feature/rich/lpl.9999]
```

#### Remove Directory or file from local repository
```sh
git rm -r [directory to remove]
git commit --all -m 'remove [directory to remove]'
```