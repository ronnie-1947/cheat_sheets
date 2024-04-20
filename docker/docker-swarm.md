---
description: Docker swarm mode is a clustering solution built inside Docker
---

# Docker Swarm

## Initialize Docker swarm

Initalizing Docker Swarm with the command "docker swarm init" does the following:

1. Initialize a node manager
2. root signing certificate is created and issued for the first node
3. Create Raft database to store root CA, configurations and secrets

### Swarm Commands

|                                                  |                                   |
| ------------------------------------------------ | --------------------------------- |
| docker swarm init --advertise-addr \<IP address> | Initialize swarm and advertise IP |
| docker swarm join                                | Join a swarm as a node or manager |
| docker swarm join-token                          | Manage join tokens                |
| docker swarm leave                               | Leave the swarm                   |
| docker swarm unlock                              | Unlock swarm                      |
| docker swarm update                              | Update the swarm                  |

### Node Commands

<table><thead><tr><th width="240"></th><th width="500"></th></tr></thead><tbody><tr><td>docker node ls</td><td>List nodes in the swarm </td></tr><tr><td>Docker node ps</td><td>List tasks running on one or more nodes</td></tr><tr><td>docker node inspect</td><td>Display detailed information on one or more nodes </td></tr><tr><td>docker node demote</td><td>Demote one or more nodes from manager in the swarm</td></tr><tr><td>docker node promote</td><td>Promote one or more nodes to manager in the swarm</td></tr><tr><td>docker node rm</td><td>Remove one or more nodes from swarm</td></tr><tr><td>docker node update</td><td>Update a node</td></tr></tbody></table>

### Docker Service

Service command replaces the docker run for swarm

<table><thead><tr><th width="329"></th><th></th></tr></thead><tbody><tr><td>docker service create [image-name]</td><td>Spin up containers in all nodes Or create a service</td></tr><tr><td>docker service ps [service-name]</td><td>Get container info running in the nodes</td></tr><tr><td>docker service update [service-name] --replicas [n]</td><td>Scale up to n number of replicas</td></tr></tbody></table>

#### Docker service ls command

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption><p>Service ls  command</p></figcaption></figure>

#### Docker service ps \[service-name] command

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption><p>Service ps  command</p></figcaption></figure>

#### Scale replica nodes up (docker service update --replica)

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>
