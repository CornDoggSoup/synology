# Docker

## Install

1. Install from DSM > Package Center
2. A `/volume1/docker` directory is auto-created


## Check Docker Versions

Synology uses older Docker:

* Use `docker-compose` - not `docker compose`
* Compose files must be `version: '2'`
* You cannot update - the move from 2 to 3 is a paradigm shift

```bash
docker --version
# Docker version 20.10.3, build 55f0773

docker-compose version
# docker-compose version 1.28.5, build 24fb474e
# docker-py version: 4.4.4
# CPython version: 3.7.10
# OpenSSL version: OpenSSL 1.1.0l  10 Sep 2019

sudo docker info
docker ps
```

## Create a Docker Network

All containers are added to this network, each with their own static IP. This way, when configuring NGINX proxy hosts and subdomains, we can reference a container's hostname vs an IP address.

```bash
# Create Network
docker network create --subnet 172.29.7.0/24 --gateway 172.29.7.1 nas_network

# List
docker network ls

# Details
docker network inspect nas_network
```

## Use in Compose Files

```bash
# Add this to start of all compose files
networks:
  default:
    name: nas_network

# Add this to each container
# Increment the last number
    networks:
      default:
        ipv4_address: 172.77.0.3
```

## Composerize

[Composerize](https://www.composerize.com/) is a free online tool that converts Docker run commands into Compose files.