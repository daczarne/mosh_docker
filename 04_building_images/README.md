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

An example `Dockerfile` at this point would look like:

``` Dockerfile
FROM node:14.16.0-alpine3.13
```

## Copying files into the image

Once we have selected a base image, we need to copy the application files into the image. We use the `COPY` command to copy files or directories from the current directory (the directory where the `Dockerfile` is located) into the image. The destination can be a directory in the image. If it doesn't exist, Docker will create it.

We can specify multiple fils and or directories to copy by simply listing them with spaces. Remember that the list is case-sensitive. We can also use patterns to specify the list of files. If we want to copy everything in the current directory into the image, we use a period (`.`).

The destination can be a directory, in which case we need to end it with a forward-slash (so `/app/`, not `/app`). We can use relative directories if the first set the `WORKDIR` command. If the destination is the `WORKDIR`, then we use a period (`.`).

An example `Dockerfile` at this point would look like:

``` Dockerfile
FROM node:14.16.0-alpine3.13
WORKDIR /app
COPY . .
```

If one of our files has a space in its name, then we need to use the array format of the `COPY` command:

``` Dockerfile
FROM node:14.16.0-alpine3.13
WORKDIR /app
COPY ["hello world.txt", "."]
```

Alternatively, we can use the `ADD` command. This command has all the same features of the `COPY` command but:

- we can add files from URLs (`http://.../file.json`)
- if we supply a compressed file, `ADD` will automatically decompress it in the image. (`ADD file.zip`)

The best practice is to use `COPY`. When setting the `WORKDIR` the container will start there if we run it in interactive mode.

## Excluding files

We don't need to build and ship images with all the application dependencies. We can add files that explain how the environment needs to be built, exclude the dependencies and libraries themselves, and then add the command to re-establish the environment.

To ignore files, we use a `.dockerignore` file. Any file or directory that we include in that file will be excluded at build time.

## Running commands

As part of the container build process we can run commands using the `RUN` keyword. For example, we can use this to install dependencies. Docker will download and install all dependencies when building, so that they are available when the image is used in a container.

``` Dockerfile
FROM node:14.16.0-alpine3.13
WORKDIR /app
COPY . .
RUN npm install
```

In each invocation of the `RUN` command we can pass a command. We can have as many invocations as needed.

## Setting environment variables

To set environment variables we use the `ENV` instruction. To it we need to supply `key=value` pairs. The `=` sign can be omitted, but that is no longer considered best practice.

``` Dockerfile
FROM node:14.16.0-alpine3.13
WORKDIR /app
COPY . .
RUN npm install
ENV API_URL=http://api.myapp.com/
```
