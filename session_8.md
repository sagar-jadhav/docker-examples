# Docker Compose Lab - 1

In this lab session we are going to deploying wordpress website with multiple container using **dockerfile and docker compose**.

### Step 1 - Create new directory **wordpress** and **code** at / directory 
Start off by making a new directory **wordpress** where you wish to store the files for WordPress and MariaDB for example in your / directory and also create new directory **code** where you have save dockerfiles and docker-compose.yml file .
```
mkdir /wordpress /code && cd /code
```

### Step 2 - Write the dockerfile called **dockerfile1** for database 
create a new file in **/code** directory called **dockerfile1** and put below code in it and save it .
```
ARG CODE_VERSION=latest
FROM mariadb:${CODE_VERSION}
ENV MYSQL_ROOT_PASSWORD=root123
ENV MYSQL_DATABASE=wordpress
```

### Step 3 - Write the dockerfile called **dockerfile2** for wordpress
create a new file in **/code** directory called **dockerfile2** and put below code in it and save it .
```
ARG CODE_VERSION=latest
FROM wordpress:${CODE_VERSION}
ENV WORDPRESS_DB_PASSWORD=root123
```

### Step 4 - Write the **docker compose** file called **docker-compose.yml** 
create a new file in **/code** directory called **docker-compose.yml** and put below code in it and save it .
```
version: '3'
services:
  wordpressdb:
    build:
      context: .
      dockerfile: dockerfile1
    volumes:
      - /wordpress/database:/var/lib/mysql
  wordpress:
    build:
      context: .
      dockerfile: dockerfile2
    ports:
      - "8080:80"
    volumes:
      - /wordpress/html:/var/www/html
    links:
      - wordpressdb:mysql
```

### Step 5 - Run the docker compose 
**docker-compose up** command is use for launch our containers
```
docker-compose up
```
### Step 6 - Open the website in browser
In this step we are to open our wordpress website on browser for that in up side on **play with docker** open port display in **blue color** click on that .

![play_with_dockeer](../images/labs_required/13.png)
