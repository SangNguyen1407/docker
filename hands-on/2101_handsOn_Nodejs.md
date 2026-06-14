# Hands-on NODEJS project
## Step by step
1. Create Nodejs source file structure
```
public
Dockerfile (➡ step 2: create new file)
package.json
server.js
(In server.js file, updated the following code)
app.listen(3000, () => {
  console.log('Server running on port 3000');
});
```
2. Create Dockerfile file

Create Dockerfile file with the content

```
FROM node:18-alpine
WORKDIR /app
COPY . /app
RUN npm install
EXPOSE 3000
CMD ["node", "server.js"]
```

3. Build container with Dockerfile

Syntax
```
docker build -t <container_name> <Dockerfile_path>
```
for example:

docker build -t mynode .

4.  Run nodeJS in Docker container

Syntax
```
docker run -p <port>:<port> <container>
```
for example:

docker run -p 3000:3000 4dee65993893

5. Web browser with localhost:3000
