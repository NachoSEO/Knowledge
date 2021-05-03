# How to get started with Docker
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
