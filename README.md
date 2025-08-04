# Docker
** We need to first create the ec2 instance and install the Docker on that EC2 machine once install we need to provide the user in the user group were we have freely use the docker without using of the docker command, below are the command:**

sudo usermod -a -G docker ec2-user

## Check user was added or not:
sudo cat /etc/group
docker:x:992:ec2-user


**Some are commands important for the Docker:**
docker images ls –a >> Docker images View

docker container >>View all container

docker system –help >>to check the Systeam information

docker image pull hello-world  >> pulling image

docker container run hello-world >> create the container and run that

-a, --attach list                      Attach to STDIN, STDOUT or STDERR

-d, --detach                     Run container in background and print container ID

-e, --env list                         Set environment variables 

-i, --interactive                      Keep STDIN open even if not attached

-m, --name string                      Assign a name to the container

-p, --publish list                     Publish a container's port(s) to the host

-t, --tty                              Allocate a pseudo-TTY


### Command for generating the container:
docker image pull httpd

docker container run --name websrever -d -i -t -p 7000:80 httpd

docker container stop webserver  >> Stop the container 

docker container start webserver >> Start the container

docker container inspect webserver >> for detail for the image and container

docker container exec -it websrever /bin/bash >> execute the container

Path for changes >>> cd htdocs/


## How to create the images and run as containerization?
-First you need to login the EC2 instance where you have install Docker:

-Now Creating the directory in the Docker server 

-Under the docker we can create the file name image, where we clone the git resposrity of the python pogram

# Dockerfile Keywords Guide

This document provides a basic overview of commonly used Dockerfile instructions. A Dockerfile is a simple text file containing predefined, case-sensitive keywords used to build custom Docker images.

---

## 🐳 Common Dockerfile Instructions

| Keyword      | Description |
|--------------|-------------|
| **FROM**      | Specifies the base image from which the Docker image is created. |
| **MAINTAINER**| Represents the name of the organization or author who created the Dockerfile. *(Deprecated in favor of `LABEL`)* |
| **CMD**       | Defines the default command that will run when the container starts. |
| **ENTRYPOINT**| Sets the default application to be executed inside the container. It also allows arguments to be passed from the `CMD` instruction. |
| **RUN**       | Executes commands in the container during the image build process. Commonly used to install software packages. |
| **USER**      | Specifies the default user to use when running the image. |
| **WORKDIR**   | Sets the working directory inside the container. |
| **COPY**      | Copies files and directories from the host machine to the container. |
| **ADD**       | Similar to `COPY`, but also supports downloading files from URLs and unpacking compressed files. |
| **ENV**       | Sets environment variables inside the container. |
| **EXPOSE**    | Indicates which port the container listens on at runtime. |
| **VOLUME**    | Creates a mount point with a specified path and marks it as holding externally mounted volumes from the native host or other containers. |
| **LABEL**     | Adds metadata to an image, such as a version or description. |
| **STOPSIGNAL**| Defines the system call signal that will be sent to the container to exit gracefully. |

---

## 📝 Example Dockerfile

```Dockerfile
FROM ubuntu:20.04
MAINTAINER example@example.com
RUN apt-get update && apt-get install -y nginx
COPY ./index.html /var/www/html/
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

-While creating the image we require the one text file which is help us to execute the command

-so creating the text file inside same path “Dockerfile”

![image](https://github.com/user-attachments/assets/7e27f5f6-b213-4dec-ab09-e532e612c91d)

# 🐳 Docker Swarm Overview

Docker Swarm is the process of running Docker containers in a **distributed environment** across multiple Docker host machines.

All containers within a Swarm can belong to a **single service** and **share resources** across nodes, even when running on different hosts. Docker Swarm is Docker’s native tool for **container orchestration**.

---

## 🧠 Terminology

- **Manager Node**: The machine on which Docker Swarm is initialized. It controls and manages the cluster.
- **Worker Nodes**: Other machines that join the Swarm and run container tasks assigned by the manager.

---

## 🛠️ Swarm Initialization

To initialize the Swarm on the manager node, use the following command:

```bash
docker swarm init --advertise-addr <private_ip_of_manager>

docker swarm init --advertise-addr 172.31.27.151
```

## ⚖️ Load Balancer in Docker Swarm

Each Docker container is designed to handle a specific amount of user load. When the load increases, Docker Swarm allows us to **replicate containers** and **distribute the traffic** among them automatically using its **built-in load balancing**.

---

### 🔁 Create a Service with Replicas

To create a service with multiple replicas (e.g., 5 instances of a webserver), use:

```bash
docker service create --name webserver -p 9090:8080 --replicas 5 tomee
```
### 📋 Check Service Status
To view the current status of your service and see where replicas are deployed:

```bash
docker service ps webserver
```
## 🗑️ Remove a Service

To remove a service from the Docker Swarm, use the following command:

```bash
docker service rm <service_name>
Example:
docker service rm webserver
```
## 📈 Scaling of Containers

When business requirements increase, we should be able to **increase the number of container replicas** to handle additional load.  
Similarly, when the demand decreases, the number of replicas can be reduced.  
This **scaling must happen without any downtime** in a Docker Swarm environment.

---

### ▶️ Create a Service with Initial Replicas

```bash
docker service create --name webserver -p 9090:8080 --replicas 2 tomee
docker service ps webserver
```
### 🔁 Scale the Service
```bash
docker service scale webserver=10
```
### 🔄 Rolling Updates in Docker Swarm
The services running in docker swarm, can be updated to any other version without any downtime. This is perfomed by docker swarm by updating one replica after another. This is called as rolling update.

## Updates: For update we can use update service command:

```bash
 docker service create --name myredis --replicas 3 redis:3
 docker service ps myredis
 
 docker service update --image redis:4 myredis
 docker service ps myredis
 
[ec2-user@Manager ~]$ docker service ps myredis
ID             NAME            IMAGE     NODE      DESIRED STATE   CURRENT STATE                 ERROR     PORTS
yyja78dnkctc   myredis.1       redis:4   Node01    Running         Running 52 seconds ago
yjn1pm14wknr    \_ myredis.1   redis:3   Node01    Shutdown        Shutdown 54 seconds ago
cwvyanxhg7mo   myredis.2       redis:4   Manager   Running         Running 58 seconds ago
425loaqngk0i    \_ myredis.2   redis:3   Manager   Shutdown        Shutdown about a minute ago
lepxesa1jkwh   myredis.3       redis:4   Manager   Running         Running 56 seconds ago
luct0k018q9x    \_ myredis.3   redis:3   Node01    Shutdown        Shutdown 57 seconds ago
```
 - To check that how many are shutdown and running we use grep command
```bash
docker service ps myredis | grep "Shutdown"
docker service ps myredis | grep "Running"
```
## Rollback: We can rollback the version in the previous version we cam use rollback commnad in service
```bash
 docker service update --rollback myredis
```

