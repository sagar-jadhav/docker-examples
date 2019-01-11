# Docker Volumes

## Lab 1 

### Creating Volume Mount from **Dockerfile**

Create a file with name volume and paste the following content to it:

```
FROM ubuntu:latest
RUN mkdir /data
WORKDIR /data
RUN echo "Hello from Volume" > test
VOLUME /data
```

Execute the following commands:

```
docker build -f volume -t sagarj/volume:1 .

docker run sagarj/volume:1

docker volume ls

cd /var/lib/docker/volumes

ls -ltr

cd ./<FOLDER_NAME_WITH_RANDOM_NUMBERS_&_CHARACTERS>

cd ./_data
 
cat test 
```

## Lab 2 

### Managing volumes through Docker CLI

Execute the following commands:

```
docker volume create demo

docker volume ls

docker inspect demo

docker volume rm demo
```

## Lab 3 

### Creating Volume Mount from **docker run** command & sharing same Volume Mounts among multiple containers

Create a file with name volume_test and paste the following content:

```
FROM ubuntu:latest
RUN mkdir /data
ENV MESSAGE=HI
ENV FILENAME=test
WORKDIR /data
ENTRYPOINT echo ${MESSAGE} > ${FILENAME} && ls
```

Execute following commands:

```
docker build -f volume_test -t sagarj/volume_test:1 .

docker run --env MESSAGE="GOOD Morning" --env FILENAME=morning_message\
 --mount type=volume,source=demo,target=/data \
 sagarj/volume_test:1
 
 docker run --env MESSAGE="GOOD Afternoon" --env FILENAME=afternoon_message\
  --mount type=volume,source=demo,target=/data \
  sagarj/volume_test:1

docker volume ls

cd /var/lib/docker/volumes/demo/_data

ls

cat morning_message &&  cat afternoon_message
```

# Bind Mounts:

## Lab 4 

### Mounting host directory into container

Execute the following commands:

```
mkdir test

docker system prune

docker volume prune

docker volume ls

docker run --env MESSAGE="GOOD Afternoon" --env FILENAME=afternoon_message\
 --mount type=bind,source="$(pwd)"/test,target=/data \
 sagarj/volume_test:1
 
 docker volume ls
 
 cd ./test
 
 ls
 
 cat afternoon_message
 ```
