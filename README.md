# Docker Introduction
## Docker basic Commands
```
docker version
//一般的なコマンド。システム全体の情報を表示する
docker info 
docker images 
docker ps 
docker ps -a 
docker status
```

## Remove container
```
docker ps -a 
docker rm -v <container_id_or_name>
```

## Run Ubuntu
```
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