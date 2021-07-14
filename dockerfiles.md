# Dockerfile Examples

Node
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

Node Express Server
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

React
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
Angular using Node plus Nginx Alpine
```
FROM node as builder

WORKDIR /app

COPY . .

RUN npm i && npm run build

FROM nginx:alpine

WORKDIR /usr/share/nginx/html

RUN rm -rf ./*

COPY --from=builder /app/dist/carDealerClient .

ENTRYPOINT ["nginx", "-g", "daemon off;"]
```

.NET Web API Example
```

FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS base
WORKDIR /app
EXPOSE 5000

ENV ASPNETCORE_URLS=http://*:5000
ENV ASPNETCORE_ENVIRONMENT=Development

FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /src
COPY ["CarDealerWebAPI/CarDealerWebAPI.csproj", "CarDealerWebAPI/"]
RUN dotnet restore "CarDealerWebAPI\CarDealerWebAPI.csproj"
COPY . .
WORKDIR "/src/CarDealerWebAPI"
RUN dotnet build "CarDealerWebAPI.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "CarDealerWebAPI.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "CarDealerWebAPI.dll"]
```

Java Spring Web API
```
FROM openjdk:8
ADD target/demo-0.0.1-SNAPSHOT.jar demo-0.0.1-SNAPSHOT.jar
ENTRYPOINT ["java", "-jar","demo-0.0.1-SNAPSHOT.jar"]
EXPOSE 8080
```

