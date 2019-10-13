## Deploy Wordpress With Multiple Container 
In this section we are deploying WordPress Website With Multiple Containers

Start off by making a new directory where you wish to store the files for WordPress and MariaDB for example in your home directory.
```
mkdir ~/wordpress && cd ~/wordpress
```

### Docker Setup For Database Server 

Search Mariadb image
```
docker search mariadb
```

Pull mariadb docker image
```
docker pull mariadb
```

Run database container 
* run -- use for run the container
* -e -- for the env variable like in below command we use MYSQL_ROOT_PASSWORD env variable to assign password
* –name wordpress -- Gives the container a name.
* -v “$PWD/database”:/var/lib/mysql -- Creates a data directory linked to the container storage to ensure data persistence.
* -d -- Tells Docker to run the container in daemon.
* mariadb:latest -- name of image
```
docker run -e MYSQL_ROOT_PASSWORD=root123 -e MYSQL_DATABASE=wordpress --name wordpressdb -v "$PWD/database":/var/lib/mysql -d mariadb:latest
```

List running docker processes 
```
docker ps
```

### Docker Setup For Wordpress

Search Wordpress image
```
docker search wordpress
```

Pull wordpress docker image
```
docker pull wordpress
```

Run Wordpress container
* -e -- for the env variable like in below command we use WORDPRESS_DB_PASSWORD env variable to assign password
* –name wordpress -- Gives the container a name.
* –link wordpressdb:mysql -- Links the WordPress container with the MariaDB container so that the applications can interact.
* -p 8080:80 -- Tells Docker to pass connections from your server’s HTTP port to the containers internal port 80.
* -v “$PWD/html”:/var/www/html -- Sets the WordPress files accessible from outside the container. The volume files will remain even if the container was removed.
* -d -- Makes the container run on background
* wordpress -- name of image
```
docker run -e WORDPRESS_DB_PASSWORD=root123 --name wordpress --link wordpressdb:mysql -p 8080:80 -v "$PWD/html":/var/www/html -d wordpress
```

List running docker processes
```
docker ps
```
Open Your Web Brower and browse **http://localhost:8080**
