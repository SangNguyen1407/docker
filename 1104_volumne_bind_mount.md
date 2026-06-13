# Volumne (bind_mount)

## What is volumne?
A Docker volumne indicates the partition of memory that Docker used to persist data inside container. When the container stops, retarts this data does not disappear

## Why use bind mount?

   | postgres | ------------>  |pgdata|

```
syntax
docker volume create <volume_name>
docker run -v <local_dir/volume>:<container_dir>
```
```
docker volume create pgdata

docker run -v pgdata:<path> -p 5432:5432 postgres
  default path: /var/lib/postgresql/data
docker run -v pgdata:/var/lib/postgresql/data -p 5432:5432 postgres

docker run -v "C:\users\html":/usr/share/nginx/html -p 80:80 nginx
```