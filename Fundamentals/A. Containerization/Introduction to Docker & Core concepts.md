# Introduction to Docker

[Docker is an open platform for developing, shipping, and running applications.](https://docs.docker.com/get-started/docker-overview/)

By using Docker we can mitigate the "it works on my machine" problem
-- Docker helps "isolate" applications from the underlying system, making it easier to replicate and run the same application on different environments.


Why not use a VM instead? --- VM is a full-fledged OS with hardware drivers, programs, and applications. It's heavy and slow to start for only one application.

But, provisioned VM (often in cloud environment) can run multiple containerised apps.


# Core concepts

Isolated application with its dependencies and configuration bundled together -- this is a **container**.
* ```docker run ubuntu``` will create a container with Ubuntu OS.
* ```docker ps``` will list all running containers --- get container_id
* ```docker stop <container_id>``` will stop a container.
* ```docker rm <container_id>``` will remove a container.
* ```docker stats``` will show resource usage of containers.
* ```docker logs <container_id>``` will show logs of a container.

To make container replication easier, we use **images**. An image is a read-only template with instructions for creating a Docker container. 

Images can use other images as a base; each image instruction in a **Dockerfile** is a **layer**, and layers are cached to speed up the build process.

Images can be stored and shared in a **registry**. The default registry is Docker Hub. Other registries: Amazon ECR, Google Container Registry, your own private registry.
* ```docker pull/run/push``` are commands to interact with required images via configured registry.

The degree of isolation between containers is controlled by **namespaces** and **cgroups**. Namespaces provide isolation for processes, networking, and filesystems, while cgroups manage resource allocation.

Docker objects such as images, containers, networks, and volumes are managed by **Docker daemon** through the Docker API.