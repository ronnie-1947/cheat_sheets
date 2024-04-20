- [Docker Basic Commands](#docker-basic-commands)
- [Docker Containers](#docker-containers)
- [Docker Images](#docker-images)
- [Docker Volumes](#docker-volumes)
- [Docker Networks](#docker-network)
- [Dockerfile](#dockerfile)
- [Docker Ignore File](#dockerignore)
- [Docker Compose](#docker-compose)
- [Utility Containers](#utility-container)

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
| `winpty docker exec -ia <containerId> bash` |Start an existing container in terminal|
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

____________

## Docker Volume
Volumes are folders on the host machine , which are made available in containers

### Anonymus Volumes
These are unnamed volumes , created with the container and have no use when the container is deleted. Creating Anonymus volume, via docker file
```
FROM node
...
VOLUME ["/app/feedback"] # Specify the folder, to be stored in volume
...
```

### Named Volumes
Named volumes are created when run a container
```
docker container run -v <volumeName>:></folder/path> -d <imageId>
```

### Bind Mounts
Bind Mounts are type of volumes, where the source path is known Or Path is given instead of VolumeName. Bind Mounts are supposed to be READ ONLY. :ro flag is applied in the end
```
docker container run -v "</source/code/path>:</workingDir>:ro" -d <imageId>
```
Running anonymus volumes, named volumes and bindmounts together
```
docker container run -v <volumeName>:></folder/path> -v "</source/code/path>:</workingDir>:ro" -v /workingDir/node_modules -d <imageId>
```

### Volume Commands 
Command | Description |
| ------- | ----------- |
| `docker volume ls` |List all volumes|
| `docker volume rm <volumeId>` |Remove a volume|
| `docker volume prune` |Remove all unused volume|
| `docker volume create <volumeName>` |Create a new volume manually|
| `docker volume inspect <volumeName>` |See Volume meta informations|

____________

## Docker Network

### Connect to localhost
Docker connects to host machine by using special address. **host.docker.internal:[PORT]**
```
mongoClient.connect('mongodb://host.docker.internal:27017/test')
```

### Connect Container to Container
####  Use Container IP address to connect
```
docker container inspect <containerName>
# Where mongo-container ip addr = 172.17.0.2
mongoClient.connect('mongodb://172.17.0.2/test')
```
#### Create Container Network
| Command | Description |
| ------- | ----------- |
| `docker network create <networkName>` |Create network in docker for communication|
| `docker run --network <networkName> <containerName>` |Start a container with the network|

Use other container name as IP address
```
mongoClient.connect('mongodb://<mongo_containerName>/test')
```
____________

## Dockerfile
```
FROM node  # Image name

WORKDIR /app # Store files in app folder

COPY package.json /app  # Copy package.json file to app folder 

RUN npm install # Runs the command on building image

COPY . /app  # Copy everything from current folder to app folder 

ARG DEFAULT_PORT=80

ENV PORT $DEFAULT_PORT

EXPOSE $PORT

CMD ["node", "server.js"] # Cmd run after starting container

ENTRYPOINT ["npm"] # appends command after npm while starting
```


## dockerignore
Prevent from copying into container
```
node_modules
Dockerfile
.git
```
________________

## Docker Compose
Docker compose runs all commands to run containers, set volumes, set networks etc. docker-compose.yaml config file to be written to use docker compose

```
version: '3.8'  # See docs to know latest version available

services: 
  mongodb: # container name
    image: mongo #image Name
    volumes: 
      - data:/data/db
    environment: 
      MONGO_INITDB_ROOT_PASSWORD: secret # set env variable
    env_file: 
      - "./env/mongo.env" # real env variable from a file

  # -------------------------------------------------
  # -------------------------------------------------

  backend: 
    build: ./backend # Relative path to backend folder

    # ------ Detail way to write build ------
    #build: 
    #  context: ./backend
    #  dockerfile: Dockerfile
      #args:
      #  some-arg:'abc..'

    ports: 
      - '2000:80'
    volumes: 
      - logs:/app/logs
      - ./backend:/app
      - /app/node_modules
    env_file: 
      - "./env/backend.env"
    depends_on: # List of containers this container depends on
      - mongodb
  
  # -------------------------------------------------
  # -------------------------------------------------
  frontend:
    build: ./frontend
    ports: 
      - '2001:3000'
    volumes: 
      - ./frontend/src:/app/src
    stdin_open: true
    tty: true
    depends_on:
      - backend

# ---- Named Volumes to be listed here ----
volumes: 
  data:
  logs:


volumes: 
  data:
```
Docker-compose.yaml file uses strict indentations

### Commands
#### Create Container Network
| Command | Description |
| ------- | ----------- |
| `docker-compose up -d` |creates and start containers in detach mode|
| `docker-compose up --build -d` |Builds images first then start containers|
| `docker-compose build` |Builds all images listed in yml file|
| `docker-compose down` |stops and removes container, network|
| `docker-compose down -v` |stops and removes container, network & volumes|
| `docker-compose run --rm <containerNameFromComposeFile> <Any Command>` |Runs docker container|

______________

## Utility Container
These are containers used to code from scratch