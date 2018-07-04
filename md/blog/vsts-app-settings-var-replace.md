# VSTS Variable Replacement for Noobs

Ametures do things insecurely. Want to Write and Release code like a pro? Here's 4 tips to developing dotnet core mvc apps securely.

1. .gitignore your appsettings.json files
2. Use enviornment specific appsettings on your local machine and in production.
3. Put your production specific appsettings in your build system
4. Secure your build system

### setup your local machine for Development

```sh
sudo vi ~\.bash_profile
```

type `i` to begin inserting text. right-click and paste the following line.

```
export ASPNETCORE_ENVIRONMENT=Development
```

Hit the `[esc]` key, and then type `:wq` and hit `[enter]`. Next you'll have to restart bash, so type 

```sh
exit
```

Then reopen your terminal. Execute `cat | env` and check that your environment variable is properly set in the list.

### Configure your build process

replace appsettings.json in release process

_todo: include screenshot_

### Configure your server for the right environment

set azure application settings

_todo: include screenshot_
