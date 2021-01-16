# Docker

Based on *Learn Docker In a Month of Lunches* Book

## TERMINOLOGY

* image 
  * is a read-only template that contains a set of instructions for creating a container that can run on the docker platform
  * contains all the content for an application along with instructions telling Docker how to start the app
  * images are like git repositories; they can be committed with changes and have multiple versions

* containers
  * lighweight, isolated environment where an application is packaged and executed
  * includes everything needed to run an application (run time, system tools, libraries, configurations, etc)
  * Each container has its own virtual environment and resources managed by docker 
    * Hostname, IP address, Disk, etc are all virual resources created by docker
  * Containers are running only while app is running; when app process ends, container goes into exited state
    * exited containers don't use any CPU time or RAM, but they still take up space/ memory
  * Unlike VMS, containers share the same resources with the host OS

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

* images & dockerfile
  * *base images* - images that have no parent image
    * includes OS and runtime libraries (ex: ubuntu or node)
  * *child images* - images that build on base images and add additional functionality
  * in dockerfiles, base images are used to create child images
  * images are stored as lots of small files (layers)
    * `docker image build` - assembles these files to container 
    * 1 dockerfile instruction = 1 image layer
  * Layers are saved to docker engine cache
   * build process is optimized by pulling shared layers from cache 
   * dockerfile instructions that are unlikely to change should be placed above the instructions that may vary
   * *search dockerfile optimization to learn more...*

* detached - where your terminal is not attached to the running container (done with the flag `-d`)
  * this means you can close your terminal after running the container, and keep the container running

* registry 
  * Server that stores images centrally (like github)

## Commands

**Maintenance commands**

```bash
# see list of all images on system
docker images

# see all containers that are currently running
docker ps
docker container -ls # alternative command

# see all containers including stopped containers
docker ps -a

# remove all stopped containers
## options = --filter -force, -f
docker container prune [options] 

# see the processes running IN a container
docker container top <container id>

# see all details of a container
docker container inspect <container id>

# See docker engine info
docker info
```

**Image commands**
```bash
# Pull container image
docker image pull <user/image-name>

# list all images
docker image ls

# Build image from DockerFile
docker image build --tag <image-name> <path to dockerfile & related files>
```

**Container Commands**
```bash

# Create Docker network for containers to communicate with each other on
docker network create <NETWORK_NAME>

# run app (image) in a container
## NOTE: will attempt to download image if not available on machine
docker container run <image-name>

## Run container with environment variables
docker container run --env <VARIABLE=VALUE> <image-name>

# Connect to a container like a remote computer
## interactive - tells Docker to set up connection to container
## tty - connect to terminal session inside the container
docker container run --interactive --tty <image name>

# Have container explicitly connect to a network with the `--network` flag
docker container run --name <container-name> -d -p <portnumber> --network <network name> <image-name>
```

## Topics

**Basic Docker workflow**
* BUILD - package your application to run in a container
* SHARE - publish your container to docker hub or else where so its available to other users
* RUN - pull the image from docker hub (or elsewhere) and run the app in a container


**Basic Docker architecture**
* Docker engine (daemon?) communicates with OS and manages all the resources, images, containers, etc
* Docker API is REST API used by client (ex: docker CLI) to interact with docker engine
* Docker CLI is one way users can use the docker API to interact with docker engine
* Similiar to client / server architectures (client/ server = docker CLI/ docker dameon)

**DockerFile**
* `docker image build` command builds image from a `DockerFile` and a *context*
  * `context` is the set of files at a specified location (`PATH` (ex: local directory) or `URL` (ex: git repo))
  * `.dockerignore` in the `context` directory can exclude files  
  * ex: `docker image build .`
    * the last arg is the PATH or URL of the context (the `.` is the current directory)
* Each instruction (ex: `FROM`, `COPY`, etc) in `dockerfile` 
  * `FROM` instruction is the starting point
  * runs independently
  * runs in order
  * outputs a new image
* Docker will re-use images from cache to speed things up
  * reused images will have "using cache" msg
* DockerFile format:
  ```
  # Comment
  INSTRUCTION arguments
  ```
* Simple DockerFile Example:
  ```bash
  FROM diamol/node
  CMD ["node", "/web-ping/app.js"]
  ENV TARGET="blog.sixeyed.com" \
        METHOD="HEAD" \
        INTERVAL="3000"
  WORKDIR /web-ping
  COPY app.js .
  ```
* Some dockerfile instructions (commands):

  `FROM`
  * starting point of an image
  * must be the first instruction (after any comments)
  * Syntax: `FROM repo/image-base-name`

  `ENV`
    * Sets values for environment variables. 
    * Syntax: `ENV KEYNAME="VALUE"`

  `WORKDIR`
    * Creates a directory in the container image filesystem
      * Sets that to be the current working directory of the image
    * Working directory is where instr. like `RUN. CMD, etc` will be executed in 
    * Syntax: `WORKDIR /directory-name`
  
  `COPY`
    * Copies files or directories from the local filesystem into the container image. 
    * Syntax: `COPY source path target path` 

  `CMD` 
    * Specifies the command to run when Docker starts a container from the image. 
  
  `ENTRYPOINT`
    * is an alternative to `CMD`
    * defines what commands get executed when running a contianer (ex: java main -> command to run java app command)
    * should be used when using the container as an executable
    * CMD should be used as a way of defining default args for a ENTRYPOINT command
    * *Check docs for better information on CMD & ENTRYPOINT*

**Multi-stage DockerFile**
* Pattern for building apps with single (multi-stage) dockerfile
  * Stage 1: Build Stage
    * Use the base image that your has your app's build tools installed (ex: Maven)
    * Copy the source code from host machine (?), and run `build` command
  * Stage 2: Test Stage
    * Use base image with test framework installed, copy the compiled binaries from build stage, and run tests
  * Stage 3: Final stage
    * Use base image with just the app runtime installed, copy the binaries from the build stage that have passed the tests in the test stage

