# Bridge Network

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

## Establishing connection between containers using bridge network

### Lab 3

```
docker run --name containerA -d -p 8080:8080 tomcat:latest

docker run --name containerB -d -p 8081:8080 tomcat:latest

docker inspect containerA

docker inspect containerB

// Note the IP Address for both containers from the output of above commands

docker exec -it containerA /bin/bash

ping <IP_ADDRESS_CONTAINER_B>

ping www.google.com

ping containerB

exit

docker exec -it containerB /bin/bash

ping <IP_ADDRESS_CONTAINER_A>

ping www.google.com

ping containerA

exit

docker network create my-net-a

docker network connect my-net-a containerA

docker network connect my-net-a containerB

docker inspect containerA

docker inspect containerB

// Note the IP Address for both containers from the output of above commands

docker exec -it containerA /bin/bash

ping <IP_ADDRESS_CONTAINER_B>

ping containerB

ping www.google.com

exit

docker exec -it containerB /bin/bash

ping <IP_ADDRESS_CONTAINER_A>

ping containerA

ping www.google.com

exit

docker network disconnect my-net-a containerA

docker network disconnect my-net-a containerB

docker container stop containerA containerB

docker container rm containerA containerB

docker network rm my-net-a

```

## Host Network

### Lab 4


```
docker network ls

docker run -d --network host --name containerA tomcat:latest

curl localhost:8080

docker stop containerA

docker rm containerA

```
