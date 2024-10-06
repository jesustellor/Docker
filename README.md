# Docker Workflow
***I am using VSCODE for this project.***
This repository is a comprehensive documentation on how to effectively use Docker with Git and Node.js it covers
* **Setting up Docker**: Installing and configuring Docker, and  and node projects.
* **Git projects**: Building and running git projects using Docker containers.
* **Node projects**: Building and running node projects using Docker containers.

Feel free to _modify_ this description, on any section you want to highlight. 

## Install Docker..

Visit [Docker.com](https://www.docker.com/products/docker-desktop/) Download and install docker for your operating system.

### Docker with Git

After you have Docker installed, you can use it to build images that when ran will turn into containers... ***Before you continue*** make sure docker is running its engine. 

create a file called `Dockerfile` to create your git image file that you will be using across your projects. This file does not have an extension. **NOTE** You are creating a text file here and not running commands in the terminal.

```
    FROM ubuntu:latest
    RUN apt-get update && apt-get install -y git
```

This file will be used to create an ubuntu image with git installed.. next you build the image with the following command. **NOTE** you will run this command in the terminal in the same directory as your `Dockerfile` file.

```
    docker build -t git-image . 
```

the period `.` is used to specify the current directory. it is there to specify the location of the `Dockerfile` file on the current directory.
This will create an image named `git-image` that contains the image with git installed. Feel free to rename it whatever you want.

to run the image you can use the following command in the directory you want to use git in. im not sure if there is a shortcut for this yet, but this is how i run the image in my terminal

```
docker run -it --rm -v "$(pwd):/workspace" -w /workspace git-image
```

this will run docker in interactive mode, remove the container when finished, mount the current directory into the container, and set the working directory to the current directory created in the container..

## Docker with Node

To run a Node.js project with Docker, you will need to install Node.js. once you are in the image above yuo can use the following command to install Node.js.

```
sudo apt-get install nodejs npm
```

You started of with an Ubuntu image and Git and now you have Nodejs as well.
 ***NOTE:***
***Docker is not a Node.js package manager.***
***Please ensure it is already installed on your system.***

### VSCODE EXTENSION

Download the [VSCODE extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) ***Dev Containers**
press ctrl+shift+p and search for "Dev Containers" Attach to running container and pick the container you have running from the command above.
It should be the only option unless you have created containers before.

VSCODE will open with git enabled, but you will see an ubuntu folder, in the above command we created /workspace inside the container so all you have to do is cd `/workspace` the `/` is important because it is at the root of the container.. 
if you `dir` here or `ls` you will see the files in your folder, `code filename.js` will open the file in vscode, or `code folderNmae` and use git like you normally would.. git add, git commit, git push.. or with VSCODE git Add ons

***NOTE*** dont forget you have to add 
```
git config --global user.name username
```
```
git config --global user.email email.@email.com
```

changing the username and email to match your git account..

### Node projects with Docker

to get this to work, if you have been following along should have 2 vscode windows open, one that you used the terminal on and Dev Devices, and one that has workspace at the top of vscode.. 

```
docker run -it --rm -v "$(pwd):/workspace" -w /workspace node:20-alpine
```
on the vs code running git press `ctrl + shift + p` and search for "Dev Devices" and again link to the running container running node, it should open another vscode window will open with node running... 

## Docker Image from Container

***Configuration files for next time***: One thing i found myself doing was typing out long hand commands, or going back to edit the bashrc file to create alias for git... to persist this across sessions. make your change. bashrc file is found in cd `/home/ubuntu` you can only use `code .bashrc` to edit it if you use vscode Dev Containers.

```
docker commit container_id new_image_name
```

You can now use `docker run -it --rm -v "$(pwd):/workspace" -w /workspace new_image_name`

Happy Coding..


# UPDATE 9/27/2024

I have mainstreamed my environment to Using Docker Only, after downloading the OpenVS Code for the Web Extension from docker, *** 18k downloads not 8k.*** i saw that in order to use the extension with added packages you had to install them using commands found here [Here](https://github.com/marcelo-ochoa/coder-docker-extension/blob/main/README.md). add them only after completing this next step or you will have to do it all again.

I then modify the config file located somewhere similar to this.. `C:\Users\jesus\AppData\Roaming\Docker\extensions\mochoa_coder-docker-extension\vm`

The docker compose file should be updated to include the following port updates for your projects, I only added the 3000 one.. target is the port created in the container in my case npm start, creating react app in localhost:3000 and the published is your localhost port so you can pick any port you want i just keep it consistent, I also just mount Documents.. thats only if you want to make it easy to transfer files between the editor and your computer.

```
name: mochoa_coder-docker-extension-desktop-extension
services:
  coder:
    command:
    - --auth
    - none
    - --verbose
    - --app-name
    - VS
    - Code
    - Web
    - Docker
    - Desktop
    - Extension
    container_name: coder_embedded_dd_vm
    deploy:
      restart_policy:
        condition: always
    environment:
      TZ: America/Argentina/Buenos_Aires
    image: codercom/code-server:4.90.2
    labels:
      com.docker.desktop.extension: "true"
      com.docker.desktop.extension.name: VS Code for the Web
    networks:
      default: null
    ports:
    - mode: ingress
      target: 8080
      published: "57080"
      protocol: tcp
    - mode: ingress
      target: 3000
      published: "3000"
      protocol: tcp
    restart: unless-stopped
    volumes:
    - type: volume
      source: coder_data
      target: /home/coder
      volume: {}
    - type: bind
      source: /var/run/docker.sock.raw
      target: /var/run/docker.sock
      bind:
        create_host_path: true
    - type: bind
      source: C:/Users/Jesus/Documents
      target: /home/coder/Documents
      bind:
        create_host_path: true
    - type: bind
      source: /run/guest-services/mochoa_coder-docker-extension
      target: /run/guest-services
      bind:
        create_host_path: true
  coder-docker-extension:
    container_name: mochoa_coder-docker-extension-desktop-extension-service
    deploy:
      restart_policy:
        condition: always
    image: mochoa/coder-docker-extension:4.90.2
    labels:
      com.docker.desktop.extension: "true"
      com.docker.desktop.extension.name: VS Code for the Web
    networks:
      default: null
    volumes:
    - type: bind
      source: /run/guest-services/mochoa_coder-docker-extension
      target: /run/guest-services
      bind:
        create_host_path: true
networks:
  default:
    name: mochoa_coder-docker-extension-desktop-extension_default
volumes:
  coder_data:
    name: mochoa_coder-docker-extension-desktop-extension_coder_data
```

Once you have restarted Docker and then your Computer, because after a Docker restart it usually doesn't do it.. you will see your folder and inside Docker Desktop, under the containers, you will you that the new port has been added as well.

## Addind extra packages
If want to run/debug NodeJS code a node command must be installed prior you checkout for project. To simplify that an script is provided as post installation step, here an example of using them:  You will run these commands on your local computer as you only have docker installed.


```
docker exec -ti --user root coder_embedded_dd_vm /bin/sh -c "curl -s https://raw.githubusercontent.com/marcelo-ochoa/coder-docker-extension/main/addNodeJS.sh | bash"

docker exec -ti --user root coder_embedded_dd_vm /bin/sh -c "curl -s https://raw.githubusercontent.com/marcelo-ochoa/coder-docker-extension/main/addDocker.sh | bash"

docker exec -ti --user root coder_embedded_dd_vm /bin/sh -c "curl -s https://raw.githubusercontent.com/marcelo-ochoa/coder-docker-extension/main/addJava.sh | bash"

docker exec -ti --user root coder_embedded_dd_vm /bin/sh -c "curl -s https://raw.githubusercontent.com/marcelo-ochoa/coder-docker-extension/main/addPython.sh | bash"

```

## Adding Oh My ZSH

simply run this command in the terminal inside vsocde for the webs terminal `sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"`

I thought at first that this was being installed on my WSL, but it is in fact being installed in the container / extension for vscode.. 

Happy Coding... 