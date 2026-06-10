## Volumns bind_mount

   | postgres | ------------>  |pgdata|

```
docker volume create <volume_name>
docker run -v <local_dir/volume>:<container_dir>

for example:
docker volume create pgdata

docker run -v pgdata:<path> -p 5432:5432 postgres
  default path: /var/lib/postgresql/data
docker run -v pgdata:/var/lib/postgresql/data -p 5432:5432 postgres

docker run -v "C:\users\html":/usr/share/nginx/html -p 80:80 nginx
```
