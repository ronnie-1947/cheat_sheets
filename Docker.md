## Docker Commands

| Command | Description |
| ------- | ----------- |
| `docker version` |Shows docker details|
| `docker info` |Shows docker more Info details|

## Docker Containers
| Command | Description |
| ------- | ----------- |
| `docker container ls -a` |Lists all available containers including hidden containers|
| `docker container run --publish 80:80 nginx` |Starts an nginx server in port 80|
| `docker container run -p 80:80 -d --name any_name nginx` |Starts an nginx server in port 80 in detach state|
| `docker container start <containerId>` |Starts unique container with the id|
| `docker container stop <containerId>` |Stops unique container with the id|
| `docker container rm -f <containerId>` |Force stops the container with id|