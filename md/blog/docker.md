# 2018.02.23 - Connecting to a local Microsoft SQL Server on MacOS with Docker and Dotnet Core

I'm a developer that uses Microsoft technology, and I'm on a mission to never use the Windows OS again. As counter intuitive as that is, a lot of advancements in the last few years have helped make my dream possible. Dotnet core is gaining real traction, Microsoft released Visual Studio Code and Visual Studio 2017 for MacOS. There's decent support for Dotnet Core apps in Visual Studio Online and Azure to Build, Release, and Host code.

Docker Inc. has been influx lately, but the company has established Docker well enough that [it should live on](https://chrisshort.net/docker-inc-is-dead/) (_tldr: skip to the last paragraph_). With that in mind, Docker shouldn't be going anywhere anytime soon. So, since you can't* install the linux version of Microsoft SQL Server on MacOS, we'll use docker to quickly spin up a box that can get us working.

#### Pull the docker image from Microsoft
Microsoft has some pretty [decent steps](https://docs.microsoft.com/en-us/sql/linux/quickstart-install-connect-docker) that I pulled from, but this article contains more of the nitpicky stuff you need to actually run a sql command against it. first things first, [download docker community-edition](https://www.docker.com/community-edition) and install it on your MacOS device. It's pretty easy to get started, so I won't belabor the point.

Open up `terminal` and pull the docker image. `sudo docker pull microsoft/mssql-server-linux:2017-latest`

#### Create a docker container from an image.
There are some pretty specific paramerters that need to be passed when creating a container. Each Docker Image has it's own individual parameters based on what you need the image for. This is straight from the Microsoft steps too. Here's what it's doing. `-d` tells docker which image it is that we'd like to use, in this case, we'd like to use the mssql server image we just pulled down. `-e` specifies environment variables that the image itself can use for configuration. We'll tell docker that we want to accept the End User License Agreement because we don't want to have to type `YES` a hundred times. We also specify a default password for the SQL server. Next, `-p` tells docker to expose the containers ports to the host machine (_so we can connect to SQL from our host machine_). Lastly, `--name` allows us to specify an easy name for the container so we can quickly start and stop it without having to look up the container name.

```bash
docker run \
   -d microsoft/mssql-server-linux:2017-latest \
   -e 'ACCEPT_EULA=Y' \
   -e 'MSSQL_SA_PASSWORD=YourSTRONG!Passw0rd' \
   -p 1401:1433 \
   --name sql1
```

#### Connecting to your SQL2017 instance

Firstly, you can use docker to run SQL commands by opening `bash` in your running container, but for our purposes, let's connect to the container from outside of our docker container. We'll just setup a Dotnet Core console app and add the SqlClient package.

```bash
mkdir ~/Source
mkdir ~/Source/sqlTest
cd ~/Source/sqlTest
dotnet new console
dotnet add package System.Data.SqlClient
```

Great, we've created a new conosole app, and we have all the dependencies we'll need to connect it to SQL. Open `Program.cs` in `vi` to get started. 

```bash
vi Program.cs
```

Tap `v` and press the down arrow to highlight the program, then Tap `d` to delete it. Then `right click` -> `paste` the following code:

```c
using System;
using System.Data.SqlClient;

class Program
{
   private static string _connStr = @"
      Server=127.0.0.1,1401;
      Database=Master;
      User Id=SA;
      Password=YourSTRONG!Passw0rd
   ";

   static void Main() 
   {
      SqlConnection conn =
         new SqlConnection(_connStr);
         
      conn.Open();
      
      SqlCommand cmd = 
         new SqlCommand(@"
            SELECT [Name] 
            FROM master.dbo.sysdatabases;
         ", conn);
         
      SqlDataReader reader = cmd.ExecuteReader();
      
      while (reader.Read()) {
         Console.WriteLine(reader["Name"].ToString());
      }
   }
}
```

Hit the `esc` key, then `shift` + `:`, then type `wq`, and hit the `enter` key to write the file and quit vi. Your program is ready to run.

```bash
dotnet run
```

Well look at that, a simple dotnet core console app that list of all the DBs in your brand spanken new MSSQL Server 2017 docker container, running on your very own local MacOS machine! 

When you're finished, be sure to turn off your docker container. _**You'll lose any databases, tables, or data you created when you shut it down**, which is fine for this simple example._ Microsoft suggests persisting the data before you close the docker container, and suggests a couple ways of doing it; [here](https://docs.microsoft.com/en-us/sql/linux/tutorial-restore-backup-in-sql-server-container), and [here](https://docs.microsoft.com/en-us/sql/linux/sql-server-linux-configure-docker#persist).

```bash
docker container stop sql1
```
