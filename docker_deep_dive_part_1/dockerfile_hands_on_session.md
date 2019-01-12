# Dockerfile

## FROM

```
FROM <image> [AS <name>]
FROM <image>[:<tag>] [AS <name>]
```

- It initializes a new build stage and sets the base image for subsequent instructions.
- A valid Dockerfile must start from a FROM instruction.
- ARG is the only instruction that precedes the FROM instruction.

### Lab 1

Create a file with name **from** and paste the following content into it:

```
ARG CODE_VERSION=latest
FROM ubuntu:${CODE_VERSION}
```

Execute following commands:

```
docker build -f from -t sagarj/from:1 .

docker run sagarj/from:1 
```

## RUN

```
RUN <command>
RUN ["executable", "param1", "param2"]
```

- It will execute any commands in a new layer on top of the current image and commit the results. The resulting committed image will be used for the next step in the Dockerfile.

### Lab 2

Create a file with name **run** and paste the following content into it:

```
ARG CODE_VERSION=latest
FROM ubuntu:${CODE_VERSION}
RUN echo hello from shellform
RUN [“echo”, “hello from execform”]
```

Execute the following commands:

```
docker build -f run -t sagarj/run:1 .

docker run sagarj/run:1 
```

## CMD

```
CMD command param1 param2
CMD ["executable","param1","param2"]
```

- There can only be one CMD instruction in a Dockerfile. If you list more than one CMD then only the last CMD will take effect.
- The main purpose of a CMD is to provide defaults for an executing container.

### Lab 3:

Create a file with name **cmd** and paste the following content into it:

```
ARG CODE_VERSION=latest
FROM ubuntu:${CODE_VERSION}
CMD echo hello
```
Execute the following commands:

```
docker build -f from -t sagarj/cmd:1 .

docker run sagarj/cmd:1 
```

## LABEL

```
LABEL <key>=<value> <key>=<value> <key>=<value> ...
```

- The LABEL instruction adds metadata to an image. A LABEL is a key-value pair.
- docker inspect command is used to view the label’s

### Lab 4

Create a file with name **label** and paste the following content into it:

```
ARG CODE_VERSION=latest
FROM ubuntu:${CODE_VERSION}
LABEL AUTHOR=“SAGAR JADHAV”
```

Execute the following commands:

```
docker build -f from -t sagarj/label:1 .

docker inspect sagarj/label:1
```

## EXPOSE

```
EXPOSE <port> [<port>/<protocol>...]
```

- The EXPOSE instruction informs Docker that the container listens on the specified network ports at runtime.
- The EXPOSE instruction does not actually publish the port. It functions as a type of documentation between the person who builds the image and the person who runs the container, about which ports are intended to be published
- To actually publish the port when running the container, use the -p flag on docker run

### Lab 5

Create a file with name **expose** and paste the following content into it:

```
ARG CODE_VERSION=latest
FROM tomcat:${CODE_VERSION}
EXPOSE 8080
```

Execute the following commands:

```
docker build -f from -t sagarj/expose:1 .

docker inspect sagarj/expose:1
```

## ENV

```
ENV <key> <value>
ENV <key>=<value> ...
```

- The ENV instruction sets the environment variable <key> to the value <value>. This value will be in the environment for all subsequent instructions in the build stage.
- The environment variables set using ENV will persist when a container is run from the resulting image.

### Lab 6

Create a file with name **env** and paste the following content into it:

```
ARG CODE_VERSION=latest
FROM ubuntu:${CODE_VERSION}
ENV NAME=SAGAR
RUN echo ${NAME}
CMD echo ${NAME}
```

Execute the following commands:

```
docker build -f from -t sagarj/env:1 .

docker run sagarj/env:1

docker run --env NAME=SUNNY sagarj/env:1

docker inspect sagarj/env:1
```

## ADD & COPY

```
ADD [--chown=<user>:<group>] <src>... <dest>
ADD [--chown=<user>:<group>] ["<src>",... "<dest>"] (Use for Path Containing Whitespaces)

COPY [--chown=<user>:<group>] <src>... <dest>
COPY [--chown=<user>:<group>] ["<src>",... "<dest>"]
```

- COPY and ADD are both Dockerfile instructions that serve similar purposes. They let you copy files from a specific location into a Docker image.
- COPY takes in a src and destination. It only lets you copy in a local file or directory from your host (the machine building the Docker image) into the Docker image itself.
- ADD lets you do that too, but it also supports 2 other sources. First, you can use a URL instead of a local file / directory. Secondly, you can extract a tar file from the source directly into the destination.

### Lab 7

Create a file with name **addcopy** and paste the following content into it:

```
ARG CODE_VERSION=latest
FROM ubuntu:${CODE_VERSION}
COPY ./copy/test_copy /
ADD ./add/test_add /
ADD https://golang.org/doc/install?download=go1.11.4.linux-amd64.tar.gz /
CMD cat test_copy && test_add && ls
```

Execute the following commands:

```
mkdir add
cd ./add
echo “HELLO ADD” > test_add
cd ..
mkdir copy
cd ./copy
echo “HELLO COPY” > test_copy
cd ..

docker build -f from -t sagarj/addcopy:1 .

docker run sagarj/addcopy:1
```

## ENTRYPOINT

```
ENTRYPOINT ["executable", "param1", "param2"] (exec form, preferred)
ENTRYPOINT command param1 param2 (shell form)
```

- An ENTRYPOINT allows you to configure a container that will run as an executable.

### Lab 8

- Create a file with name **entrypoint** and paste the following content into it:

```
ARG CODE_VERSION=latest
FROM ubuntu:${CODE_VERSION}
ENTRYPOINT [“echo”, “hello”]
```

- Execute the following commands:

```
docker build -f from -t sagarj/entrypoint:1 .

docker run sagarj/entrypoint:1 
```

## WORKDIR

```
WORKDIR /path/to/workdir
```

- The WORKDIR instruction sets the working directory for any RUN, CMD, ENTRYPOINT, COPY and ADD instructions that follow it in the Dockerfile. If the WORKDIR doesn’t exist, it will be created even if it’s not used in any subsequent Dockerfile instruction.

### Lab 9

- Create a file with name **workdir** and paste the following content into it:

```
ARG CODE_VERSION=latest
FROM ubuntu:${CODE_VERSION}
WORKDIR /data
RUN echo “hello” > test
ENTRYPOINT [“cat”, “test”]
```

- Execute the following commands:

```
docker build -f from -t sagarj/workdir:1 .

docker run sagarj/workdir:1 
```
