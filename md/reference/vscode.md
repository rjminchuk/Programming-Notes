#### [<< readme.md](../README.md) 
# Visual Studio Code Notes

## Creating a new .Net Core console app
1. ``ctrl + ` `` to open terminal
2. Make a Directory for the new project and navigate to it in terminal.  
3. Exec `dotnet new`
4. Resolve the build assets by typing `dotnet restore`. 
5. Exec `dotnet run`

## Project files explained
- `[project.json\project.csproj]` - If you are using the MSBuild-based .NET Core Tools, a .csproj project 
file will be created instead of a project.json. This is looking like it's going to be the standard going 
forward. [More info here.](https://blogs.msdn.microsoft.com/dotnet/2016/05/23/changes-to-project-json/)
- `project.lock.json` - contains information about your project's dependencies to make 
subsequent `dotnet restore` commands faster

## VSCode keybindings for sequential tabbing between open files.
```js
[
    {   "key": "ctrl+shift+tab",
        "command": "workbench.action.previousEditor"
    },
    {   "key": "ctrl+tab",
        "command": "workbench.action.nextEditor"
    },
    {   "key": "ctrl+w",
        "command": "workbench.action.closeActiveEditor"
    }
]
```

## Shortcuts & Commands
- to open terminal: `` ctrl + `(backtick)``
- to open output window: ` [ctrl/command] + shift + u ` 
- pull a tab to the right pane `ctrl + alt + right`
- pull a tab to the left pane `ctrl + alt + left`

## Run VSCode from terminal
- `cmd + shift + p` 
- `shell command: install...` hit `enter`. 
- Then restart VSCode.
```sh
code [filename.ext]
```