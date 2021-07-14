Node Dockerfile example
```# How to add arg to build...
# ARG BASE_IMAGE
# FROM $BASE_IMAGE

FROM node:latest



# Create working directory
WORKDIR /usr/src/app
# Install app dependencies
# A wildcard is used to ensure both package.json AND package-lock.json are copied
# where available (npm@5+)
COPY package*.json ./
COPY . .
RUN npm install

# How to add arg to ENV...
# ARG INPUT_PORT
# ENV PORT $INPUT_PORT
# EXPOSE $PORT

EXPOSE 4000
CMD ["node", "server.js"]
```

Node Express Server Example
```
# ARG BASE_IMAGE
# FROM $BASE_IMAGE

FROM node:latest



# Create working directory
WORKDIR /usr/src/app
# Install app dependencies
# A wildcard is used to ensure both package.json AND package-lock.json are copied
# where available (npm@5+)
COPY package*.json ./
COPY . .
RUN npm install

# How to add arg to ENV...
# ARG INPUT_PORT
# ENV PORT $INPUT_PORT
# EXPOSE $PORT

EXPOSE 4000
CMD ["node", "server.js"]
```

React Example
```
FROM node:latest
WORKDIR /app
ENV PATH /app/nodes_modules/.bin:$PATH
COPY package.json ./
COPY package-lock.json ./
RUN npm i
COPY . ./
CMD ["npm", "start"]
```
