---
title: Docker Notes
date: 2025-10-10
type: note
tags:
  - Docker
  - CLI
---

A collection of useful Docker commands.

[[toc]]

## Docker CLI

### Docker Run

To run a container from an image, you can use the `docker run` command.

```bash
# Run a container in detached mode and map port 8080 to 80
docker run -d -p 8080:80 docker/welcome-to-docker
```

### Docker ps

To list all running containers.

```bash
docker ps
```

> [!NOTE]
> Use the `-a` flag to show all containers, including those that have stopped.

```bash
docker ps -a
```

### Docker stop

To stop a running container, you need its ID.

```bash
docker stop <container-id>
```

### Docker Image

Here are some common commands for managing Docker images.

| Command                    | Description                     |
| -------------------------- | ------------------------------- |
| `docker images`            | List all images                 |
| `docker images -a`         | List all images (including all) |
| `docker rmi <image-id>`    | Remove a specific image         |
| `docker rmi <repo>:<tag>`  | Remove by repository:tag        |
| `docker rmi <id1> <id2>`   | Remove multiple images          |
| `docker image prune`       | Remove all dangling images      |
| `docker image prune --all` | Remove all unused images        |
