# Programming Notes


## Quotes
> An opportunity to learn is wasted only by those unwilling to start.


## Frameworks VS Design Pattern
- I like to think of new development as an opportunity to build a framework for interaction between different software layers, using design patterns as the structure of how each layer will interact with one another layer.
- A framework dictates how to use itself, while a design pattern is used to develop the framework.


## OOP Concepts


### Abstract Classes
- An abstract class might be used to represent the similarities between two different concrete classes.
- An abstract class cannot be instatiated but it's members may be implemented and extended.

### Interfaces
- An interface can be used to represent similar functionalities across concrete classes that are very much dissimilar.
- An interface can be inherited.

### Factories
- A factory can be used to instantiate an object in order to limit instantiation of that object to one place in code.


## GIT Commands
Following are commands that can be used in creation, configuration, and branching of a git repository.


### Create a local git repository.
```sh
rem 'Inialize a local git repository from the current directory' 
git init
```


### Initial git repository setup
```sh
echo "# Programming-Notes" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/rjminchuk/Programming-Notes.git
git push -u origin master
```

### Commit Changes
```sh
git commit --all -m 'a message'
```


### Git repository from existing repo on local.
```sh
git remote add origin https://github.com/rjminchuk/Programming-Notes.git
git push -u origin master
```


### Git branch commands
git -prune origin 


## VIM Commands
Create new or open file in vim

```sh
vim README.md
```

Make edits with `Insert`.

Stop editing with `Esc`.

Save edits with `Esc` `:` `w` `Enter`.

Quit without saving `Esc` `:` `q` `!` `Enter`.





