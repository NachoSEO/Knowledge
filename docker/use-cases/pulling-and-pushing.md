# Pulling and pushing from Docker Hub

1. Register in https://hub.docker.com/
2. Create a repository
  * Same process as in Github/Bitbucket
3. Add a tag to the image: `$ docker tag <tag_name> <repository_name>/<image_name>`
4. Push the image: `$ docker push <repository_name>/<image_name>:<tag>`
  * i.e. `$ docker push user/hello-world:latest` If we don't add the tag, by default is `latest`
  * This will work too: `$ docker push user/hello-world`
5. We can now pull the image: `$ docker pull <repository_name>/<image_name>`
  * i.e. `$ docker pull user/hello-world`