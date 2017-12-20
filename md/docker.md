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