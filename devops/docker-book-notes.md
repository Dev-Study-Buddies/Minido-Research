# Docker

* Based on *Learn Docker In a Month of Lunches* Book


**Commands**
```bash
# see list of all images on system
docker images

# see all containers that are currently running
docker ps
docker container -ls

# see all containers including stopped containers
docker ps -a

# remove all stopped containers
## options = --filter -force, -f
docker container prune [options] 

# see the processes running IN a container
docker container top <container id>

# see all details of a container
docker container inspect <container id>

# run app (image) in a container
## NOTE: will attempt to download image if not available on machine
docker container run <image-name>

# Connect to a container like a remote computer
## interactive - tells Docker to set up connection to container
## tty - connect to terminal session inside the container
docker container run --interactive --tty <image name>

# Pull container image
docker image pull <user/image-name>

# list all images
docker image ls

# Build image from DockerFile
docker image build --tag <image-name> <path to dockerfile & related files>

```
**TERMINOLOGY**

* image 
  * is a read-only template that contains a set of instructions for creating a container that can run on the docker platform
  * contains all the content for an application along with instructions telling Docker how to start the app
  * In-depth details:
    * images are like git repositories; they can be committed with changes and have multiple versions
    * *base images* are images that have no parent image; these are OS images like ubuntu or runtime images like node
    * *child images* are iamges that build on base images and add additional functionality
    * images are stored as lots of small files (image layers); docker assembles them to create container
      * 1 dockerfile instruction = 1 image layer
      * Docker engine saves the layers (sub-image files) into the docker engine cache
      * The benefit of this ^ is that image layers can be shared between different images and containers
      * so, the layers that contains node.js runtime can be shared among other containers that run nodejs apps
      * sharing layers leads to images taking up less space and being *optimized* 
      * to optimize dockerfiles, place instructions that are unlikely to change above the instructions (layers) that will change alot...so that these unchanging layers can be shared among containers

* containers
  * lighweight, isolated environment where an application is packaged and executed
  * includes everything needed to run an application (run time, system tools, libraries, configurations, etc)

* daemon
  * background service running on the host that manages building, running, and distributing Docker containers
  * this is the process that runs in the operating system which clients talk to (in a client-server application, this would be the "server")

* client/ CLI
  * the tool that allows the the user to interact with the daemon

* docker hub
  * online registry of docker images..like github for docker images
  * registry - server hosting docker images

* dockerfile
  * file that contains list of commands that client calls while creating an image
  * automates the image creation process (the commands are similiar to the linux commands you would use to create the image)


* detached - where your terminal is not attached to the running container (done with the flag `-d`)
  * this means you can close your terminal after running the container, and keep the container running

## CH 2 - Understanding Docker and running Hello World

* Docker workflow: 
  * BUILD - package your application to run in a container
  * SHARE - publish your container to docker hub or else where so its available to other users
  * RUN - pull the image from docker hub (or elsewhere) and run the app in a container

* Container
  * Each container has its own virtual environment and resources managed by docker 
    * Hostname, IP address, Disk, etc are all virual resources created by docker
  * Containers are running only while app is running; when app process ends, container goes into exited state
    * exited containers don't use any CPU time or RAM, but they still take up space/ memory

* VM vs Container (simple explanation)
  * VM has its own operating system separate from the host OS; does not share any resources with the host OS
  * Containers are isolated/ separate from each other, but they share the same resources with the host OS
    * this makes them lightweight and allows you to run many more containers than you would be able to with VMs

* Docker architecture
  * Docker engine (daemon?) is what talks with the host OS and manages all the images and containers and other resources; its like the "server"
  * You can "interact" with the daemon through the Docker API, which is a standard HTTP-based REST API; access to this API (to any docker engine/ daemon API) can be configured
  * Docker CLI is the client of the daemon/ api (client); users use CLI or other client programs to interactct with the docker engine


## CH 3 - Building Your Own Docker Images
* you can run a container with environment variables
  * ex: `docker container run --env TARGET=google.com diamol/ch03-web-ping`
* docker file
  * Ex: 
```bash
FROM diamol/node
 CMD ["node", "/web-ping/app.js"]
 ENV TARGET="blog.sixeyed.com" \
       METHOD="HEAD" \
       INTERVAL="3000"
 WORKDIR /web-ping
 COPY app.js .
 ```

 ```
FROM 
 - Every image has to start from another image. 
 - In this case, the web-ping image will use the diamol/node image as its starting point. 
 - That image has Node.js installed, which is everything the web-ping application needs to run.

ENV 
 - Sets values for environment variables. 
 - The syntax is [key]="[value]"

WORKDIR 
 - Creates a directory in the container image filesystem, and sets that to be the current working directory. 

COPY 
 - Copies files or directories from the local filesystem into the container image. 
  - The syntax is [source path] [target path] 

CMD 
 - Specifies the command to run when Docker starts a container from the image. 
 - This runs Node.js, starting the application code in app.js.
```

## CH 4 - Packaging apps from source code into Docker images

