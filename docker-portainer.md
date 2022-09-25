# Docker and Portainer


## Install Docker

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

## Composerize

[Composerize](https://www.composerize.com/) is a free online tool that converts Docker run commands into Compose files.


## Install Portainer

After this step, check DSM > Docker - you will see the running container.

```bash
# Escalate to root
sudo -i
password

# Go here
cd /volume1/docker

# Create volume
mkdir portainer-ce

# Create compose file - paste from below
cd portainer-ce
nano docker-compose.yml

# Docker up
docker-compose up -d

# Exit sudo -i
exit
```

## docker-compose.yml

```yml
version: '2'

services:
  portainer:
    image: portainer/portainer-ce:latest
    container_name: portainer
    hostname: portainer
    restart: unless-stopped
    ports:
      # - 8000:8000   # 💡 Edge agents
      - 9443:9443     # ⚠️ Web UI
    volumes:
      - /volume1/docker/portainer-ce:/data
      - /var/run/docker.sock:/var/run/docker.sock
```

## Sign In

Portainer generates a self-signed SSL so you can sign in at an HTTP URL.

1. https://192.168.1.209:9443
2. Follow screen to create a new user
3. Select 'Get Started' to use local environment


## References

* https://docs.portainer.io/start/install/server/docker/linux
* https://www.portainer.io/
* https://www.youtube.com/watch?v=bLHWxtrU8Tg
* https://www.wundertech.net/how-to-install-portainer-on-a-synology-nas
