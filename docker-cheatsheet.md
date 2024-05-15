# Docker Cheat Sheet

Docker is a platform that simplifies the process of building, shipping, and running applications using containers. 

## Table of Contents

- Basic Concepts
- Docker Commands
- Building Images
- Managing Containers
- Networking
- Volumes
- Docker Compose
- Tips and Best Practices


## Basic Concepts

- **Image:** A read-only template for creating containers.
- **Container:** A runnable instance of an image.
- **Dockerfile:** A text file that defines how to build an image.
- **Registry:** A repository for storing and distributing images (e.g., Docker Hub).
- **Docker Compose:** A tool for defining and running multi-container applications.


## Docker Commands

| Command                  | Description                                  | Example                                               |
| ------------------------- | -------------------------------------------- | ----------------------------------------------------- |
| `docker images`          | List local images                            | `docker images`                                      |
| `docker ps`              | List running containers                       | `docker ps -a` (all containers)                   |
| `docker run`             | Create and start a container                 | `docker run -d -p 80:80 nginx`                       |
| `docker build`           | Build an image from a Dockerfile              | `docker build -t my-image .`                         |
| `docker pull`            | Pull an image from a registry                | `docker pull nginx`                                |
| `docker push`            | Push an image to a registry                  | `docker push my-image`                             |
| `docker stop`            | Stop a running container                     | `docker stop my-container`                          |
| `docker start`           | Start a stopped container                    | `docker start my-container`                         |
| `docker restart`         | Restart a container                          | `docker restart my-container`                        |
| `docker rm`              | Remove a container                          | `docker rm my-container`                           |
| `docker rmi`             | Remove an image                             | `docker rmi my-image`                              |
| `docker logs`            | Fetch the logs of a container                | `docker logs my-container`                           |
| `docker exec`            | Execute a command in a container             | `docker exec -it my-container bash`                  |


## Building Images

- **Dockerfile:** Use keywords like `FROM`, `RUN`, `COPY`, `ENV`, `EXPOSE`, `CMD` to define image layers.
- **Build Context:** The directory where `docker build` is executed.


## Managing Containers

- **Port Mapping:** `-p hostPort:containerPort`
- **Environment Variables:** `-e VAR_NAME=value`
- **Mounting Volumes:** `-v hostPath:containerPath`
- **Interactive Mode:** `-it` (for shell access)
- **Naming Containers:** `--name my-container`


## Networking

- **Bridge Network:** Default network for communication between containers on the same host.
- **User-defined Networks:** Create isolated networks with custom IP ranges.

## Volumes

- **Named Volumes:** `docker volume create my-volume`
- **Bind Mounts:** Directly mount host directories into containers.


## Docker Compose

- **docker-compose.yml:** Define services, networks, volumes in a YAML file.
- **Commands:** `docker-compose up`, `docker-compose down`, `docker-compose ps`, `docker-compose logs`


## Tips and Best Practices

- **Keep Images Small:** Use multi-stage builds and minimize unnecessary dependencies.
- **Use .dockerignore:** Exclude files from the build context.
- **Tag Images:** Use meaningful tags (e.g., `my-image:latest`, `my-image:1.0.0`)
- **Clean Up:** Remove unused containers, images, and volumes.
- **Security:** Scan images for vulnerabilities.
