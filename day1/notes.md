# Day 1 Docker :
## Why use Docker?
We use docker to assure the consistency across multiple development environments.<br>

Docker allows us to package an application with all of its dependencies into a standardized unit for software development.<br>

Exemple : 
 You create an application that works on your machine (dev environment) but when you try to run it on another machine (production environment) it doesn't work because of different versions of libraries, different configurations, etc.<br>

## How it works ?
Docker will create an image of your application that includes everything it needs to run (code, runtime, libraries, environment variables, configuration files, etc.).<br>
The image will be used in a container that assure :
- Isolation : Each container runs in its own isolated environment, so you don't have to worry about conflicts with other applications or services running on the same machine.
- Portability : Docker containers can run on any machine that has Docker installed, regardless of the underlying operating system or hardware.
- Lightweight : Docker containers are lightweight and efficient, so they can be started and stopped quickly, making them ideal for development, testing, and deployment.

## Difference between Docker and Virtual Machine ?
- VM : A full operating system embadded with its own kernel, which makes it heavy and resource-intensive. A VM is isolated from the host operating system.

- Docker : Shares the host operating system's kernel , which makes it lightweight and efficient. But every container has its own dependencies and libraries.

## Docker Flow :
1. You create a Dockerfile that contains instructions on how to build your application image.
2. You build the image using the Dockerfile with the command `docker build -t your_image_name .` The image will contain everything your application needs to run.
3. You push the image to a Docker registry (like Docker Hub) so that it can be accessed from other machines using the command `docker push your_image_name`. Generally, every entreprise has its own private registry.
4. Pull the image from the registry on the target machine using the command `docker pull your_image_name`.
5. Run the image in a container using the command `docker run -d -p host_port:container_port your_image_name`. The `-d` flag runs the container in detached mode (in the background), and the `-p` flag maps a port on the host machine to a port in the container.

## Docker architecture :

Docker implements a client-server architecture, the above image illustrates the architecture of Docker.<br>
<center><img src="docker.png" alt="Docker Architecture" width="900" height="300"/></center><br>

### Docker Daemon:
    The Docker daemon (`dockerd`) is the server-side of Docker. The Daemon receives the commands sent from the **Docker Client** and manages various Docker objects like : services, images , containers, networks, and volumes. <br> The Docker Daemon can also communicate with others daemons to manage Docker services.<br>
    
### Docker Client:
    The Docker Client (`docker`) is the command-line interface (CLI) that allows users to interact with the Docker Daemon. When you run a Docker command in your terminal, the Docker Client sends the command to the Docker Daemon, which then executes it. The Docker Client can communicate with more than one Daemon.

### Docker registries:
    Docker Registry is the place where the created docker images are stored.
    By default, the Docker Hub registry is the public registry where anyone can host their docker images. But enterprises generally use private registries to store their images securely (Nexus registry for example).