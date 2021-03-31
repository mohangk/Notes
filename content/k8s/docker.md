---
draft: true
title: Docker
categories:
  - K8s
---
### docker basics

registry - stores all images
repo - for a particular image, store different versions

#### commands

##### ps

ps , list running docker containers
ps -a , list all images, including those that have been stopped

##### run

-i -t , interactive
-d , detached mode

run --rm , delete the container once done
run --name blah_blah, name the running container

docker run -d -p 8081:8080 mohangk/node-test:1.0.0, start a container in the background and forward port from it

##### build

docker build -t mohangk/node-test:1.0.0 .

--no-cache (to invalidate any cache)

##### commit

docker commit container-id repository_name:tag

##### tag

docker tag <container_id> repository:tag

##### exec

docker exec -it <container_id> <command>
docker exec -it 67bc16cdb672 sh

#### networking

Host network "ip addr show docker0"
On the container: "ip route show"

To setup such that 127.0.0.1 points to host
docker run -it --net="host" alpine:latest
https://stackoverflow.com/questions/31324981/how-to-access-host-port-from-docker-container

#### Dockerfile

FROM - what container to  user

RUN - instruction to run in the container, each run will be done on a separate writable layer and committed individually. Each RUN creates a new image layer

CMD - what to run when the container boots up, doesn't run when building the image

COPY  - copies files from build context to container

ADD - superset of copy, can get file from internet and can unpack file

USER - set the user that the process will run as 

WORKDIR - sets the working directory for any COPY, RUN, ENTRYPOINT cmd that follows it in the Dockerfile



### docker-compose

#### commands

1. up
2. ps
3. logs -f <container-name>
4. stop 
5. rm
6. build (rebuilds the images associate with this docker-compose file)



#### docker-compose.yml

version: '2'

services:

​    <service-name>:

​        build: .

​        ports

​     <service-name>:

​        image: <image-name>



