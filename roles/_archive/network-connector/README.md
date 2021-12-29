# Network connector

This role sets up stack with the [homecentr/swarm-local-network-connector](https://github.com/homecentr/docker-swarm-local-network-connector) image. This image allows connecting Docker containers to networks which use drivers which are not normally compatible with Docker Swarm (e.g. macvlan).

## Variables

| Name | Default value | Description |
|----|-----------|-----------|
| network_connector_docker_image_version | latest | Docker image tag in case you want to pin to a specific version |
| network_connector_group_id | 9001 | GID the connector process in the container will use. The group will be created on the host machine if it does not exist. |
| network_connector_group_name | network-connector | Name of the group the connector process in the container will use. |
| network_connector_user_id | 9001 | UID the connector process in the container will use. The user will be created on the host machine if it does not exist. |
| network_connector_user_name | network-connector | Name of the user the connector process in the container will use. |

## Availability

### Downtime impact on the user
This container is only required when a new container is started (a container which actually needs to be connected using this connector). In case the connector is down at that momnent, the container will not start correctly or the service will not be available. Therefore it is considered to be **CRITICAL**.

### High availability strategy
The container runs on each node and can be automatically restarted in case it fails because of a transient issue. The application does not have state it would need to maintain and no dependencies. Effectively the only reason why it could fail is a bug in the code itself.

## Data resiliency

This component is fully stateless and does not store/produce any data.