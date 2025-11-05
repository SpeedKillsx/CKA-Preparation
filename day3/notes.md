# Day 3 : Docker Multistage Builds

## What is a Multistage Build?
A multistage build is a Docker feature that allows to use multiple `FROM` statements in a single Dockerfile. This will optimize the final image size by allowing you to copy only the necessary artifacts from intermediate stages. <br>

The Dockerfile became more readable, simple and faster to build, as the layers do not change a lot during the build process.

## Day's Steps : 
1. Use the same project in the `Day 2`.
2. Build during 2 stages (using Node for stage 1 and NGINX for stage 2)