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
```
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
```
1. docker version
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

The important fields, needs to check, are Client version, 
Server(Engine) (run docker info command), API version, 
```
```
2. docker ps -a
show all container(running or stopped) on the system.
In STATUS fields, this info shows container status of containers.
for example, 
①　Created                       → container is not started yet
①　Up 5 minutes                  → container is running
②　Exited (0) 2 hours ago        → container is stopped
③　Restarting (1) 5 seconds ago　→ container is restarted

CONTAINER ID   IMAGE         COMMAND        CREATED        STATUS                     PORTS     NAMES
a1b2c3d4e5f6   nginx         "/docker..."   2 hours ago    Exited (0) 1 hour ago                webserver
b7c8d9e0f1a2   hello-world   "/hello"       3 hours ago    Exited (0) 3 hours ago               hopeful_morse

Exit code matter:
・Exited (0)   → normal exit
・Exited (1)   → error
・Exited (137) → killed (forced stop)

Filter container status:
①docker ps -a --filter "status=created"
②docker ps --filter "status=running"
③docker ps -a --filter "status=exited"
④docker ps -a --filter "status=restarting"
```
```
3. docker container stop/start <container_id>

```
