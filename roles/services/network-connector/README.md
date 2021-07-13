# Network connector

This role sets up stack with the [homecentr/swarm-local-network-connector](https://github.com/homecentr/docker-swarm-local-network-connector) image. This image allows connecting Docker containers to networks which use drivers which are not normally compatible with Docker Swarm (e.g. macvlan).

## Variables

| Name | Default value | Description |
|----|-----------|-----------|
| docker_image_version | latest | Docker image tag in case you want to pin to a specific version |
| container_gid | 9001 | GID the connector process in the container will use. The group will be created on the host machine if it does not exist. |
| container_uid | 9001 | UID the connector process in the container will use. The user will be created on the host machine if it does not exist. |
| group_name | network-connector | Name of the group the connector process in the container will use. |
| user_name | network-connector | Name of the user the connector process in the container will use. |