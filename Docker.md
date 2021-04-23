## Docker Basic Commands

| Command | Description |
| ------- | ----------- |
| `docker version` |Shows docker details|
| `docker info` |Shows docker more Info details|
| `docker container stats` |Live monitoring of containers|
| `docker container inspect <containerId>` |Shows metadata how the container was started|
| `docker top <containerId>` |shows the process running inside a container|

## Docker Containers
| Command | Description |
| ------- | ----------- |
| `docker container ls -a` |Lists all available containers including hidden containers|
| `docker container run --publish 80:80 nginx` |Starts an nginx server in port 80|
| `docker run -p 3000:80 -d --name any_name nginx` |Runs an nginx server in port 3000 in detach state|
| `docker container start <containerId>` |Starts unique container with the id|
| `docker container stop <containerId>` |Stops unique container with the id|
| `docker container rm -f <containerId>` |Force removes the container with id|
| `winpty docker run -it <imageName> bash` |Starts a new image in bash shell|
| `winpty docker start -ia <containerId>` |Start an existing container|

## Build Custom Image

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

| Command | Description |
| ------- | ----------- |
| `docker build .` |Builds custom image reading into Dockerfile|