# Virtual Workshop: Docker from Scratch

## Basics

`docker run -it --platform=linux ubuntu /bin/bash/`

in the container: ls -la, exit

`docker ps` shows all running containers on the machine. Nothing shows because we exited the container and it stopped.

`docker ps -a` shows all containers in any state.

`docker start {container ID or container name}` will start the container

`docker attach {container ID or container name}` will go back into the container

`docker stop {container ID}` can also stop the container.

`docker rm {container ID}` when the container is no longer needed

## Running multiple containers from 1 image

```
docker run -it -d --rm --platform=linux --name ubuntu1 ubuntu /bin/bash
docker run -it -d --rm --platform=linux --name ubuntu2 ubuntu /bin/bash
docker run -it -d --rm --platform=linux --name ubuntu3 ubuntu /bin/bash
```

-d = want the container to be interactive, but not go into it yet
--name = customize the name instead of autogenerating

```
docker attach ubuntu1
docker attach ubuntu2
docker attach ubuntu3
```

`docker ps` will see 3 containers running

`docker stop ubuntu1` will gracefully stop 1 container.

`docker kill ubuntu2` will send a hard exit (of course, use with care)

`docker rm {all images}`

## 
