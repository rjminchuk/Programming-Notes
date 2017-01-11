# Programming Notes

## OOP Concepts

#### Frameworks VS Design Pattern
- I like to think of new development as an opportunity to build a framework for interaction between different software layers, using design patterns as the structure of how each layer will interact with one another layer.
- A framework dictates how to use itself, while a design pattern is used to develop the framework.

#### Abstract Classes
- An abstract class might be used to represent the similarities between two different concrete classes.
- An abstract class cannot be instatiated but it's members may be implemented and extended.

#### Interfaces
- An interface can be used to represent similar functionalities across concrete classes that are very much dissimilar.
- An interface can be inherited.

#### Factories
- A factory can be used to limit the instantiation of an object to one place in code. 

## GIT Commands
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

## VIM Commands
Create new or open file in vim

```sh
vim README.md
```

Make edits with `Insert`.

Stop editing with `Esc`.

Save edits with `Esc` `:` `w` `Enter`.

Quit without saving `Esc` `:` `q` `!` `Enter`.

## Quotes
> An opportunity to learn is wasted only by those unwilling to start.