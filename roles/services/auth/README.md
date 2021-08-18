# Auth

[Pomerium](https://www.pomerium.com/) based authentication and authorization for applications which do not support direct OAuth integration. The authentication is enabled on other services by using the `auth@pomerium` Traefik middleware.

## Role Variables

| Name | Default value | Description |
|------|---------------|-------------|
| TBA | |

## Availability

### Downtime impact on the user
All services protected by the proxy are not available, therefore this service is considered **CRITICAL**.

### High availability strategy
The container can run on any node in the cluster. The only requirement is the config files to be on the node. Pomerium may store data about active sessions but if these are lost and the service is restarted without it, the impact is negligible, the users just have to sign in again.

## Data resiliency
The service does not produce any irreplacible data.