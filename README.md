# Docker Workflow
***I am using VSCODE for this project.***
This repository is a comprehensive documentation on how to effectively use Docker with Git and Node.js it covers
* **Setting up Docker**: Installing and configuring Docker, and  and node projects.
* **Git projects**: Building and running git projects using Docker containers.
* **Node projects**: Building and running node projects using Docker containers.

Feel free to _modify_ this description, on any section you want to highlight. 

## Install Docker

Visit [Docker.com](https://www.docker.com/products/docker-desktop/) Download and install docker for your operating system.

### Docker with Git

After you have Docker installed, you can use it to build images that when ran will turn into containers... ***Before you continue*** make sure docker is running its engine. 

create a file called `Dockerfile` to create your git image file that you will be using across your projects as if git was installed in your operating system. <span style="color:red;">**NOTE**</span> You are creating a text file here and not running commands in the terminal.

```
    FROM ubuntu:latest
    RUN apt-get update && apt-get install -y git
```

This file will be used to create an ubuntu image with git installed.. next you build the image with the following command <span style="color:red;">**NOTE**</span> you will run this command in the terminal in the same directory as your `Dockerfile` file.

```
    docker build -t git-image . 
```

This will create an image named `git-image` that contains the image with git installed.

to run the image you can use the following command in the directory you want to use git in. im not sure if there is a shortcut for this yet, but this is how i run the image in my terminal

```
docker run -it --rm -v "$(pwd):/workspace" -w /workspace git-image
```

this will run docker interactive, remove the container when finished, mount the current directory into the container, and set the working directory to the current directory created in the container..

