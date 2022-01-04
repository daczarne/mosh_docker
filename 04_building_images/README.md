# Building images

## Images and containers

- An **Image** includes everything an application needs to run
- A **Container** provides an isolated environment for running the application

Each container gets its base from the image, but each one has its own write layer.

## Dockerfile instructions

The first step to dockerize an application is to add a `Dockerfile` to it. This file will contain the instructions for building an image. Instructions can be:

- `FROM` for specifying the base image.
- `WORKDIR` for setting the working directory. Once we set it, all subsequent commands will be run from that directory.
- `COPY` and `ADD` for copying files and directories into the container.
- `RUN` for executing operating system commands.
- `ENV` for setting environment variables.
- `EXPOSE` for exposing the application on a given port
- `USER` for specifying the user that should run the application. This should be a user with limited privileges.
- `CMD` and `ENTRYPOINT` for specifying the command that should be executed when we start a container.

## Choosing the right base image

Images can be in any registry. The default that Docker will use is DockerHub, but there are others (like MCR). When not using DockerHub, we need to supply the complete URL for the `FROM` instruction. For each base image (like `node`, `python`, etc) we can have multiple (even hundreds) of different tags that combine versions of our runtime and OS. Choosing the correct one is important- But doing so requires understanding the specifications of the application, requirements, dependencies, etc. and therefore needs to be done on a case by case basis.

Once we are ready to build the image, we run:

``` shell
docker build -t IMAGE_NAME .
```

To check the images that we have, we use:

``` shell
docker images
```

To start a container using the image that we've built, we use:

``` shell
docker run -it IMAGE_NAME
```

If we want to start the container in a different program than the default (for example, start in `bash`) we run:

``` shell
docker run -it IMAGE_NAME bash
```

Or `shell`:

``` shell
docker run -it IMAGE_NAME sh
```
