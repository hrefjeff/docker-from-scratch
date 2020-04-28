# Virtual Workshop: Docker from Scratch

## Basics

`docker run -it --platform=linux ubuntu /bin/bash/`

in the container: ls -la, exit

`docker ps` shows all running containers on the machine. Nothing shows because we exited the container and it stopped.

`docker ps -a` shows all containers in any state.

`docker start {container ID or container name}` will start the container

`docker attach {container ID or container name}` will go back into the container
