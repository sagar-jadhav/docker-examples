#Dockerize Shell Script Simple 
In this lab session we are going to dockerize shell script 
##Step 1 :- Write the simple shell script
In this step we are going to write simple shell script which take run time arguments <br/> 
create the file **test.sh** put below code in it and save it.
```
#!/bin/bash
echo "First Name: $FIRST"
echo "Last Name: $LAST"
```
##Step 2 :- Write the dockerfile 
In this step we are going to write the dockerfile which load the base image ,copy our contents to that image ,run our particular commands . <br/>
create the file **docfile** put below code in it and save it.
```
ARG CODE_VERSION=latest
FROM ubuntu:${CODE_VERSION}
COPY ./test.sh /
ENV FIRST=SAGAR
ENV LAST=JADHAV
RUN chmod u+x /test.sh
ENTRYPOINT ["/test.sh"]
```
##Step 3 :- Build the image using dockerfile
In this step we are going to build the image using docker file 
```
docker build -f docfile -t shell_script:1 . 
```
##Step 4 :- Run the Container using our image which perform our particular task 
In this step we are going to run the container using our image 
```
docker run shell_script:1
docker run -e FIRST=AKSHAY -e LAST=ITHAPE shell_script:1
```
