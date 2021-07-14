# Docker-compose Examples

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
.NET WEb API with SQL

```
# Please refer https://aka.ms/HTTPSinContainer on how to setup an https developer certificate for your ASP .NET Core service.

version: '3.4'

services:
  sqlserver:
    image: "mcr.microsoft.com/mssql/server:2019-latest"
    user: root
    volumes:
      - sql-data:/var/opt/mssql/data
    ports:
      - "1433:1433"
    environment:
      - ACCEPT_EULA=Y
      - SA_PASSWORD=Password123
  cardealerwebapi:
    image: cardealerwebapi
    build:
      context: .
      dockerfile: CarDealerWebAPI/Dockerfile
    ports:
      - 5000:5000
    depends_on:
      - sqlserver
    restart: always
volumes:
  sql-data:
  ```
