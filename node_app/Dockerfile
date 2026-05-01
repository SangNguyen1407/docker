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