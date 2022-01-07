# Running multi-container applications

- [Running multi-container applications](#running-multi-container-applications)
  - [Installing Docker Compose](#installing-docker-compose)
  - [Cleaning up our workspace](#cleaning-up-our-workspace)

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
