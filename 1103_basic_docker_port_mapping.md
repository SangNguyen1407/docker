## What is port mapping?
The clients (a browser, API calls, etc) connect the container in Docker

Local machine (a web browser) |http:// port 80| ----> |(port 80)| | container Nginx |

## What is nginx


## Step by step for port mapping to container Nginx

1_1. Run ssh directly in to container 
```
docker run -d nginx
docker ps
docker exec -it <container_id> sh

curl localhost
```

1_2. Run port mapping into container with port 80
```
PS D:\Docker> docker run -p <target_port>:<container_port> -d nginx
PS D:\Docker> docker ps
CONTAINER ID   IMAGE     COMMAND                   CREATED          STATUS          PORTS                                 NAMES
1a8ee90f4126   nginx     "/docker-entrypoint.…"   17 seconds ago   Up 16 seconds   0.0.0.0:80->80/tcp, [::]:80->80/tcp   charming_hamilton

Open web browser and run localhost
```

2. Log trace
Show log in container
```
PS D:\Docker> docker logs -f 1a8ee90f4126
```
