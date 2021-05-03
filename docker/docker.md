# Docker

## What is docker?
Docker enables you to separate your applications from your infrastructure so you can deliver software quickly

Basic process:

1. Build the image
2. Ship it
3. Run it

## Installation
In order to run Docker , we need to [download it](https://www.docker.com/products/docker-desktop)

## Usage
In order to run it in our repo

1. Create a `Dockerfile` in the root of the PATH of your repo
2. Add the config file in that repo:

    ```docker
    FROM node:12.16.0-alpine AS production # Select which engine to use + tag

    COPY ./src /opt/app # Copy the App into the path that you specify

    WORKDIR /opt/app # Docker will move to that dir (to run the commands)

    # Required dependencies
    RUN yarn 

    CMD ["node", "index.js"] # This will get executed when we do: $ docker
    ```

3. Then, we can build our image: `$ docker build`
4. Time to run the image: `$ docker run` 
    We can pass some flags like:
    * The port `-p 3000:3000`(with the 3000:3000 we are matching our local port to the docker port)
    * The name `--name hello`
    * If we want to run it on the background we can detach it `-d`
5. Final: `$ docker run -p 8080:80 --name hello -d hello-world` 
    * The `hello-world` is the name of the image

## Useful commands

* `$ docker images` : to check which images I've had in my machine (The image ID is a SHA that will be the identifier of the image, compare to the image above, it's the same image that we built previously)
* `$ docker ps`: (docker processes) useful to check which  processes from docker are running. If we add the flag `-a` we'll see all the images (running or not) `$ docker ps -a`
* `$ docker start <image_name>`: Command to restart a process/image
* `$ docker stop <image_name>`: Command to stop an image
* `$ docker rm <image_name>`: To remove images