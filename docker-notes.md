# Docker Commands

## View Containers
<pre>
docker container ls
</pre>
<pre>
docker ps
</pre>

Flags:
<pre>
-a = show all containers

-q = show all active container ids

-ql = show last active container id

-qa = all container ids

docker start $(docker ps -ql)

docker ps -qa | xargs docker stop
</pre>

## Inspect Container
<pre>
docker inspect <b>container_id</b> || <b>container_name</b> 
</pre>

## Start Container

<pre>
docker start <b>container_id</b> || <b>container_name</b> 
</pre>

## Stop Container

<pre>
docker stop <b>container_id</b> || <b>container_name</b>
</pre>

## Delete Container
<pre>
docker rm <b>container_id</b> || <b>container_name</b>
</pre>

Delete all:
<pre>
docker ps -qa | xargs docker rm -f
</pre>

## Create Container
<pre>
docker run <b>base_image_name:tag_name</b> + <b>args_for_ENTRYPOINT</b> --name <b>name_container</b>
</pre>

- If no local image found, runs Docker pull to check Docker Hub

-it = shell into the running container


## Rename Container
<pre>
docker rename <b>container_id</b> || <b>old_container_name</b> <b>new_container_name</b>
</pre>

## Remove Container
<pre>
docker rm <b>container_id</b> || <b>container_name</b> 
</pre>

## Get Image
<pre>
docker pull <b>username</b>/<b>image_name:tag_name</b>
</pre>

- Default tag name is latest

## View Images

docker images

## View Image Layers
<pre>
docker <b>name_of_image:tag_name</b> history
</pre>

## Create Image

### Standard

#### 1. Create Dockerfile

Dockerfile:
<pre>
FROM <b>base_image_name:tag_name</b>
RUN <b>install x && install y</b>
RUN <b>install y</b>
COPY <b>file_name</b> + <b>destination</b>
WORKDIR <b>starting_directory_for_CMD</b>
CMD <b>npm start</b>
ENTRYPOINT ["<b>command</b>", "<b>command</b>", "<b>command</b>"]
MAINTAINER <b>author_name</b> + <b>author_email</b>
EXPOSE <b>port_number</b>
ENV <b>environment_var</b>
USER <b>command_line_user</b>
</pre>
- RUN installs, commands end after execution always put before COPY src

- CMD only executed once per Dockerfile as default command

- ENTRYPOINT allows for args to be passed in via terminal with docker run to be executed by ENTRYPOINT overriding a potentially a default CMD

- COPY copies directories recursively. All files in the root directory can be selected with . The root directory of the  build starts with / for the destination. best practice is . /src/

- VOLUME directory to hold persistent data

- WORKDIR sets directory for run commands, instead of cd <b>directory_name</b>

- ENV can be injected on docker run with -e

- USER default is root user

#### 2.  Build Image
<pre>
docker build -t <b>name_of_image:tag_name</b> + <b>Dockerfile_location</b>
</pre>
Flags:
<pre>
-q with build = limits output

--no-cache = build from scratch
</pre>

## Share Image

<pre>
docker push <b>username/image_name:tag_name</b>
</pre>

## Delete Image

<pre>
docker rmi <b>container_id</b> || <b>container_name</b>
</pre>


## Connect to Container

-In order to connect to a container you must publish the exposed ports

- You can do this with the -P flag during container creation then run docker ps to find the port mapping

- Or you can manually map the port to connect with it using -p <b>exposed_port</b>:<b>container_port</b>

- bridge network (same computer) is the default

- --net none is null network driver

- overlay network (multiple computers)

- can use --net to connect new container to existing network 

- can use same container_name if different networks using --net-alias

## View Networks
<pre>
docker network ls
</pre>
##  Mount Volume

<pre>docker run -v <b>host_location:container_location</b></pre>

## Connect Container to Container

### A. Default Network 

- docker-compose finds all child Dockerfiles to run multiple containers in default bridge network

docker-compose.yaml:

<pre>
<b>service_name</b>: 
  build: <b>src_directory</b>
  image:
  ports:
  networks:
  volumes:
  environment:
  command:

-build can be written as
      build:
        context: ../../jenkins/docker
</pre>

### B. Define Your Own

<pre>
version: "3"
services:
  postgres:
    container_name: class_postgres_4
    image: postgres:11.1
    restart: unless-stopped
    networks:
      - my-net
    ports:
      - "15432:5432"
    environment:
      POSTGRES_USER: "postgres"
      POSTGRES_PASSWORD: "${MY_PG_PASS}"
      POSTGRES_DB: "gogs"
    healthcheck:
      test: ["CMD", "pg_isready", "-U", "postgres", "-d", "gogs"]
      interval: 10s
      timeout: 5s
      retries: 5

Defining the network via docker-compose:

networks:
  my-net:
    driver: bridge

</pre>

### C. CLI Option

<pre>
docker network ls
</pre>
<pre>
docker network create <b>network_name</b> 
</pre>
<pre>
docker run --net <b>network_name</b> 
</pre>

## Docker Compose CLI

<pre>docker-compose ps</pre>
<pre>docker-compose up -d</pre>
<pre>docker-compose down</pre>
<pre>docker-compose kill</pre>
<pre>docker-compose rm</pre>
<pre>docker-compose build</pre>


## View Volumes

<pre>docker volume ls</pre>

## Orchestration Tools

New Relic’s Centurion
Spotify’s Helios
Ansible’s Docker

## Distributed Schedulers

Google’s Kubernetes
Apache Mesos

## Cloud Providers

Amazon EC2 Container Service
Google Container Engine
Red Hat OpenShift 3
Joyent Triton
Microsoft Azure
