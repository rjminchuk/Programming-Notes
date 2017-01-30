#### [<< readme.md](README.md) 
# Visual Studio Code Notes

## Creating a new .Net Core MVC project

Initialize a C# project:
1) ``[ctrl/command] + ` `` to open terminal
2) Make a Directory for the new project and navigate to it in terminal.  
3) Exec `dotnet new`
4) Resolve the build assets by typing `dotnet restore`. 
5) Exec `dotnet run`

## Project files explained
- `[project.json\project.csproj]` - If you are using the MSBuild-based .NET Core Tools, a .csproj project 
file will be created instead of a project.json. This is looking like it's going to be the standard going 
forward. [More info here.](https://blogs.msdn.microsoft.com/dotnet/2016/05/23/changes-to-project-json/)
- `project.lock.json` - contains information about your project's dependencies to make 
subsequent `dotnet restore` command faster

## Shortcuts & Commands
 
- to open terminal: `` [ctrl/command] + `(backtick)``
- to open output window: ` [ctrl/command] + shift + u ` 