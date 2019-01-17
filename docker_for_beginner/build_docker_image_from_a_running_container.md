# Build Docker Image From A Running Container

Hello Friends, In this lab session we are going to build the image from running container and for that we are going to use nginx image and run time we are going to modify the web contents and build the new nginx image with modify contents .

## Step 1 - Run the Nginx Container 
Search the Nginx Offical Image 
```
docker search nginx
```

Pull the Nginx Image 
```
docker pull nginx
```

Run the Nginx Container
* -p 8080:80 option -- Tells Docker to pass connections from your server’s HTTP port to the containers internal port 80.
* -d -- Makes the container run on background
```
docker run -p 8080:80 -d nginx:latest
```

List running docker processes
```
docker ps
```

## Step 2 - Check the Web Site is Working or Not
Check that Weather our website(which is hosted with nginx on container) is working or not 
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

## Step 3 - Modify the Contents of Web Page 
Now in this step we want to modify the content the contents of webpage<br/>
So 1st we want to get the access of bash of Our Running container 
* exec -- It is use to Run a command in a running container
* -it -- the -i (interactive) flag to keep stdin open and -t to allocate a terminal.
* 093d97214496 -- it your nginx running container ID .
* /bin/bash -- it is a command which you want to execute .
```
docker exec -it 093d97214496 /bin/bash
```

When you getting running container terminal execute following command 
```
echo 'hello this for testing' > /usr/share/nginx/html/index.html
exit
```

## Step 4 - Check Weather Modify Contents are Display or Not 
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

## Step 5 - Create the Image From Running Container 
In This step we want to create image of our nginx running container 
* commit -- Create a new image from a container's changes
* jovial_mendeleev -- it is name of our running nginx container 
* nginx_modify:1 -- it name of your new image and tag 
```
docker commit jovial_mendeleev nginx_modify:1
```

To check that weather your new image is created or not 
```
docker images
```

## Step 6 - Stop and Remove All Containers
Now it time to stop your all running containers and so we get all port free 
* stop -- is use to stop the running container with that we provide the container ID
* rm -- is use to remove the container with that we provide the container ID
```
docker stop 093d97214496 efabb59050c2
docker rm 093d97214496 efabb59050c2 
```
## Step 7 - Run New container With Our Created Image
In this step we are going to start new container with our new nginx image 
```
docker run -p 8080:80 -d nginx_modify:1
```

## Step 8 - Check That Weather Our Modify Contents Are Display or Not
And Finally we are going test the weather our new image is show in modify contents or not 
```
wget http://localhost:8080
```

To see the contents of **index.html** file
```
cat index.html
```
If you get **hello this for testing** then **we did it.**
