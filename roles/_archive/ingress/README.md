# Ingress

This role sets up stack with the [homecentr/traefik](https://github.com/homecentr/docker-traefik) image. Traefik serves as an SSL termination proxy and allows publishing multiple web apps under different (sub)domains on the same IP/port.

## Role Variables

| Name | Default value | Description |
|----|-----------|-----------|
| ingress_certificates_dir | | Path to the directory where SSL certificates are stored (usually `<certbot_root_dir>/certs`) |
| ingress_certificates_group_id | | GID of a secondary group the traefik process should be a member of to be able to read the SSL certificates. |
| ingress_dashboard_domain | | Domain on which the Traefik dashboard should be published. |
| ingress_docker_image_version | latest | Docker image tag in case you want to pin to a specific version |
| ingress_group_id | 9002 | GID the traefik process in the container will use. The group will be created on the host machine if it does not exist. |
| ingress_group_name | ingress | Name of the group the traefik process in the container will use. |
| ingress_log_level | `error` | Log level of the traefik container, valid values are defined in [Traefik's docs](https://doc.traefik.io/traefik/observability/logs/#level). |
| ingress_root_dir | | Root directory where ingress containers will store their data. |
| ingress_user_id | 9002 | UID the traefik process in the container will use. The user will be created on the host machine if it does not exist. |
| ingress_user_name | ingress | Name of the user the traefik process in the container will use. |

## Auth
TODO: TBA when centralized SSO middleware is available.

## Availability

### Downtime impact on the user
Downtime of ingress effectively cuts the users off all services which are published through ingress and the service is therefore considered **CRITICAL**.

### High availability strategy
The traefik container runs on all nodes of swarm cluster and the DNS to all services should be set as multi-answer. I.e. the DNS server will return IP addresses of all nodes in the cluster. Browsers automatically failover to the next address if the first one does not work. This may be visible to the user until the browser realizes it should failover to the next ip but without an external load balancer it's the best option available.

## Data resiliency

Ingress does not store/produce any critical data and can be recreated using this ansible role from scratch.