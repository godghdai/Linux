https://hub.docker.com

```
docker run postgres:9.6
docker ps
docker run postgres:10.10
```

```
docker pull redis
docker run redis

# docker run = start new container with a command
# the container in a detached mode and that is minus deep
docker run -d redis
docker run redis:4.0

# docker stop = stops the container
docker stop   containerID
docker start  containerID

# list running and stopped container
docker ps -a  
```

```
# bind port of your host to the container
# host:container
docker run -p6000:6379 -d redis 
docker run -p6001:6379 -d redis:4.0
```

```
docker logs containerID
docker run -p6000:6379 -d redis --name redis-older redis:4.0

# it = Interactive Terminal
docker exec -it  containerID|containerName  /bin/bash

docker run is to create a new container
docker start is to restart a stoppedd container
```



Docker in Software Development

```shell
docker pull mongo
docker pull mongo-express
docker images
docker network ls
docker network create mongo-network
```

```
docker run -d \
-p 27017:27017 \ 
-e MONGO_INITDB_ROOT_USERNAME=admin \
-e MONGO_INITDB_ROOT_PASSWORD=password \
--name mongodb \
--net mongo-network \
mongo 
```

```
docker run -d \
-p 8081:8081 \ 
-e ME_CONFIG_MONGODB_ADMINUSERNAME=admin \
-e ME_CONFIG_MONGODB_ADMINPASSWORD=password \
-e ME_CONFIG_MONGODB_SERVER=mongodb
--name mongodb-express \
--net mongo-network \
mongo-express
```

```
curl localhost:8081
docker logs containerID | tail
# stream logs
docker logs containerID -f 
```

```js
var MongoClient=require('mongodb').MongoClient;
MongoClient.connect('mongodb://admin:password@localhost:27017')
```

Docker Compose

```
vim mongo.yaml
```

```yaml
version: '3'
services:
  # my-app:
  # image: ${docker-registry}/my-app:1.0
  # ports:
  # - 3000:3000
  mongodb:
    image: mongo
    ports:
      - 27017:27017
    environment:
      - MONGO_INITDB_ROOT_USERNAME=admin
      - MONGO_INITDB_ROOT_PASSWORD=password
    volumes:
      - mongo-data:/data/db
  mongo-express:
    image: mongo-express
    ports:
      - 8080:8081
    environment:
      - ME_CONFIG_MONGODB_ADMINUSERNAME=admin
      - ME_CONFIG_MONGODB_ADMINPASSWORD=password
      - ME_CONFIG_MONGODB_SERVER=mongodb
volumes:
  mongo-data:
    driver: local
```

```
docker-compose -f mongo.yaml up
docker-compose -f mongo.yaml down
docker-compose -f mongo.yaml up -d
```

Dockerfile

```
# The name must be "Dockerfile"
vim Dockerfile
```

```
# start by basing it on another image
FROM node:13-alpine

# optionally define environment variables
ENV MONGO_DB_USERNAME=admin \
    MONGO_DB_PWD=password
    
# execute any linux command
# directory is created INSIDE of the container !
RUN mkdir -p /home/app

# execute on the HOST machine !
COPY . /home/app
CMD ["node","/home/app/server.js"]
```

```
docker build -t my-app:1.0 .
docker images
docker run my-app:1.0
docker ps -a | grep my-app
# deleted the container
docker rm containerID
# deleted the image
docker rmi imageID
docker ps 
docker logs containerID

# inside the container
# some containers to not have bash installed
# docker exec -it containerID /bin/bash
docker exec -it containerID /bin/sh
ls -al
env
```

Docker Volumes

```
# host directory:container directory
docker run -v /home/mount/data:/var/lib/mysql/data

# container directory
# /var/lib/docker/volumes/random-hash/_data:/var/lib/mysql/data
docker run -v /var/lib/mysql/data

docker run -v name:/var/lib/mysql/data

```

