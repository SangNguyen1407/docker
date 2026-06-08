# Docker Introduction
## what is difference Docker and WSL?
Docker is platform for running containers.  
・Docker uses images to create containers  
・Docker Engine manges and runs many containers  
・Using services like MySQL, Redis, Nginx  
・Using work with microservices  
WSL(Windows SubSystem for Linux): run a real Linux distribution inside Windows  
・can install Ubuntu, Debian, Kali, etc  
・VM no required  
・can run Linux commnands, tools, and development environments  
(In summary, WSL is not Docker, Docker using WSL to run container)  
・Using to run a full Linux envrionment for develop software in Linux  
(means build source code in Linux)  
・Want to Linux tool (Python, Node, bash, apt, etc)  
## Docker basic Commands

syntax

docker <component> <command>

・component: image, container, network, volume

・command: ls, run, exec, stop, pull, push

```
① With component image
・Download the image from a registry to local machine
docker image pull <image>
docker image pull <image>:<tag>
・Download the image from local machine to a registry
docker image push <image>
docker image push <image>:<tag>
・List all images
docker images OR docker image ls
・Other common command:
docker image prune (Delete unused images from local system)

Exception: (short command)
docker pull
docker push

② With component container
・Create a new container from image and start it
docker container run <image>
・List container is running (even container is shutdown)
docker container ls OR docker container ls -a
docker ps OR docker ps -a
・Delete unused container
docker container prune
・Run a command inside an already running container.
docker container exec <container_id> <command>

Exception: (short command)
docker run
docker stop
docker exec

③ Other:
docker version
docker info 
docker images 
docker ps 
docker ps -a 
docker stats
docker network ls
docker container stop/start <container_id>
docker rm -v <container_id_or_name>
```

## Using WSL to run Ubuntu
```
// check state(Runing or stopped) and version
wsl -l -v
// Install Ubuntu
wsl --install
// Run Ubuntu
wsl -d Ubuntu(wsl -u root)
// Change root pass Ubuntu
wsl -u root
passwd
// Exit Ubuntu
exit
```
## Detail commands info
### 1. Show docker version 
```
docker version
Client:
 Version:           29.4.1
 API version:       1.54
 Go version:        go1.26.2
 Git commit:        055a478
 Built:             Mon Apr 20 16:35:45 2026
  Experimental:     false
 containerd:
  Version:          v2.2.3
  GitCommit:        77c84241c7cbdd9b4eca2591793e3d4f4317c590
 runc:
  Version:          1.3.5
  GitCommit:        v1.3.5-0-g488fc13e
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0
```
The important fields, needs to check, are Client version, 
Server(Engine) (run docker info command), API version, 

### 2. Show all container
```
Container life cycle

docker run ----> (UP) --docker stop/kill--> (EXITED) -----docker rm----->  (DEAD)
                  |                          |  docker container prune
                  |                          |
                  |<-----docker start--------|
```
Run "docker run" command to excute a main process. When the main process ends run "docker stop <container_id>"and "docker rm" to delete the container completely.

The main process has PID=1, the container will remain alive when the PID=1 process is alive.

When using "docker stop" command, Docker will send SIGTERM (termination signal) to stop container of PID=1 process and container status is EXITED(0).

After recevice SIGTERM within 10s, if container does not exit, Docker will send SIGKILL (kill signal), the container will stop and status is EXITED(137).

The container has 2 mode: Attach(eg: attach linux termial into container), and Detach

Attach: Ctrl + D (exit container and container status is Exited(0))

Dettach: Ctrl + P + D(When attach the container again, using "docker attach <container_id>")

(when run with detach container, using "docker run -d <container>")

```
docker ps -a
CONTAINER ID   IMAGE         COMMAND        CREATED        STATUS                     PORTS     NAMES
a1b2c3d4e5f6   nginx         "/docker..."   2 hours ago    Exited (0) 1 hour ago                webserver
b7c8d9e0f1a2   hello-world   "/hello"       3 hours ago    Exited (0) 3 hours ago               hopeful_morse
```
show all container(running or stopped) on the system.