Example:
```
FROM diamol/base AS build-stage
 RUN echo 'Building...' > /build.txt
 FROM diamol/base AS test-stage
 
 COPY --from=build-stage /build.txt /build.txt
 RUN echo 'Testing...' >> /build.txt
 
 FROM diamol/base
 COPY --from=test-stage /build.txt /build.txt
 CMD cat /build.txt
```

Some notes on the example:
* Multi-stage dockerfiles consist of multiple stages, each starting with `FROM`
  * the example above has 3 stages: `build-stage`, `test-stage`, and unamed stage
* Multi-stage dockerfile will output a single docker image with the contents of the final stage
* Each stage runs independently
  * Files and directories can be copied from previous stages (in the ex: `COPY --from=build-stage`)
* `RUN` command executes a command inside a container during build and saves any output from the command in the image layer
  * The command you want to run must exist in the Docker Image used in the `FROM` instruction


**Multi-stage DockerFile Examples**

--------------------------------------------------------------------------------

Java DockerFile Example:
```
FROM diamol/maven AS builder
 
 WORKDIR /usr/src/iotd
 COPY pom.xml .
 RUN mvn -B dependency:go-offline
 
 COPY . .
 RUN mvn package
 
 # app
 FROM diamol/openjdk
 
 WORKDIR /app
 COPY --from=builder /usr/src/iotd/target/iotd-service-0.1.0.jar .
 
 EXPOSE 80
 ENTRYPOINT ["java", "-jar", "/app/iotd-service-0.1.0.jar"]
```

Some notes on this example:
* `WORKDIR` sets the working directory for any instructions that follow it in the DockerFile (ex: `RUN, CMD, etc` will be executed in the `WORKDIR`)
  * Multiple `WORKDIR` instructions can be used to set new ones; they are relative to the previous `WORKDIR` instruction
  * If `WORKDIR` not exist, then it will be created by default
* `COPY . .` - "copy all files & directories" from the location where Docker build is running, into the working directory in the image"
* the last step in the `Build` stage is `RUN mvn package` which will output a JAR file (runnable Java zip file, the output of java building)
* `ENTRYPOINT` is an alternative to `CMD`; it tells Docker what to do when a container is started from the image (in this case, run Java app)
* the final application image should have no build tools
  *  `FROM diamol/openjdk` does not include the maven build tool. so you can't use maven when running this image in a container
  * anything from the stages prior to the final stage, must be copied to be included in the final image

The main takeaway: similar applications (that need to be compiled) follow this general pattern in DockerFIle:
* Build stage
  * FROM image that consists of language compiler and build tools
  * Runs the compilation/ build tools and produce files for execution
* Test stage: where tests are run on the source code
* Final stage
  * Binaries/ compiled files are copied from the prior stages
  *  FROM image that includes only what is necessary to execute the app (ex: openjdk, not maven, JUnit, etc)

---

Node.js DockerFile Example

```
# Build stage
FROM diamol/node AS builder
 
 WORKDIR /src
 COPY src/package.json .
 
 RUN npm install
 
# Final stage
FROM diamol/node
 
 EXPOSE 80
 CMD ["node", "server.js"]
 
 WORKDIR /app
 COPY --from=builder /src/node_modules/ /app/node_modules/
 COPY src/ .
 ```

the main takeaway:
  * the build stage in this example bases the image on the language runtime (in this ex: node) downloads the dependencies (in this ex, from `package.json`)
  * the final stage includes the node runtime and exposes the ports and sets up the execution of the app
  * this can pattern can apply to python and ruby apps as well

---
**Go DockerFile Example**

```
FROM diamol/golang AS builder
 COPY main.go .
 
 RUN go build -o /server
 
 # app
 FROM diamol/base
 
 ENV IMAGE_API_URL="http://iotd/image" \
       ACCESS_API_URL="http://accesslog/access-log"
 CMD ["/web/server"]
 
 WORKDIR web
 COPY index.html .
 COPY --from=builder /server .
 RUN chmod +x server
 ```

 * the main takeway
   * languages like Go, Rust, etc compile to native binaries and don't require the language runtime to execute apps
   * the build stage here includes the go tools and runtime
   * the final stage includes only layer of OS tools to execute the native binary
---

**image reference**
* Domain - the URL domain
* Account - Account name of the image owner
* Image repo name: application name
* Image Tag: version of the application (latest is the default if no tag is provided)
  * Tags are used to identify different versions of the same application
  * One tag naming scheme: `[major].[minor].[patch]`
* Syntax: `docker.io/account-name/app-name:tag-version`

**Pushing images to Docker hub**
* Need to login to the registry with the docker CLI
  * Store docker ID (username) in a variable (ex: `export dockerId=<username>`)
  * Login to docker hub: `docker login --username $dockerId`
* Need to give image a reference that includes the name of an account where you have permission to push
  * Ex: `docker image tag <image-name> $dockerId/<image-name>:<tag>`
  * images can have multiple references (or be associated with multiple repos)
* Push image to the registry
  * Ex: `docker image push $dockerId/<image-name>:<tag>`
  * Same with building images, Docker pushes/uploads image by layers
    * layers are only uploaded if there isn't one with the same hash in the registry
    * optimizing images will lead to having to push only couple of layers
  
**Docker Configuration**
* Docker engine has a JSON config file called `daemon.json` for settings
* Found in `C:\ProgramData\docker\config` on windows and `/etc/docker` on linux
* Any changes to `daemon.json` requires Docker engine to be restarted for the changes to take effect