# Runing Node.js + MySQL in a container
1.Installs MySQL server
2.Installs Node.js
3.Starts both services inside the same container

## Create Dockerfile
```
FROM ubuntu:22.04

# Install MySQL + Node.js
RUN apt-get update && apt-get install -y \
    mysql-server \
    curl \
    && curl -fsSL https://deb.nodesource.com/setup_18.x | bash - \
    && apt-get install -y nodejs \
    && apt-get clean

# MySQL setup
RUN mkdir -p /var/run/mysqld && chown -R mysql:mysql /var/run/mysqld
RUN service mysql start && \
    mysql -e "CREATE DATABASE mydb;" && \
    mysql -e "ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '123456';"

# Copy Node.js app
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .

# Install supervisor
RUN apt-get install -y supervisor
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

EXPOSE 3000 3306

CMD ["/usr/bin/supervisord"]
```
## Create supervisord.conf file
```
[supervisord]
nodaemon=true

[program:mysql]
command=/usr/bin/mysqld_safe
autostart=true
autorestart=true

[program:node]
command=node server.js
directory=/app
autostart=true
autorestart=true
```
## Build the container
docker build -t node_mysql_app .

## Run the container
docker run -p 3000:3000 -p 3306:3306 node_mysql_app

