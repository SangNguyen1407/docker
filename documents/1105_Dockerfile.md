# Create Dockerfile

## What is Dockefile?
Dockerfile is a template that instruct/tells Docker to build image step by step

Running Dockerfile is not changed exsisting image, instead of, Docker is:

1. Reads each instruction line in Dockerfile

2. Excutes all line in this file

3. Creates a new image layer for each instruction

4. Produces a brand‑new image at the end

Notices: [layers] 
➡when run Dockerfile step by step, the image is not changed, and Docker create new layer for each instruction in image. This layer is changed into image.

for example:

1. I have COPY command in the Dockerfile to copy file from local directory into container directory(Docker creates a new layer into container and presist file from local directory. )
```
FROM node:alpine
WORKDIR /server
COPY ./*.json ./
CMD["node", "app.js"]
```

Runs the Alpine Linux image (tagged latest) having Node.js as the base layer.

Sets /server as the working directory for all the following instructions.

Copies all JSON file in local directory into the container directory.

Sets the default command to start the application by running app.js.

2. I want to run bash when container starts

The Dockerfile contains the following instructions:
```
FROM alpine:latest
RUN apk add bash
CMD ["bash"]
```
Runs the Alpine Linux image (tagged latest) as the base layer.

Installs bash package as next steps and becomes a new image layer.

Set default command when a container starts.


## How to run Dockerfile
```
docker build -t <image_name>:<tag> <Dockerfile_path>
```
<Dockerfile_path> refers to the folder that contains Dockerfile.

All Dockerfile insides this folder is called for building to Docker image.
```
PS D:\> docker build -t lated_hypatia .\Docker\Dockerfile\
... (log)
PS D:\> docker run -it alpine
/ # (bash is run)
```

## Dockerfile keywords
```
FROM <image_name>
RUN <command>
WORKDIR <directory>
COPY <src> <dst>
ADD <src/URL> <dst>
EXPOSE <port>
CMD ["command", "arg1", "arg2", ...]
```

## Other samples with Dockerfile

1. The Dockerfile contains the following instructions (backend java):
```
FROM openjdk:8-jre
EXPOSE 8080
WORKDIR /app
COPY ./target/app-1.0-SNAPSHOT.jar .
CMD["java", "-jar", "app-1.0-SNAPSHOT.jar"]
```
Using the Linux having OpenJDK 8 JRE image as the base layer

The container listens on port 8080 (EXPOSE 8080 as comment)

Sets /app as the working directory for all the following instructions.

Copies JAR file from local directory into the container /app directory

When the container starts, run jar

2. The Dockerfile contains the following instructions (frontend ExpressJS):
```
FROM node:alpine
WORKDIR /server
COPY ./node_modules ./node_modules
COPY ./*.js ./
COPY ./public ./public
CMD["node", "app.js"]
```
Runs the Alpine Linux image (tagged latest) having Node.js as the base layer.

Sets /server as the working directory for all the following instructions.

1.Copies all node_modules file in local directory into the container directory.

2.Copies all JS file in local directory into the container directory.

3.Copies local public folder into the container public folder.

Sets the default command to start the application by running app.js.

* Having 3 COPY command in Dockerfile, should it(copy command) arrage or random to use COPY command? OR can use 1 COPY command for Dockerfile?

Docker uses hash values to detect whether a layer has changed or not.

When a layer is considered changed, Docker rebuilds that layer and all layers that come after it.

In this example, the node_module directory contains the project's installed dependencies. Each project has its own node_modules folder, and it not often changed. In contract, source code folder is always changes during development.

Therefore, it make sense seperate these into 2 layers 

1. [COPY ./node_modules ./node_modules]

2. [COPY <source_code_in_local_directory>  <source_code_in_container_directory>] 

This allows Docker to reuse the cached node_modules layer and only rebuild the layer that contains the source code.





