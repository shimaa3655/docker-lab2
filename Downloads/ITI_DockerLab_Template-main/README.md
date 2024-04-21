# ITI - Docker Lab2 🐋

## Task 1:
Run a container using nginx image, and mount a directory from your host into the 
Docker container. example: /home/samy/nginx:/home/nginx (bind mount)
```bash

```
---

## Task 2:
### Steps
#### 1. Create 2 docker network (net-1 & net-2)
```bash
docker network create network_1
docker network create network_2

```
#### 2. Run 2 new containers using nginx:alpine image, and attach the net-1 to them
```bash
docker run -d --name nginx_net1 --net network_1 nginx:alpine
docker run -d --name nginx_net2 --network network_1 nginx:alpine
```
#### 3.  Run another 1 new containers using nginx:alpine image, and attach the net-2 to them
```
docker run -d --name nginx_net3 --net network_2 nginx:alpine
```
#### 4. Inspect the 3 containers to know their IPs and write them aside
```bash
docker inspect -f '{{.NetworkSettings.Networks.network_1.IPAddress}}' nginx_net1
172.18.0.2

docker inspect -f '{{.NetworkSettings.Networks.network_1.IPAddress}}' nginx_net2
172.18.0.3

docker inspect -f '{{.NetworkSettings.Networks.network_2.IPAddress}}' nginx_net3
172.20.0.2
can use : docker inspect nginx_net1 | grep IPAddress
```
#### 5. Enter a container in the net-1 network and try to ping a container in the net-2 
network (What do you notice?)
```bash
docker exec -it nginx_net1 sh 
Use ping or curl
ping 172.20.0.2
ping fails, because the two containers exist in different networks so we can see any response in terminal.
```
#### 6. Enter a container in the net-1 network and try to ping the other container in the 
same network (What do you notice?)
```bash
docker exec -it nginx_net1 sh 
curl 172.18.0.2
ping 172.18.0.3
ping successful and return response, because the two containers exist in the same network.
```
---
## Task 3: Explain the difference between Docker volumes and Bind Mount.
```bash

 Docker Volumes:
Docker volumes are managed by Docker and stored within the Docker environment. They are stored in a part of the host filesystem which is managed by Docker (/var/lib/docker/volumes/ on Linux by default).
Volumes can be created and managed using Docker CLI commands (docker volume create, docker volume rm, etc.).
They are more suited for production use because they are easily managed and can be used with Docker Swarm or Kubernetes for orchestration.
Docker volumes can be named which makes them easier to identify and manage.
Docker automatically handles permissions and ownership within volumes.

Bind Mounts:
Bind mounts link a directory or file on the host machine into a container's filesystem.
With bind mounts, the files or directories on the host are directly mounted into the container. Changes made in either the container or on the host are immediately reflected in the other.
Bind mounts provide flexibility as they allow you to mount any directory from the host into the container.
They are often used in development environments where developers need to edit code on their local machines and see the changes reflected immediately in the container.
Bind mounts have the advantage of being more straightforward and easier to understand compared to volumes, as they simply map a host directory into the container.

