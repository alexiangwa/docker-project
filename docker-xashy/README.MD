- Docker: is a containerisation software that is used to package our application with its dependencies.
- docker creates an image after the containerisation process . this image containe=s the artifact and it's dependencies
- every container is isolated from each other
- the container uses a linux kernel to give cpu and memory
nocejs + amazon linux = container 1
mysql db + ubuntu = container 2
war + tomcat + centos  = container3 all deployed to docker server 

ct1 + cont2 + cont 3 (isolated from each other)
    docker
   os kernel

   containers :
- very light weight (vm are heavy)
- can run on any server that has the docker host and os kernel (in vms app might run on one server and might not run on another server)
- do not use alot  of process capacity ( needs alot of process capacity)
- can boot very fast (start) (vms take long time to boot)
- run if the processes are running as soon as the process dies the containers dies. (vm runs even if the process is stopped)

when you install docker it comes with:
- docker CLI (launch docker commands to build, run images and containers)
- docker API ( getting responses through to the docker daemon and providing response through the client )
- docker daemon ( it is the core of docker functionalilty, it is what performs the docker process)

two types of docker versions
commumity free (owned by redhat)
enterprise ( paid comes with suppory, security and priv repo)

        image                          container
---------------------------          |-------------------------------
- it's a tamplate that has artifact  | - it's an instantiation os an image 
runtime and os dependencies          |  its a running image
- from the image u can run a         | - live time is short once stopped it dies
container

NAMESPACE:
why are conntainers isolated from ech other
- when u install docker it would create a docker namespace
- Namespace: is an isolated area for the proper running of a container 

TYPES OF NAMESPACES
--------------------
1) PROCESS NAMESPACE: allocates processes to ur container

2) C-groups: allocates cpu and memory from the kernel and allocating it to container

3) Mount namespace: by default every data that a container stores while its runs is only in tact when the container is running. the mount  namespace would help us mount our docker containers to the storage of your VMs, you can create a folder on docker host and attach it to the container, even if  the container is terminated its data is stored in a folder

Network namespace: containers have their own network, eg bridge ntwk, host ntwk, and none ntwk and each container is allocated an IP address


- DOCKER COMMANDS
- docker pull   -use to pull image from where u specify if u don't specify it will pull from dockerhub.

- docker images --- shows you all your running images

-docker ps -- shows all running containers on your host

-docker ps -a --- shows all containers on your host and existed once

- docker run -d <imagename> -- creates a container from an image and runs the container ( we  can run the container in an attached mode and dettached mode)
-by default a container is ran in an attached mode : i.e when the container starts running it displaces processes on your shell.
- detached mode : runs the container in the background 
-if docker doesnot find the image on your host it coud download it from dockerhub

NB every container has an ID, status, and name

containers created from just am OS image is going to run and exit immediately . this is because docker is meant to run on OS
- why do we have OS : so that we can use them as baseimages in our dockerfile to create application images

- if i want to run a container frorm an os image i most specify the option SLEEP and the time in seconds 
(docker run -d centos sleep 100)
- os containers have no process in them so docker can not have them running so we specify the sleep which will be the command the start the container from an os image. 

docker stop <container-id>

-docker rm <container-id>  -to remove exited containers

if u want to remove a container you have to stop it before removing.

- docker rmi - use to remove docker images from your host (NB the image should not be in use by a container)

PORT MAPPING
=============
HELPS you to map a port on a vm to the port on the container.
eg if i want this to be expose on a docker host. i need to map a port on the host to that on the container.

- docker run -d -p 8000:9000 sonarqube

- docker run  -d -P nginx ( will give nginx a random port)

- docker exec command to see the content of the container
(docker exec -t containerID bash)

- i ---> lets u run the container in an interactive mode
- t ---> lets u run the container on an interactive shell or terminal.

cat /etc/os-release

- docker inspect <container-id>  -- used to inspect ur container eg network port
the default network is called bridge network  i.e container has a port that it is assigned to, it is uses to retrieve details indoemation about docker objects, images,network,