In STATUS fields, this info shows container status of containers.

for example, 

①　Created                       → container is not started yet

①　Up 5 minutes                  → container is running

②　Exited (0) 2 hours ago        → container is stopped

③　Restarting (1) 5 seconds ago　→ container is restarted

Exit code matter:

・Exited (0)   → normal exit

・Exited (1)   → error

・Exited (137) → killed (forced stop)

・Exited (130) → killed (user stop this container)

Filter container status:

①docker ps -a --filter "status=created"

②docker ps --filter "status=running"

③docker ps -a --filter "status=exited"

④docker ps -a --filter "status=restarting"


### 3. Start/Stop and Remove
```
docker container stop/start <container_id>
docker container rm  <container_id>

```
* Sends a SIGTERM signal to the container

* Gives the main process time to shut down cleanly

* If it doesn’t stop in time, Docker sends SIGKILL


### 4. Shows all network 

```
$ docker network ls
NETWORK ID     NAME               DRIVER    SCOPE
d41baab1c6b9   bridge             bridge    local
c3c2ba7161db   host               host      local
aeef3df47d0e   none               null      local
```
when installing Docker,it automatically creates 3 networks(bridge, host, none).
Create network in docker 
```
$ docker network create mynet
$ docker network ls
NETWORK ID     NAME               DRIVER    SCOPE
d41baab1c6b9   bridge             bridge    local
c3c2ba7161db   host               host      local
9a2f9efd2d88   mynet              bridge    local
aeef3df47d0e   none               null      local

$ docker run --net=mynet
```
### 5. Inspect the bridge network to see connected containers:
$ docker network inspect bridge
```
[
    {
        "Name": "bridge",
        "Id": "d41baab1c6b96bda48135666bd4a876c912ade1f070bba83b6e0977686618ef2",
        "Created": "2026-05-10T05:58:18.7787197Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv4": true,
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "172.17.0.0/16",
                    "Gateway": "172.17.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Options": {
            "com.docker.network.bridge.default_bridge": "true",
            "com.docker.network.bridge.enable_icc": "true",
            "com.docker.network.bridge.enable_ip_masquerade": "true",
            "com.docker.network.bridge.host_binding_ipv4": "0.0.0.0",
            "com.docker.network.bridge.name": "docker0",
            "com.docker.network.driver.mtu": "1500"
        },
        "Labels": {},
        "Containers": {},
        "Status": {
            "IPAM": {
                "Subnets": {
                    "172.17.0.0/16": {
                        "IPsInUse": 3,
                        "DynamicIPsAvailable": 65533
                    }
                }
            }
        }
    }
]
```
### 6. Disconnect a container from a user-defined bridge
To disconnect a running container from a user-defined bridge, use the docker network disconnect command.
```
$ docker network ls
NETWORK ID     NAME               DRIVER    SCOPE
d41baab1c6b9   bridge             bridge    local
c3c2ba7161db   host               host      local
9a2f9efd2d88   mynet              bridge    local
aeef3df47d0e   none               null      local

$ docker run -d --name my-nginx nginx

$ docker ps -a
CONTAINER ID   IMAGE     COMMAND                   CREATED          STATUS          PORTS     NAMES
71ae62d8d90b   nginx     "/docker-entrypoint.…"   6 seconds ago    Up 6 seconds    80/tcp    my-nginx
8a79c4a4a74b   alpine    "sleep 5000"              33 minutes ago   Up 33 minutes             sleeping-alpine

$ docker create --name my-nginx --network mynet --publish 8080:80 nginx:latest
(if my-nginx container existed, remove container: docker rm my-nginx)
$ docker start my-nginx
$ docker network disconnect mynet my-nginx

```

### 7. Excute command insides container

Syntax
・ docker exec <container_id><command>


