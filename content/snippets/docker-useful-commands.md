---
title: "Docker Useful Commands"
date: 2020-04-08T15:57:01+02:00
draft: true
---


## build the docker image

```bash
docker build . -t <image_tag:version>
```

## create and start the container

```bash
docker run --name <container_name> -d -p 5000:5000 <image_tag>
```

## connect to the container via bash

```bash
docker exec -it mathisi-molecular-design-api bash
```

Stop container and remove all images which are not used by existing containers

```bash
docker stop mathisi-molecular-design-api
docker rm mathisi-molecular-design-api
docker image prune -a
```

Remove all stopped containers

```bash
docker rm $(docker ps -a -q)
```

Removing all unused docker images

```bash
docker rmi $(docker images -q)
```

If you wanna force 

```bash
docker rmi $(docker images -q) --force
```

When the docker image fails to start

```bash
docker run --name mathisi-molecular-design-api  -it --entrypoint /bin/sh mathisi-molecular-design 
```

## Dockerfile from a built image

Inspect layers and see how they was created

```bash
docker history --no-trunc <IMAGE_NAME_OR_ID>
```


https://www.docker.com/blog/intro-guide-to-dockerfile-best-practices/
https://docs.docker.com/develop/dev-best-practices/