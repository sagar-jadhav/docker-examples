# Docker Swarm

## Prerequisite

### Create a Docker Hub id
Visit https://hub.docker.com/signup - use your personal email

### Verify Email Address
You will receive an email to confirm the Docker Hub account, once confirmed, your account is created with Docker Hub.

### Accessing Docker Lab
This is a free online lab environment for Docker, you can access it using the Docker Hub account create using the above steps, visit https://labs.play-with-docker.com/ and click on Login. This will open up a pop-up for login, ensure you don’t have pop-up blocker enabled in your browser. Once logged in with Docker, you should see the start button, click on Start

### Add a Docker Instance
Click on ‘ADD NEW INSTANCE’ label under Instance in the left hand side navbar. This will spawn up a docker instance for running lab tasks. Congratulations ! You have successfully setup your lab. You can click on close session if you want to close the lab.

## Lab 1 - Setting up Docker Swarm Master & Worker Nodes

In this lab session we will learn How to set up the Docker Swarm Master & Worker Nodes.

### Setting up Master Node

- Add new instance
- Make a note of IP mentioned in the top
- Execute following command. In this command in place of MANAGER-IP use the IP address noted in above step
```
docker swarm init --advertise-addr <MANAGER-IP>
```
- Copy the command mentioned under **To add a worker to this swarm, run the following command** in output of execution of above command. Find below the sample output:
```
docker swarm join --token SWMTKN-1-4toxzxmtqx8ddhcjwbdf47sygtgakg38qu0v6u1ztwhe
w658b9-4fiebjgjh72ge3eoo1mgpowpu 192.168.0.48:2377
```
- Execute **docker info**
- Execute **docker node ls**

### Setting up Worker Node

- Add new instance
- Execute the command noted in previous steps for e.g. 
```
docker swarm join --token SWMTKN-1-4toxzxmtqx8ddhcjwbdf47sygtgakg38qu0v6u1ztwhew658b9-4fiebjgjh72ge3eoo1mgpowpu 192.168.0.48:2377
```
- Execute **docker info**
- Go to Master Node and execute **docker node ls**

## Lab 2 - Deploying & Inspecting a Service 

In this lab session we will learn How to deploy a Service in Swarm cluster.

- Go to Master Node
- Execute the following command
```
docker service create --replicas 1 --name serviceone alpine top

--replicas = Number of instance
--name = Name of the service
alpine = Name of docker image
top = Command to execute on start
```
- Execute **docker service ls**
- Execute **docker service ps serviceone**
- Execute **docker ps**
- Execute **docker service logs serviceone**
- Execute **docker inspect service --pretty serviceone**

## Lab 3 - Scaling a Service

In this lab session we will learn how to scale a service with different options & also how to create a global service.

- Go to Master Node
- Execute the following commands to scale a service
```
docker service create --replicas 1 --name servicetwo alpine top
docker service ls
docker service scale servicetwo=2
docker service ls
docker service ps servicetwo
docker service update --replicas 3 servicetwo
docker service ps servicetwo
```
- Execute the following commands to create a Global Service
```
docker service create --mode global --name servicethree alpine top
docker service ps servicethree
docker service scale servicethree=5
```
  
## Lab 4: Networking

### Publishing Port

- Go to Master Node
- Execute the following commands to publish the port of Service
```
docker service create --name servicefour --replicas 2 --publish published=8080,target=8080 tomcat:8
docker service ls
docker service ps servicefour
curl http://localhost:8080
```
- At the top of UI near to the IP click on 8080. It will open the Tomcat Home Page running inside the docker.
- Go to Worker Node and execute the following: http://localhost:8080 

### Connecting to an Overlay Network

- Go to Master Node
- Execute the following commands:
```
docker network ls
docker network create -d overlay my-net
docker network ls
docker service create --name serverone --replicas 1 --network my-net tomcat
docker service create --name servertwo --replicas 1 --network my-net tomcat
docker inspect serverone
docker inspect servertwo
docker ps 
docker exec -it <CONTAINER_ID> /bin/bash
ping servertwo
curl http://servertwo:8080
exit
```
- Go to Worker Node
- Execute the following commands:
```
docker ps 
docker exec -it <CONTAINER_ID> /bin/bash
ping serverone
curl http://serverone:8080
exit
```
## Lab 5: Storage

In this session we will learn how to mount volumes of type mount & bind in the Service. If the driver used is local then volumes are created on each node wherever the replica is running. Replication among the nodes is not managed by Docker.

### Mount

- Go to Master Node
- Execute the following commands:
```
docker volume create my-volume
docker volume ls
docker service create --name servicefive --replicas 2 --mount src=my-volume,dst=/etc/nginx/ nginx
docker inspect my-volume

# Go to volume dir
cd <VOLUME PATH>
ls
echo "Test" > test.log
```
- Go to Worker Node
- Execute the following commands:
```
docker inspect my-volume

# Go to volume dir
cd <VOLUME PATH>
ls
```
### Bind

- Go to Master Node
- Execute the following commands:
```
docker service create --name servicesix --replicas 1 --mount type=bind,src=/tmp,dst=/var/www/ nginx
cd /tmp
echo "Hello" > index.html
ls
docker ps
docker exec -it <CONTAINER_ID> /bin/bash
cd /var/www
ls
```

  