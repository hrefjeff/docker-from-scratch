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

## Step 3 - Volumes

To make files available in the container that is on the host, use a volume.

`docker run --platform=linux --rm -v "$(pwd):/files" maxcnunes/unrar unrar x -r Trunk.rar`

before the colon, it is our path, then after it's the location in the container

## Step 4 - Some images

`docker run --platform=linux -it --rm --name node node:7.7.4-alpine`

Can write javascript on the node server now

`console.log('hello')`

`var fs = require('fs')`

`fs.readdirSync('/', function(err, files){ console.log(files); })`

## Step 5 - Node app

`docker run --platform=linux -it --rm --name node -d -v "$(pwd):/src" -w /src node:7.7.4-alpine node app.js`

-w = working directory 
-d = remember this is interactive, but we don't attach to the process

`docker ps` to see if it's up

`curl http://localhost:3000` will be from the host machine

Uh uh! Can't connect!

`docker logs -f node`

`docker inspect node` shows details of container. notice ports is empty

`docker kill node`

## Step 6 - Port access

`docker run --platform=linux -it --rm --name node -d -v "$(pwd):/src" -w /src -p 8080:3000 node:7.7.4-alpine node app.js`

`docker ps` will now show ports 0.0.0.0:8000 -> 30000/tcp

`curl http://localhost:8080` on host machine. Yay! It works

## Step 7 - Dockerfile

```Dockerfile
FROM node:7.7.4-alpine

EXPOSE 3000
RUN mkdir /src
COPY app.js /src
WORKDIR /src
CMD node app.js
```

expose is just an indicator that the container will be listening on 3000. it doesn't do anything internally.

`docker build --platform=linux -t nodejs-app .`

`docker run --platform=linux -p 8080:3000 -d nodejs-app`

`docker images` will show the image that was just created

`curl http://localhost:8080` will show hello world

`docker rmi nodejs-app` to get rid of the image that was built

## Step 8 - docker-compose

Defined with YAML files

```YAML
version: "2.4"

services:
    node:
        image: node:7.7.4-alpine
        ports:
            - "8080:3000"
        volumes:
            - .:/src
        working_dir: /src
        command: node app.js
        networks:
            - webnet
        platform: linux
networks:
    webnet:
```
