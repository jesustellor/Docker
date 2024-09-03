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

create a file called `Dockerfile` to create your git image file that you will be using across your projects as if git was installed in your operating system. **NOTE** You are creating a text file here and not running commands in the terminal.

```
    FROM ubuntu:latest
    RUN apt-get update && apt-get install -y git
```

This file will be used to create an ubuntu image with git installed.. next you build the image with the following command. **NOTE** you will run this command in the terminal in the same directory as your `Dockerfile` file.

```
    docker build -t git-image . 
```

the period `.` is used to specify the current directory. it is there to specify the location of the `Dockerfile` file on the current directory.
This will create an image named `git-image` that contains the image with git installed.

to run the image you can use the following command in the directory you want to use git in. im not sure if there is a shortcut for this yet, but this is how i run the image in my terminal

```
docker run -it --rm -v "$(pwd):/workspace" -w /workspace git-image
```

this will run docker in interactive mode, remove the container when finished, mount the current directory into the container, and set the working directory to the current directory created in the container..

### VSCODE EXTENSION

Download the [VSCODE extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) ***Dev Containers**
press ctrl+shift+p and search for "Dev Containers" Attach to running container and pick the container you have running from the command above.
It should be the only option unless you have created containers before.

VSCODE will open with git enabled, but you will see an ubuntu folder, in the above command we created /workspace inside the container so all you have to do is cd `/workspace` the `/` is important because it is at the root of the container.. 
if you `dir` here or `ls` you will see the files in your folder, `code filename.js` will open the file in vscode, git add, git commit, git push.. 
***NOTE*** dont forget you have to add 
git config --global user.name username
git config --global user.email email.@email.com