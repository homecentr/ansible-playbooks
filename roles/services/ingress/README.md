# Traefik

This role sets up stack with the [homecentr/traefik](https://github.com/homecentr/docker-traefik) image. Traefik serves as an SSL termination proxy and allows publishing multiple web apps under different (sub)domains on the same IP/port.

## Variables

| Name | Default value | Description |
|----|-----------|-----------|
| docker_image_version | latest | Docker image tag in case you want to pin to a specific version |
| container_gid | 9002 | GID the connector process in the container will use. The group will be created on the host machine if it does not exist. |
| container_uid | 9002 | UID the connector process in the container will use. The user will be created on the host machine if it does not exist. |
| group_name | traefik | Name of the group the connector process in the container will use. |
| user_name | traefik | Name of the user the connector process in the container will use. |