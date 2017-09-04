---
layout: post
title: Simple DevOps
date: 2017-09-02 12:15:00 +0800
categories:
- Tech
tags:
- Docker
- Container

---

## The Demo

- GitHub:[devops](https://github.com/xianyuxmu/devops)
- Docker Hub: [devops2](https://hub.docker.com/r/xianyuxmu/devops2/)

### Containers

Build and run the app:

- Clone the demo project. 
	- Execute `git clone https://github.com/xianyuxmu/devops.git`, and then `cd ./devops`.
- Build A Docker Image.
	- Execute the build command `docker build -t devops .`, this creates a Docker image, wait for a while...
	- After successfully built, run `docker images` to check your built image(It’s in your machine’s local Docker image registry).
- Run the app.
	- `docker run -d -p 5000:3000 devops`, the command launchs the image as a container and maps your machine's port 5000 to the container's published port 3000 using `-p`.
- Your app is running now.
	- Using `docker ps` or `docker container ls` to see the containers which is currently running.
	- You can stop a container's process using `docker stop <CONTAINER ID>`
	- How to get into a docker container? Using `docker exec -it <CONTAINER ID> bash` then we can run commands inside the container.
	- Also, we could run some commands directly from outside using `docker exec -it <CONTAINER ID> whoami`.
	- Or similarly, we could start a shell in the container using: `docker exec -it <CONTAINER ID> sh`.


### Share your image

- Login in first `docker login`.
- Tag your image `docker tag image username/repository:tag`
- Publish your image `docker push username/repository:tag`


----

## Related

- [Configure automated builds on Docker Hub](https://docs.docker.com/docker-hub/builds/)
- [HOW TO USE YOUR OWN REGISTRY](https://blog.docker.com/2013/07/how-to-use-your-own-registry/)


