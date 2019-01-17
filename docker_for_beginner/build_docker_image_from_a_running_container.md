# Build Docker Image From A Running Container

Hello friends, In this lab session we are going to build the image from running container and for that we are going to use nginx image and run time we are going to modify the web contents and build the new nginx image with modify contents .

## Step 1 - Run the nginx container 
Search the nginx offical image 
```
docker search nginx
```

Pull the nginx image 
```
docker pull nginx
```

Run the nginx container
* -p 8080:80 option -- tells docker to pass connections from your server’s HTTP port to the containers internal port 80.
* -d -- makes the container run on background
```
docker run --name containerA -p 8080:80 -d nginx:latest
```

List running docker processes
```
docker ps
```

## Step 2 - Check the web site is working or not
Check that weather our website(which is hosted with nginx on container) is working or not for this we use the wget command who request our web page and download the index.html from our container . 
```
wget http://localhost:8080
```
If you get index.html file then it working .<br/>
If you don't have **wget** command in your system then you can install it by below command .
```
apt-get install -y wget 
```
To see the contents of **index.html** file
```
cat index.html
```
After that remove **index.html** file
```
rm index.html
```

## Step 3 - Modify the contents of web page 
Now in this step we want to modify the content the contents of webpage<br/>
So 1st we want to get the access of bash of Our Running container 
* exec -- it is use to Run a command in a running container
* -it -- the -i (interactive) flag to keep stdin open and -t to allocate a terminal.
* containerA -- it your nginx running container name .
* /bin/bash -- it is a command which you want to execute .
```
docker exec -it containerA /bin/bash
```

When you getting running container terminal execute following command 
```
echo 'hello this for testing' > /usr/share/nginx/html/index.html
exit
```

## Step 4 - Check weather modify contents are display or not 
Ok, Now in this step we can check weather we get our modify contents or not 
```
wget http://localhost:8080
```

To see the contents of **index.html** file 
```
cat index.html
```
If you get **hello this for testing** then it working fine .<br/>
After that remove **index.html** file
```
rm index.html
```

## Step 5 - Create the image from running container 
In This step we want to create image of our nginx running container 
* commit -- Create a new image from a container's changes
* containerA -- it is name of our running nginx container 
* nginx_modify:1 -- it name of your new image and tag 
```
docker commit containerA nginx_modify:1
```

To check that weather your new image is created or not 
```
docker images
```

## Step 6 - Stop and remove all containers
Now it time to stop your all running containers and so we get all port free 
* stop -- is use to stop the running container with that we provide the container name
* rm -- is use to remove the container with that we provide the container name
```
docker stop containerA
docker rm containerA
```
## Step 7 - Run new container with our created image
In this step we are going to start new container with our new nginx image 
```
docker run --name containerB -p 8080:80 -d nginx_modify:1
```

## Step 8 - Check that weather our modify contents are display or not
And Finally we are going test the weather our new image is show in modify contents or not 
```
wget http://localhost:8080
```

To see the contents of **index.html** file
```
cat index.html
```
If you get **hello this for testing** then **we did it.**
