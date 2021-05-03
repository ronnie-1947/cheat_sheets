- [Docker Basic Commands](#docker-basic-commands)
- [Docker Containers](#docker-containers)
- [Docker Images](#docker-images)
  * [Dockerfile](#dockerfile)

## Docker Basic Commands

| Command | Description |
| ------- | ----------- |
| `docker login` |Login to Dockerhub|
| `docker logout` |Logout to Dockerhub|
| `docker version` |Shows docker details|
| `docker info` |Shows docker more Info details|
| `docker container stats` |Live monitoring of containers|
| `docker container inspect <containerId>` |Shows metadata how the container was started|
| `docker top <containerId>` |shows the process running inside a container|

______________


## Docker Containers
| Command | Description |
| ------- | ----------- |
| `docker container ls -a` |Lists all available containers including hidden containers|
| `docker container run --publish 80:80 nginx` |Starts an nginx server in port 80|
| `docker run -p 3000:80 -d --name any_name --rm nginx` |Runs an nginx server in port 3000 in detach state|
| `docker run -p 3000:80 --rm nginx` |provide --rm flag to automatically remove the container as it stops|
| `docker container start <containerId>` |Starts unique container with the id|
| `docker container stop <containerId>` |Stops unique container with the id|
| `docker container rm -f <containerId>` |Force removes the container with id|
| `winpty docker run -it <imageName> bash` |Starts a new image in bash shell|
| `winpty docker start -ia <containerId>` |Start an existing container in terminal|
| `docker logs -f <containerId>` |Shows all logs printed in terminal, and follows it|
| `docker cp <localFilePath> <containerName>:<./destinationPath>` |Copy to and from a running container|
______________

## Docker Images


| Command | Description |
| ------- | ----------- |
| `docker images` |Shows all available images with little detail|
| `docker image inspect <imageId>` |Shows full info of an image|
| `docker rmi <imageID>` |Removes image with the imageId|
| `docker build -t <imageName>:<imageTag> .` |Builds custom image reading the Dockerfile|
| `docker image prune` |Removes all unused images|
| `docker tag <imageName> <newImageName>` |Renames an image name|
| `docker push <imageName>` |Push to dockerhub or some other hub|
| `docker pull <imageName>` |Pull from dockerhub or some other hub|

### Dockerfile
```
FROM node  # Image name

WORKDIR /app # Store files in app folder

COPY package.json /app  # Copy package.json file to app folder 

RUN npm install # Runs the command on building image

COPY . /app  # Copy everything from current folder to app folder 

EXPOSE 80 

CMD ["node", "server.js"] # Cmd run after starting container
```