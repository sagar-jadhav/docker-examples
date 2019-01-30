## Steps to set up Docker Lab

It's a prerequisite for the Hands On Session

**Create a Docker Hub id**
Visit https://hub.docker.com/signup - use your personal email

**Verify Email Address**
You will receive an email to confirm the Docker Hub account, once confirmed, your account is created with Docker Hub.

**Accessing Docker Lab** 
This is a free online lab environment for Docker, you can access it using the Docker Hub account create using the above steps, visit https://labs.play-with-docker.com/ and click on Login. This will open up a pop-up for login, ensure you don’t have pop-up blocker enabled in your browser. Once logged in with Docker, you should see the start button, click on Start

**Add a Docker Instance**
Click on ‘ADD NEW INSTANCE’ label under Instance in the left hand side navbar. This will spawn up a docker instance for running lab tasks. Congratulations ! You have successfully setup your lab. You can click on close session if you want to close the lab.

## 1.Run your first container

1. Run a container
2. Run multiple containers
3. Remove the containers

----------------------------------------------------------------------------------
### Run a container :-
Verify version of docker
```
docker --version
```
List docker images
```
docker images
```

Pull tomcat docker image
```
docker pull tomcat:8.0
```

Verify that tomcat docker image is pulled successfully or not
```
docker images
```

Run container A
```
docker run -d -p 8080:8080 --name containerA tomcat:8.0 
```

List running Docker processes
```
docker ps
```

Test the application deployed in a container
```
curl http://localhost:8080
```

----------------------------------------------------------------------------------
###  Run multiple containers :-

Pull java docker image
```
docker pull openjdk:8
```

Run container B
```
docker run --name containerB openjdk:8
```

List all docker processes
```
docker ps -a
```

----------------------------------------------------------------------------------
###  Remove the containers :-

List all docker processes
```
docker ps -a
```

Stop running container
```
docker stop containerA
```

List all docker processes
```
docker ps -a
```
Remove all containers
```
docker rm containerA containerB
```
Verify whether docker containers removed or not 
```
docker ps -a
```

----------------------------------------------------------------------------------
## 2.Add CI/CD value with Docker images

1. Create a Java App (without using Docker)
2. Create and build the Docker image
3. Run the Docker image
4. Push to a central registry
5. Deploy a change
6. Understand image layers
7. Remove containers

----------------------------------------------------------------------------------


### 1. Create a Java App (without using Docker)
### 2. Create and build the Docker image
### 3. Run the Docker image

Run container
```
docker run -it --name javacontainer openjdk:8
```

Clone the HelloWorld java program
```
git clone https://gist.github.com/b95b2903d391af2191c63411efb2e113.git
```

Go inside the cloned directory
```
cd ./b95b2903d391af2191c63411efb2e113
```

Compile HelloWorld java program
```
javac HelloWorld.java
```

Copy the class to root
```
cp ./HelloWorld.class /
```

Go to root
```
cd /
```

Test the HelloWorld java program
```
java HelloWorld
Run apt-get update
apt-get update
```

Install nano file editor
```
apt-get install nano
```

Open bashrc file
```
nano ~/.bashrc
```

Add following command at the end and the save using ctrl+o & ctrl+x
```
java HelloWorld
```

Test the changes
```
source ~/.bashrc
```

Exit from the container
```
exit
```

Commit the container to create new docker image
```
docker commit javacontainer helloworld:1
```

List all docker images
```
docker images
```

Remove the javacontainer
```
docker rm javacontainer
```

Run helloworld container
```
docker run -it --name helloworldcontainer helloworld:1
```

Exit from the container
```
exit
```

Remove the helloworldcontainer
```
docker rm helloworldcontainer
```

----------------------------------------------------------------------------------

### 4. Push to a central registry

Tag the docker image, Here instead of sagarj use your own docker hub username
```
docker tag helloworld:1 sagarj/helloworld:1
```

List all the docker image
```
docker images
```

Do docker login. Provide your docker hub username & password
```
docker login
```

Push helloworld docker image into the docker hub repository
```
docker push sagarj/helloworld:1
```

Vist the docker hub and verify that your image is pushed or not
```
Browse https://hub.docker.com/
```

----------------------------------------------------------------------------------

### 5. Deploy a change
### 6. Understand image layers

Run the helloworldcontainer
```
docker run -it --name javacontainer helloworld:1
```

Go inside the cloned directory
```
cd ./b95b2903d391af2191c63411efb2e113
```

Edit the HelloWorld program and update the message
```
nano HelloWorld.java
```

Compile HelloWorld java program
```
javac HelloWorld.java
```

Copy the class to root
```
cp ./HelloWorld.class /
```

Go to root
```
cd /
```

Test the HelloWorld java program
```
java HelloWorld
```

Exit from the javacontainer
```
exit
```

Commit the changes
```
docker commit javacontainer sagarj/helloworld:2
```

List all docker images
```
docker images
```

Remove the javacontainer
```
docker rm javacontainer
```

Test the HelloWorld container
```
docker run --name updatedcontainer -it sagarj/helloworld:2
```

Exit from the updatedcontainer
```
exit
```

Push helloworld docker image into the docker hub repository
```
docker push sagarj/helloworld:1
```

Vist the docker hub and verify that your image is pushed or not
```
Browse https://hub.docker.com/
```

----------------------------------------------------------------------------------

### 7. Remove containers

Clean up the containers
```
docker system prune
```

Clean up the images
```
docker rmi -f $(docker images)
```

----------------------------------------------------------------------------------
