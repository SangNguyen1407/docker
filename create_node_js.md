# Ceate basic Node.js project
## Create node.js folder 
mkdir node_app
cd node_app

## Create node.js config file and docker file
1. Create package.json file
```
{
  "name": "my-node-app",
  "version": "1.0.0",
  "main": "server.js",
  "scripts": {
    "start": "node server.js"
  }
}
```
2. Create server.js file
```
const http = require("http");

const server = http.createServer((req, res) => {
  res.end("Hello from Node.js in Docker!");
});

server.listen(3000, () => {
  console.log("Server running on port 3000");
});
```
3. Create docker file
```
# create image Node.js 
FROM node:18-alpine

# Create app folder
WORKDIR /app

# Copy file package.json file
COPY package.json .

# Install dependencies
RUN npm install

# Copy source code
COPY . .

# run app
CMD ["npm", "start"]
```
## Build and Run Node.js
docker build -t my-node-app .
docker run -p 3000:3000 my-node-app
In Browser, run http://localhost:3000


