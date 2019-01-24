# Docker Compose Lab - 2

In this lab session we are going to deploying wordpress website with multiple container using **dockerfile and docker compose**.

### Step 1 - Create new directory **wordpress** and **code** at / directory 
Start off by making a new directory **wordpress** where you wish to store the files for WordPress and MariaDB for example in your / directory and also create new directory **code** where you clone the repo for required files .
```
mkdir /wordpress /code && cd /code
```

### Step 2 - Clone the git repo 
In this step we are going to clone the repo and in that repo all files which are required for this lab 
```
git clone https://github.com/sagar-jadhav/docker-tutorials.git
```

### Step 3 - Go to the directory **docker compose lab 1**.
To change the directory we use **cd** command <br/>
To see the contents of that directory we use **ls** command <br/>
To see the contents of that file we use **cat** command 
```
cd docker-tutorials/lab_sessions/docker_compose_lab_1/
ls
cat dockerfile1
cat dockerfile2
cat docker-compose.yml
```

### Step 4 - Run the docker compose 
**docker-compose up** command is use for launch our containers
```
docker-compose up
```
### Step 5 - Open the website in browser
In this step we are to open our wordpress website on browser for that in up side on **play with docker** open port display in **blue color** click on that .

![play_with_docker](../../images/labs_required/13.png)
