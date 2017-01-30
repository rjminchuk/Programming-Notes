#### [<< readme.md](README.md) 
# Visual Studio Code Notes

## .Net Core vs Desktop .Net Framework

> VS Code does not support debugging applications running on the Desktop .NET Framework.
> Due to this focus, many standard C# project types are not recognized by VS Code. 
> An example of a non-supported project type is an ASP.NET MVC Application (though 
> ASP.NET Core is supported). In these cases, if you simply want to have a lightweight 
> tool to edit a file - VS Code has you covered. If you want the best possible experience 
> for those projects and development on Windows in general, we recommend you use Visual 
> Studio Community.

## .Net Standard

[.Net Standard](https://blogs.msdn.microsoft.com/dotnet/2016/09/26/introducing-net-standard/)

## Migrating from project.json to project.csproj for MSBuild
> The dotnet migrate command will migrate a valid Preview 2 project.json based project 
> to a valid Preview 4 csproj project. By default, the command will migrate the root 
> project and any project references that the root project contains. This behavior can 
> be disabled using the --skip-project-references option at runtime. 
> [More info here.](https://docs.microsoft.com/en-us/dotnet/articles/core/preview3/tools/dotnet-migrate)

Get the latest SDK for new commands
- [sdk](https://www.microsoft.com/net/download/core#/current/sdk)
- [runtime](https://www.microsoft.com/net/download/core#/current/runtime)

```sh
rem 'Run .Net Migrate'
dotnet migrate
```

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