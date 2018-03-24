# docker

## docker commands

#### all containers running or not
```bash
docker ps -a
```

#### Create a docker container from an image.
There are specific paramerters needed to be passed when creating a container. Each Docker Image has it's own specific parameters that need to be run.
```bash
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=[YourSTRONG!Passw0rd]' -p 1401:1433 --name sql1 -d microsoft/mssql-server-linux:2017-latest
```

#### start and stop a docker container 
```bash
docker container start [container name]
docker container stop [container name]
```

#### container or image info

```bash 
docker container ls
docker image ls
```

#### remove a container or image
```bash
docker container rm [containerid]
docker image rm [imageid]
```

#### ip of running container
```bash
docker inspect --format '{{ .NetworkSettings.IPAddress }}' sql1
```

#### connect to bash of linux container
```bash
docker exec -it [container name] "bash"
```

#### install vim on your basic ass container
```bash
apt-get update
apt-get install vim
```



# 2018.02.23 - Microsoft SQL Server on MacOS with Docker and Dotnet Core

I'm a developer that uses Microsoft technology, and in the last few years, a lot has changed. Dotnet core is gaining traction, and you can even compile and run your .net framework apps on MacOS using the Mono Runtime. `Wow,` I thought. `Everything compiled.` I ran my .net framework app on my MacBook and ran headfirst into my first major setback. Mono (at the time) didn't support SSL sql connections*. After much tinkering, I found that since my DB lives in Azure, and Azure requires SSL, I was SOL.

I still really wanted to get my .net framework app running in mono, so I was off to find a way to run a local sql instance.
First things first, you can't* install the linux version of Microsoft SQL Server on MacOS. So then what? Docker. Docker is dead. Shit.. That's what I get for waiting three months to write this blog up. [Apparently it'll survive](https://chrisshort.net/docker-inc-is-dead/), so.. long live Docker. Let's dive in.

#### Pull the docker image from Microsoft
Microsoft has some pretty [decent steps](https://docs.microsoft.com/en-us/sql/linux/quickstart-install-connect-docker) that I pulled from, but this article contains more of the nitpicky stuff you need to actually run a sql command against it. first things first, [download docker community-edition](https://www.docker.com/community-edition) and install it on your MacOS device.

Open up `terminal` and pull the docker image. `sudo docker pull microsoft/mssql-server-linux:2017-latest`

#### Create a docker container from an image.
There are some pretty specific paramerters that need to be passed when creating a container. Each Docker Image has it's own individual parameters based on what you need the image for. This is straight from the Microsoft steps too.

```bash
docker run -e 'ACCEPT_EULA=Y' \
   -e 'MSSQL_SA_PASSWORD=YourSTRONG!Passw0rd' \
   -p 1401:1433 \
   --name sql2017 \
   -d microsoft/mssql-server-linux:2017-latest
```

#### Connecting to your SQL2017 instance

Firstly, you can use docker to run SQL commands by opening `bash` in your running container, but for our purposes, let's connect to the container from outside of our docker container.

```bash
ifconfig
ping 192.168.1.70
mkdir ~/Source
mkdir ~/Source/sqlTest
cd ~/Source/sqlTest
dotnet new console
dotnet add package System.Data.SqlClient
vi Program.cs
```

Well you opened vi and you're in a blank text editor. Tap `i` to start editing. then `right click` -> `paste` the following code:

```c
using System;
using System.Data.SqlClient;

class Program
{
   private string _server = "192.168.1.70,1401";
   private string _pass = "YourSTRONG!Passw0rd";
   
   static void Main()
   {
      SqlConnection conn = new SqlConnection($"Server={_server};Database=Master;User Id=SA;Password={_pass}");
      conn.Open();
      SqlCommand cmd = new SqlCommand("SELECT Name FROM master.dbo.sysdatabases", conn);
      SqlDataReader reader = cmd.ExecuteReader();
      while (reader.Read())
      {
         string test = (string)reader[0];
         Console.WriteLine(test);
      }
   }
}
```

Hit the `esc` key, then `shift` + `:`, then type `wq`, and hit the `enter` key. You're out of vi and your program is ready to run.

```bash
dotnet run
```

Well look at that, a simple list of all the DBs in your brand spanken new MSSQL Server 2017 docker container, running on your very own local system! 

When you're finished, be sure to turn off your docker container. You'll lose any databases, tables, or data you created when you shut it down, which is fine for this simple example. Microsoft suggests persisting the data before you close the docker container, and suggests a couple ways of doing it; [here](https://docs.microsoft.com/en-us/sql/linux/tutorial-restore-backup-in-sql-server-container), and [here](https://docs.microsoft.com/en-us/sql/linux/sql-server-linux-configure-docker#persist).

```bash
docker container stop sql2017
```

# TODO
- fix intro. it sucks. too much context, not enough explanation of what we're going to actually do.
- add link to .net core nuget package article with installation instructions for dotnet core. [/creating-a-dotnet-core-nuget-package](http://richminchuk.io/creating-a-dotnet-core-nuget-package)
- add note about subing in your own IP and password. 
- replace “sql2017” with “sql1” in article commands