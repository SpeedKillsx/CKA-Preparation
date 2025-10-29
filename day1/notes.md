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
- VM : A full operating system embadded with its own kernel, which makes it heavy and resource-intensive.
- Docker : Shares the host operating system's kernel , which makes it lightweight and efficient. But every container has its own dependencies and libraries.