### add typscript to project

```bsh
dotnet add package Microsoft.TypeScript.MSBuild
```

### compile typescript

```bsh
tsc
```

### dotnet build vs tsc

dotnet build will build all *.ts files with the tsc command automatically. Running `tsc` will complie your ts on the fly, say if you were already running an app and wanted to transpile your ts to js and refresh the browser. people sometimes automate this with a gulp task.