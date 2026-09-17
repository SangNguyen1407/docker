## What is Alpine?

Alpine is lightweight security-focused Linux distribution.

It is used as a base image for Docker containers with 5MB size.

(it is significantly smaller than other Linux-based Docker image such as CentOS or Ubuntu)

## Why is Alpine Linux preferred in Docker?

Alpine Linux is preferred because it is minialistic, secure and has a small footprint.

It is significantly smaller than other Linux-based Docker image such as CentOS or Ubuntu.


## Intall Docker Alpine Image
```
docker --version
docker pull alpine
docker run -it alpine
 with
  -i (--interactive) keep STDIN open even for not attached
  -t (--tty) allocate a pseudo-TTY(TeleTYpewrite)
```

## Run/Exit/Attach/Detach Alpine Image
```
①Run alpine
PS D:\Docker> docker run -it alpine
/ # 

② To exit/ attach/ detach Alpine Image
*To exit the shell within a container, press Ctrl+D
PS D:\Docker> docker run -it alpine
/ # (➡ Ctrl+D)
PS D:\Docker\Docker> docker ps -a
CONTAINER ID   IMAGE     COMMAND     CREATED         STATUS                      PORTS     NAMES
1e5b02519df7   alpine    "/bin/sh"   6 minutes ago   Exited (0) 24 seconds ago             jovial_golick

*To detach mode: using Ctrl+P+Q
*To attach mode: docker attach <cotainer_id>
PS D:\Docker\Docker> docker run -it alpine
/ #  (➡ Ctrl+P+Q)
PS D:\Docker\Docker> docker ps -a
CONTAINER ID   IMAGE     COMMAND     CREATED          STATUS                     PORTS     NAMES
2192d05afa62   alpine    "/bin/sh"   18 seconds ago   Up 17 seconds                        brave_matsumoto
1e5b02519df7   alpine    "/bin/sh"   11 minutes ago   Exited (0) 6 minutes ago             jovial_golick
PS D:\Docker\Docker> docker attach 2192d05afa62
/ #
```
## SSH into Alpine Image
Using exec command for running a child process inside container
```
PS D:\Docker\Docker> docker ps
CONTAINER ID   IMAGE     COMMAND     CREATED              STATUS              PORTS     NAMES
ca1658277f58   alpine    "/bin/sh"   About a minute ago   Up About a minute             elated_hypatia

PS D:\Docker\Docker> docker exec -it ca1658277f58 sh
/ # cat /etc/os-release
NAME="Alpine Linux"
ID=alpine
VERSION_ID=3.23.4
PRETTY_NAME="Alpine Linux v3.23"
HOME_URL="https://alpinelinux.org/"
BUG_REPORT_URL="https://gitlab.alpinelinux.org/alpine/aports/-/issues"
/ #(➡ Ctrl+D)

What's next:
    Try Docker Debug for seamless, persistent debugging tools in any container or image → docker debug ca1658277f58
    Learn more at https://docs.docker.com/go/debug-cli/

PS D:\Docker\Docker> docker ps
CONTAINER ID   IMAGE     COMMAND     CREATED         STATUS         PORTS     NAMES
ca1658277f58   alpine    "/bin/sh"   4 minutes ago   Up 4 minutes             elated_hypatia

PS D:\Docker\Docker>
```

## Create a Dockerfile Using Alpine
# Use Alpine as the base image

1. Create Dockerfile file and add ther following line:
```
FROM alpine:latest
# Install a package (e.g., curl)
RUN apk add --no-cache curl
# Set default command
CMD ["sh"]
```
2. Build a Docker Image
```
docker build -t my-alpine-app .
```
3. Run Alpine Image
```
docker run -it my-alpine-app
```