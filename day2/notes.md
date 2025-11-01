# Day 2 Dockerize an Application :
Today, I will contenerize a simple application using Docker. <br>
The application is a simple TODO LIST application provided by docker.
## Steps to Dockerize the Application :
1. Clone the application repository from GitHub using the command :
```git clone -b build-image-from-scratch https://github.com/docker/getting-started-app.git```
2. Navigate to the project directory using the command :
```cd getting-started-app```

3. Create a Dockerfile in the root directory of the projeect.

4. After writing the instructions in the Dockerfile, build the Docker image using the command :
```docker build -t node-app:0 .```
5. Run the Docker container using the command :
```docker run -n day2-node-app -p 3000:3000 node-app:0```<br> We Expose port 3000 of the container to port 3000 of the host machine.

## Some Docker commands:
### See all containers : 
- Use : ```docker ps ``` to see all running containers <br>
- If you want to see all the container : ```docker ps -a```

### Build Docker Image:
- Format : ```docker build -t <image-name>:<version> .```<br>
- Exemple : ```docker build -t node-app:0 .```

### Run Docker container
```docker run --name day2-node -p 3000:3000 node:0```
### Push docker image to Hub : 
1. Create a repository in your Docker hub account.
2. ```docker <your-username>/node-app:0 ```.

### Pull Docker image from Hub : 
```docker pull <your-username>/node-app:0 ```