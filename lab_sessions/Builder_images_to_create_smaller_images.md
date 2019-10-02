# Builder images to create smaller final images
In this lab session we are going to write efficient Dockerfiles to create smaller images using builder images
## Step 1 :- Write the simple Go program
In this step we are going to write simple Go program which takes run time arguments and creates an executable <br/> 
Create the file **main.go** put below code in it and save it.
```
package main
import (
    "fmt"
)
func main() {
    fmt.Println("Sagar says, Hello!")
}
```
## Step 2 :- Write the Dockerfile 
In this step we are going to write the dockerfile which load the base image as a builder image, copy our contents to that image, build our Go program, then copy the executable created to the other smaller, final image and execute it. <br/>
create the file **Dockerfile** put below code in it and save it.
```
ARG CODE_VERSION=latest
FROM golang:${CODE_VERSION} as builder
WORKDIR /app
COPY . .
RUN go build main.go

FROM alpine:${CODE_VERSION}
WORKDIR /root/
COPY --from=builder /app/main .
CMD ["./main"]
```

The **`as`** keyword is used to provide an alias to the builder image so that files in it's filesystem can be referenced later in the Dockerfile.

**`--from`** will reference the alias of the builder image to access it's filesystem and copy the executable from the builder image's WOKRDIR to the final image's WORKDIR

## Step 3 :- Build the image using dockerfile
In this step we are going to build the image using dockerfile 
```
docker build -f Dockerfile -t demo/docker-builder:1 . 
```
Run `docker images | grep demo/docker-builder` and check the size of the image

```
docker-tut-builder  latest  802d8ccd4758    About a minute ago   8.14MB
```

But if we see the base builder image, we see that it is much, much bigger (350x)

```
golang              alpine   48260c3da24c   6 days ago           359MB
```

## Step 4 :- Run the Container using our image which perform our particular task 
In this step we are going to run the container using our image 
```
docker run -it demo/docker-builder:1
```

We should get such an output: 

```
$ docker run -it demo/docker-builder:1
Sagar says, Hello!
```
