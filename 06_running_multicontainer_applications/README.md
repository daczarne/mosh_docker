# Running multi-container applications

- [Running multi-container applications](#running-multi-container-applications)
  - [Installing Docker Compose](#installing-docker-compose)
  - [Cleaning up our workspace](#cleaning-up-our-workspace)
  - [Creating a compose file](#creating-a-compose-file)
    - [Docker compose version](#docker-compose-version)
    - [Services](#services)
    - [Builds and images](#builds-and-images)
    - [Mapping ports](#mapping-ports)
    - [Setting environment variables](#setting-environment-variables)
    - [Volumes](#volumes)

## Installing Docker Compose

Docker Compose is shipped with Docker Desktop on MacOS and Windows. To verify that it has been installed run:

``` shell
docker-compose --version
```

## Cleaning up our workspace

To remove all Docker images run:

``` shell
docker container rm -f $(docker container ls -a -q)
docker image rm $(docker image ls -q)
```

The first command stops all containers, the second one removes all images.

## Creating a compose file

Docker compose will look for a file called `docker-compose.yml` in the root of the project.

### Docker compose version

The first property that we need to set is the `version`. This is not the version of our software, but the version of compose files. Each version of compose files has a compatibility relationship with the Docker engine that we are using. You can read all about them [here](https://docs.docker.com/compose/compose-file/compose-versioning/).

``` yaml
version: "3.8"
```

### Services

Next we need to define the `services` of our application. This will be an object with keys set to the name of the service (like `backend`, `frontend`, `db`, etc). The names that we give to the services is arbitrary.

``` yaml
version: "3.8"

services:
  frontend:
  backend:
  database:
```

``` yaml
version: "3.8"

services:
  web:
  api:
  db:
```

### Builds and images

Here we are defining services, telling Docker how to build images for each of them, and how to run them. Therefore, once we specified a service, the first thing we need to tell Docker is how to build the image for that service. For that we use the `build` key. Its value should be the path to the appropriate `Dockerfile`.

If for some particular service we don't have a `Dockerfile` but will just use the base image (for example, from DockerHub), then we need specify the `image` key and its value should be the name of the image.

``` yaml
version: "3.8"

services:
  web:
    build: ./frontend
  api:
    build: ./backend
  db:
    image: mongo:4.0-xenial
```

### Mapping ports

Next we need to map ports. To do so we start a new array called `ports`. Each mapping needs to be of the form `HOST_PORT:CONTAINER_PORT`.

``` yaml
version: "3.8"

services:
  web:
    build: ./frontend
    ports:
      - 3000:3000
  api:
    build: ./backend
    ports:
      - 3001:3001
  db:
    image: mongo:4.0-xenial
    ports:
      - 27017:27017
```

### Setting environment variables

If we need to define environment variables, we can do so with the `environment`. To it we can supply an array with values, or an array of objects. In the case of a data base URL the value should be `db-engine://host-name/db-name`.

Docker compose will generate the necessary hosts. Each service that we define will be a host whose name is equal to the name we give it in the `docker-compose.yml` file.

``` yaml
version: "3.8"

services:
  web:
    build: ./frontend
    ports:
      - 3000:3000
  api:
    build: ./backend
    ports:
      - 3001:3001
    environment:
      - DB_URL=mongodb://db/vidly
  db:
    image: mongo:4.0-xenial
    ports:
      - 27017:27017
```

``` yaml
version: "3.8"

services:
  web:
    build: ./frontend
    ports:
      - 3000:3000
  api:
    build: ./backend
    ports:
      - 3001:3001
    environment:
      DB_URL: mongodb://db/vidly
  db:
    image: mongo:4.0-xenial
    ports:
      - 27017:27017
```

### Volumes

To add a volume we use the `volumes` key. This can be an array of volumes. Each value of the array should be of the form `VOLUME_NAME:CONTAINER/DIRECTORY/PATH`.

``` yaml
version: "3.8"

services:
  web:
    build: ./frontend
    ports:
      - 3000:3000
  api:
    build: ./backend
    ports:
      - 3001:3001
    environment:
      DB_URL: mongodb://db/vidly
  db:
    image: mongo:4.0-xenial
    ports:
      - 27017:27017
    volumes:
      - vidly:/data/db
```

Since we have used a volume, we need to define it in the compose file. This requires a different object called `volumes`. Each volume that we use needs to be an object in the `volumes` object.

``` yaml
version: "3.8"

services:
  web:
    build: ./frontend
    ports:
      - 3000:3000
  api:
    build: ./backend
    ports:
      - 3001:3001
    environment:
      DB_URL: mongodb://db/vidly
  db:
    image: mongo:4.0-xenial
    ports:
      - 27017:27017
    volumes:
      - vidly:/data/db

volumes:
  vidly:
```
