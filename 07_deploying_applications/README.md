# Deploying applications

- [Deploying applications](#deploying-applications)
  - [Deployment options](#deployment-options)
  - [Virtual Private Server](#virtual-private-server)
  - [Docker Machine](#docker-machine)

## Deployment options

We have two deployment options:

- deploying to a single-host
- deploying to a cluster

Single-host deployment is simple and easy, but they don't scale up. Cluster deployment requires orchestration tools. Currently, the most popular orchestration tool is Google's Kubernetes.

## Virtual Private Server

To deploy our application we need a VPS. We can procure it from platforms like Digital Ocean, GCP, Azure, AWS, etc.

## Docker Machine

Once we have a server we need a tool called Docker Machine to talk to the Docker Engine on that server. With it we can execute Docker commands locally, and they will be sent to the Docker Engine on the server. You can get Docker Machine from [here](https://github.com/docker/machine/releases).

As usual, to verify that it has been installed run:

``` shell
docker-machine --version
```
