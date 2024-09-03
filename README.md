# Docker Workflow

This repository is a comprehensive documentation on how to effectively use Docker with Git and Node.js it covers
* **Setting up Docker**: Installing and configuring Docker, and  and node projects.
* **Git projects**: Building and running git projects using Docker containers.
* **Node projects**: Building and running node projects using Docker containers.

Feel free to _modify_ this description, on any section you want to highlight. 

## Install Docker

Visit [Docker.com](https://www.docker.com/products/docker-desktop/) Download and install docker for your operating system.

### Docker with Git

After you have Docker installed, you can use it to build images that when ran will turn into containers... ***Before you continue*** make sure docker is running its engine. 

create a file called `Dockerfile` to create your git image file that you will be using across your projects as if git was installed in your operating system.

```
    FROM ubuntu:latest
    RUN apt-get update && apt-get install -y git
```

This will create an ubuntu image with git installed.. next you build the image with the following command

```
    docker build -t git-image . 
```
