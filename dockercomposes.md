
MERN Stack Example (Mongo, Express, React, Node)
```
services:
  react:
    tty: true
    build:
      context: ./ClientApp
      dockerfile: Dockerfile
    ports: 
      - "3001:3000"
    environment: 
      - CHOKIDAR_USEPOLLING=true
  node:
    tty: true
    build:
      context: ./node
      dockerfile: Dockerfile
    restart: always
    ports: 
      - "8081:8080"
  mongo:
    container_name: mongodb
    image: mongo
    restart: always
    volumes:
      - ./mongoDb:/data/db
    ports:
      - "27017:27017"
    command: [--auth]
      #  environment:
  #    MONGO_INITDB_ROOT_USERNAME: root
  #    MONGO_INITDB_ROOT_PASSWORD: example
```
