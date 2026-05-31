# Dockerfile for backend API
# just using apline as lack storage 
FROM node:18-alpine

WORKDIR /app

# first copied package
COPY package*.json ./

# ran to install whats needed
RUN npm install --omit=dev

# copy code
COPY . .

# dont have admin acess 
USER node

# push out port 5000 as it matches the code 
EXPOSE 5000

# start it up
CMD ["node", "server.js"]