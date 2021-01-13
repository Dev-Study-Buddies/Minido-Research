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
docker container run <image-name>

# Connect to a container like a remote computer
## interactive - tells Docker to set up connection to container
## tty - connect to terminal session inside the container
docker container run --interactive --tty <image name>

```
**TERMINOLOGY**

* image 
  * is a read-only template that contains a set of instructions for creating a container that can run on the docker platform
  * contains all the content for an application along with instructions telling Docker how to start the app
  * images are like git repositories; they can be committed with changes and have multiple versions
  * *base images* are images that have no parent image; these are OS images like ubuntu or runtime images like node
  * *child images* are iamges that build on base images and add additional functionality
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

* VM vs Container (simple explanation)
  * VM has its own operating system separate from the host OS; does not share any resources with the host OS
  * Containers are isolated/ separate from each other, but they share the same resources with the host OS
    * this makes them lightweight and allows you to run many more containers than you would be able to with VMs


## CH 3 - Building Your Own Docker Images



## CH 4 - Packaging apps from source code into Docker images