# Local Docker and K8s Development

This guide should be primarily used for devs who are building their containers
locally and want to deploy these containers into their local k8s environment.

## Prerequistes

1. Have docker desktop installed
2. Have docker desktop running a single node k8s cluster
    * Alternative is minikube or k3s

## Run local docker registry

Docker registry is used to host containers that will be deployed into your
local k8s environment

1. Open a terminal
    * `docker pull registry:2.8.1`
    * `docker run -d -p 5000:5000 --name registry --restart=always registry:2.8.1`
        * `-d` - run in detached mode
        * `-p` - port mapping bind port 5000 to container port 5000
        * `--name` - the name of the container
        * `--restart` - docker desktop will ensure this container is always
        running even after the daemon has restarted
        * `registry:2.8.1` - the container you want to restart

## Push to local docker registry

Once you've built a docker container the following steps will be needed to push
that container into your local docker registry

1. Open a terminal
    * `docker tag YourImageName localhost:5000/YourImageName:<version>`
    * `docker push localhost:5000/YourImageName:<version>`

2. Check registry, open a browser and navigate to catalog
    * `http://localhost:5000/v2/_catalog`
    * A list of images that the local registry contains will be returned

## How to reference local docker registry in k8s yaml

This is how to reference your local docker registry in a kubernetes yaml
for local k8s development/deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: localhost:5000/nginx:1.14.2 # Local reference made here
        imagePullPolicy: Always # tells k8s to always pull image when deployed
        ports:
        - containerPort: 80
```

## Helpful links

* https://hub.docker.com/_/registry
* https://docs.docker.com/registry/spec/api/
* https://docs.docker.com/registry/deploying

