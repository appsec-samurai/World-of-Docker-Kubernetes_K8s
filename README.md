# docker-cheat-sheet


### Table of Contents

| No. | Questions |
|---- | --------- |
|1 | [**Create,Run,Update and Delete containers**](#create-run-update-and-delete-containers)|
|2 | [**Container Management Commands**](#container-management-commands)|
|3 | [**Container Inspecting Commands**](#container-inspecting-commands)|
|3 | [**Networks**](#networks)|
|4 | [**Cleanup commands**](#cleanup-commands)|


### Create Run Update and Delete containers

#### Create

Create a new container

```cmd
docker container create [OPTIONS] IMAGE [COMMAND] [ARG...]

Example:
docker container create -t -i appsec-samurai/golang --name golang
```

#### Rename

Rename a container

```cmd
docker container rename CONTAINER NEW_NAME

Example:
docker container rename golang golanguage
docker container rename golanguage golang
```

#### Run

```cmd
docker container run [OPTIONS] IMAGE [COMMAND] [ARG...]

Example:
docker container run -it --name golang -d appsec-samurai/golang
```

You can also run a command inside container
```cmd
docker exec [OPTIONS] CONTAINER COMMAND [ARG...]

Example:
docker exec -it golang sh // Or use bash command if sh is failed
```

#### Update

Update configuration of one or more containers

```cmd
docker container update [OPTIONS] CONTAINER [CONTAINER...]

Example:
docker container update --memory "1g" --cpuset-cpu "1" golang // update the golang to use 1g of memory and only use cpu core 1
```

#### Remove

Remove one or more containers

```cmd
docker container rm [OPTIONS] CONTAINER [CONTAINER...]

Example:
docker container rm golang
docker rm $(docker ps -q -f status=exited) // Remove all the stopped containers
```
  **[⬆ Back to Top](#table-of-contents)**

### Container Management Commands

#### Start

Start one or more stopped containers

```cmd
docker container start [OPTIONS] CONTAINER [CONTAINER...]

Example:
docker container start golang
```

### Stop

Stop one or more running containers

```cmd
docker container stop [OPTIONS] CONTAINER [CONTAINER...]

Example:
docker container stop golang
docker stop $(docker ps -a -q) // To stop all the containers
```

#### Restart

Restart one or more containers and processes running inside the container/containers.

```cmd
docker container restart [OPTIONS] CONTAINER [CONTAINER...]

Example:
docker container restart golang
```

#### Pause

Pause all processes within one or more containers

```cmd
docker container pause CONTAINER [CONTAINER...]

Example:
docker container pause golang
```

### Unpause/Resume

Unpause all processes within one or more containers

```cmd
docker container unpause CONTAINER [CONTAINER...]

Example:
docker container unpause golang
```

#### Kill

Kill one or more running containers

```cmd
docker container kill [OPTIONS] CONTAINER [CONTAINER...]

Example:
docker container kill golang
```

#### Wait

Block until one or more containers stop and print their exit codes after that

```cmd
docker container wait CONTAINER [CONTAINER...]

Example:
docker container wait golang
```
  **[⬆ Back to Top](#table-of-contents)**

### Container Inspecting Commands

#### ps

To list running containers

```cmd
docker ps

Example:
appsec-samurai@Torpedo:~$ docker ps
```

#### ps -a

To list all containers

```cmd
docker ps -a

Example:
appsec-samurai@Torpedo:~$ docker ps -a
```

#### logs

To show the container output (stdout+stderr)

```cmd
docker logs -f container

Example:
appsec-samurai@Torpedo:~$ docker logs -f 62ae76de85c7
```

#### top

To list the processes running inside the containers

```cmd
docker top container 

Example:
appsec-samurai@Torpedo:~$ docker top 62ae76de85c7
```

#### diff

To show the differences with the image(modified files)

```cmd
docker diff container 

Example:
appsec-samurai@Torpedo:~$ docker diff 62ae76de85c7
```

#### inspect

To show low level infos (in json format)

```cmd
docker inspect container 

Example:
appsec-samurai@Torpedo:~$ docker inspect 62ae76de85c7
```
  **[⬆ Back to Top](#table-of-contents)**

### Networks

Docker provides network commands connect containers to each other and to other non-Docker workloads. The usage of network commands would be `docker network COMMAND`

#### List networks

List down available networks

```cmd
docker network ls
```

#### Connect a container to network

You can connect a container by name or by ID to any network. Once it connected, the container can communicate with other containers in the same network.

```cmd
docker network connect [OPTIONS] NETWORK CONTAINER

Example:
docker network connect multi-host-network container1
```

#### Disconnect a container from a network

You can disconnect a container by name or by ID from any network.

```cmd
docker network disconnect [OPTIONS] NETWORK CONTAINER

Example:
docker network disconnect multi-host-network container1
```

#### Remove one or more networks

Removes one or more networks by name or identifier. Remember, you must first disconnect any containers connected to it before removing it.

```cmd
docker network rm NETWORK [NETWORK...]

Example:
docker network rm my-network
```

#### Create network

It is possible to create a network in Docker before launching containers

```cmd
docker network create [OPTIONS] NETWORK

Example:
sudo docker network create –-driver bridge some_network
```

The above command will output the long ID for the new network.

#### Inspect network
You can see more details on the network associated with Docker using network inspect command.

```cmd
docker network inspect networkname

Example:
docker network inspect bridge
```
  **[⬆ Back to Top](#table-of-contents)**

### Cleanup commands

You may need to cleanup resources (containers, volumes, images, networks) regularly.

#### Remove all unused resources

```cmd
docker system prune
```

#### Images

```cmd
$ docker images
$ docker rmi $(docker images --filter "dangling=true" -q --no-trunc)

$ docker images | grep "none"
$ docker rmi $(docker images | grep "none" | awk '/ / { print $3 }')
```

#### Containers

```cmd
$ docker ps
$ docker ps -a
$ docker rm $(docker ps -qa --no-trunc --filter "status=exited")
```

#### Volumes

```cmd
$ docker volume rm $(docker volume ls -qf dangling=true)
$ docker volume ls -qf dangling=true | xargs -r docker volume rm
```

#### Networks

```cmd
$ docker network ls
$ docker network ls | grep "bridge"
$ docker network rm $(docker network ls | grep "bridge" | awk '/ / { print $1 }')
```

  **[⬆ Back to Top](#table-of-contents)**
