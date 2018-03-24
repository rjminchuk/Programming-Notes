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



# 2018-02-10 - Setting up Microsoft SQL Server in Docker on MacOS

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

Microsoft then suggests you change your password again and provides these commands as well to execute a password change in TSQL from within the container. I vaguely remember this being important. Better run it just in case.

```bash
sudo docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd \
   -S localhost -U SA -P 'YourStrong!Passw0rd' \
   -Q 'ALTER LOGIN SA WITH PASSWORD="YourNewStrong!Passw0rd"'
```

#### Connecting to your SQL2017 instance

Firstly, you can use docker to run SQL commands by opening `bash` in your running container, but for our purpose, let's connect to the container from not within our docker container.

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
   static void Main()
   {
      SqlConnection conn = new SqlConnection("Data Source=(192.168.1.70,1401);Database=Master;Integrated Security=SSPI;User Id=SA;Password=YourStrong!Passw0rd");
      conn.Open();
      SqlCommand cmd = new SqlCommand("SELECT * FROM master.dbo.sysdatabses", conn);
      SqlDataReader reader = cmd.ExecuteReader();
      while (reader.Read())
      {
         string test = (string)reader[0];
         Console.WriteLine(test);
      }
   }
}
```
