# Docker Registry

## Prerequisite
- Must have Docker installed on the host. 
- Good to have have two machines. We can either use two VMs or two instances from a cloud provider (AWS) in same VLAN. This set up would be more suitable to run through the lab. 

### Spin-up Docker Instances
- Log into the two instances and ensure that Docker is installed. 
- Execute **docker info** to ensure that the user has required permissions. If there is an error, we can switch to **sudo** user and continue.
With that set, congratulations! We have successfully setup our lab.

## Lab 1 - Running the first Registry Server

In this lab session we will run our very first Registry Server based on the **registry:2.7** image available on [Docker Hub](https://hub.docker.com/_/registry).

### Spin-up the Registry Server

- Log into the first Docker host.
- Make a note of IP address of this host. We can get the IP, using **ifconfig** command.
- Execute following command
```
docker run -it -p 5000:5000 --name registry registry:2
```

### Interacting with the Registry Server

- Open a browser and go to **http://REGISTRY-HOST-IP:5000/v2/_catalog/**
- This returns the list of **Repositories** as a JSON response, and it should look like
```
// http://REGISTRY-HOST-IP:5000/v2/_catalog
{
  "repositories": [
  ]
}
```

## Lab 2 - Push a Docker Image to Registry 

In this lab session we will build a Docker image and then push it to the Registry.

### Build a Docker Image

- Create a Dockerfile using the **nano Dockerfile**
- Add the following commands in the Dockerfile
```
FROM busybox
LABEL Author="<YOUR-NAME>"
LABEL Event="DockerPune Meetup"
```
- Execute the following command to build the image 
```
docker build -t <REGISTRY-HOST-IP>:5000/my-busybox .
```
- Execute **docker images** to verify the image 

### Push the image

- Execute the following command to tag the image
```
docker tag <REGISTRY-HOST-IP>:5000/my-busybox <REGISTRY-HOST-IP>:5000/my-busybox:v1
docker tag <REGISTRY-HOST-IP>:5000/my-busybox <REGISTRY-HOST-IP>:5000/my-busybox:custom
```
- Execute the command to push the image
```
docker push <REGISTRY-HOST-IP>:5000/my-busybox
```

### Verify the push

- Open a browser and go to **http://REGISTRY-HOST-IP:5000/v2/_catalog/**
- This returns the list of **Repositories** as a JSON response, and it should look like
```
// http://FIRST-HOST-IP:5000/v2/_catalog
{
  "repositories": [
    "my-busybox"
  ]
}
```
- Go to **http://REGISTRY-HOST-IP:5000/v2/my-busybox/tags/list/** to get list of image tags
```
// http://FIRST-HOST-IP:5000/v2/my-busybox/tags/list
{
  "name": "my-busybox",
  "tags": [
    "custom"
    "latest",
    "v1"
  ]
}
```
## Lab 3 - Pulling the image

In this lab session we will learn how to add our registry to different Docker hosts and pull an image.

### The First Attempt

- Go to second Docker host
- Execute the following command to pull the image we just pushed to our registry
```
docker pull <REGISTRY-HOST-IP>:5000/my-busybox
Using default tag: latest
Error response from daemon: Get https://13.233.122.144:5000/v2/: http: server gave HTTP response to HTTPS client
```
- What's wrong? 
- Execute **docker info** command to get Docker daemon details and notice **Insecure Registries**.
  
### Add the Registry Server

- Execute the following command to create the config file for Docker **daemon**
```
nano /etc/docker/daemon.json
```
- Add the following JSON
```
{
  "insecure-registries" : ["REGISTRY-HOST-IP:5000"]
}
```
- Restart the Docker daemon by executing the command **sudo service docker restart**
- Retry to pull the image
```
docker pull <REGISTRY-HOST-IP>:5000/my-busybox
```



  