DOCKERFILE:
-----------
by default the docker file should be named dockerfile
- FROM -- specify the base image, it execute shell commands within the docker image

- RUN - it is use to execute shell commands within the docker image

- COPY - copies files and directories from the docker host into the docker image

- ADD instruction is uded to download something frorm internet into the docker image

- EXPOSE -- specifies the port which the image should be exposed on

- WORKDIR -specifies the working directory for the subsequent command in the docker file

- COMMAND AND ENTRYPOINT -specifies the command that would be use to start up the container that could be created from the image

CMD is a default instruction that would start up a container from the image
CMD can be over written (has parameters wh can be overwritten)

ENTRYPOINT contains the executables use to run the container without being overwritten

- when an image is develop from a dockerfile it is build on layers
- every step in a dockerfile  is a layer on its own

if a step on the dockerfile fails , the image building stops
when u strt building again, docker doesnot start from the first line, it would continue from the layer where it stopped.(docker caches layers)

- docker build -f dockerfilename --(locate the docker file and build the docker image)

- docker build -t tagname -f dockerfilename   -the image can be tag use the -t command 


docker build -t xashywebapp:v1 .

docker run -d --name myywebapp -P xashywebapp:v1

http://50.17.111.200:32768/xashy-webapp/ # TO ACCESS THE APP ON LINE note that a random port is attached to the container (port: 32768)

Docker volume
==============
Docker volume a volue on your host is mapped to a volume on your container..
if the container exit every data stored on your container could be available on your docker volume.
- Docker vol makes your stouage to be persistent. 

docker volume create myapp-volume  (# to create a volume on your host)

docker volume ls

when u install docker on the host it has sa default directory tat is created called /var/lib/docker (it containers every information about every docker object that docker is managing within your host system)

docker run -d -P --name myapp -v myapp-volume:/usr/local/tomcat/webapp
s xashywebapp:v1

(to attach a volume to a container and name it the container)


docker networking
==================
by default the network of a container is isolated from that of the host. 
- every container has its ip address from its network cidr
- networking in docker determines how containers would communicate with each other
- the default docker network type is called Bridge network

bridge network
--------------
- The bridge ntwk creates an internal network that allows containers to communicate with each other. (container can talk to each other in the same network using their hostname i.e container name)

docker network create mynetwork

to attach a network
====================
- docker run -d -P --name mywebapp -v data:/usr/local/tomcat/webapps --network mynetwork xashywebapp:v1

- docker logs <containerID> shows you logs that are generated
and you can trobleshoot from the logs

Host network
------------
it takes off the docker network and let your containers make use of the default network of thehost
- has more speed

 overlay network
-------------------
it allows communication between docker containers running on multiple docker host
- make containers in multiple nodes talk to each other

Tagging images and pushing it to dockerhub
-------------------------------------------
when tagging an image it should start with the user name of ur dockerhub/ repository name:version of image

docker build -t alexiangwa/xashy-webapp:v1 . (to tag an iamge)

 docker push alexiangwa/mscloud-webapp:v1



DOCKER COMPOSE
===============

 Docker helps in starting multiple containers at the same time and stop muliple containers at the same time

For you to laverage docker compose you need to have a docker compose file. 
 The docker compose file is wriiten in yaml.

 - components of a docker compose file
 -----------------------------------------
 - a ddocker compose file is name as docker-compose.yaml

 1) services: define the individual containers that make up the application, each service represent a seperate container. each service has its properties

 2) image: specifies the docker image to use, if the image is not on your host , docker compose would pull it from dockerhub or docker registry

 3) Network: specifies the network u want your servics to run on

 4) Volumes: u can have a volume section where u have all your volume and u specify everthing need for that volume.

 5) environment variables: the way some container images are build theyy might have a section for environment variables. that means if u have to run a conatiner from that image u might need to specify an environment variable

 6)Port: we use a port to expose your app to the outside world , by mapping a port on a container to that of the host.  

 7) dependencies: uses the depend-on: to ensure that one service starts before the other. 

 all thiese are not recommended . u can specify just the version, service images and portin the services without network and ports
