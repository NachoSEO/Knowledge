# Docker

## What is docker?
Docker enables you to separate your applications from your infrastructure so you can deliver software quickly

Basic process:

1. Build the image
2. Ship it
3. Run it

## Installation
In order to run Docker , we need to [download it](https://www.docker.com/products/docker-desktop)

## Use-cases
* [How to get started](./use-cases/how-to-get-started.md)
* [Push & pull](./use-cases/pulling-and-pushing.md)

## Useful commands

* `$ docker images` : to check which images I've had in my machine (The image ID is a SHA that will be the identifier of the image, compare to the image above, it's the same image that we built previously)
* `$ docker ps`: (docker processes) useful to check which  processes from docker are running. If we add the flag `-a` we'll see all the images (running or not) `$ docker ps -a`
* `$ docker start <image_name>`: Command to restart a process/image
* `$ docker stop <image_name>`: Command to stop an image
* `$ docker rm <image_name>`: To remove images
* `$ docker logs -f <image_name`: To see the logs of the process. If we don't add `-f` we'll see just the logs until that moment