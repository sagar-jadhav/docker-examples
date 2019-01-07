# Docker Networking

## Managing Network

### Lab 1

Execute the following commands:

```
docker network ls

docker network create -d bridge my-net-1

docker network create my-net-2 

docker network ls

docker network rm my-net-1 my-net-2
```

## Connecting User Defined Network with container

### Lab 2

```
docker network create my-net-1

docker run --network my-net-1 -p 8080:8080 -d  --name containerA tomcat:latest

docker inspect containerA

docker network inspect my-net-1

```

