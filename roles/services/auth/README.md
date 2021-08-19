# Auth

[Pomerium](https://www.pomerium.com/) based authentication and authorization for applications which do not support direct OAuth integration. The authentication is enabled on other services by using the `auth@pomerium` Traefik middleware.

## Role Variables

| Name         | Default value | Description |
|--------------|---------------|-------------|
| auth_cookie_secret | | Secret which will be used to encrypt and sign the session cookies. Pomerium devs suggest generating one using the `head -c32 /dev/urandom | base64` shell command. |
| auth_docker_image_version | `latest` | Docker image tag in case you want to pin to a specific version. |
| auth_domain | | Domain on which the Pomerium authentication site should be published. |
| auth_group_id | `9005` | GID the Pomerium process in the container will use. The group will be created on the host machine if it does not exist. |
| auth_group_name | `auth` | Name of the group the traefik process in the container will use. |
| auth_idp_type | | Type of the [identity provider](https://www.pomerium.io/docs/identity-providers/) |
| auth_idp_url | | Root url of the [identity provider](https://www.pomerium.io/docs/identity-providers/) |
| auth_idp_scopes | | Scopes which will be requested during the sign in OAuth flow |
| auth_idp_client_id | | Client ID you get from you [identity provider](https://www.pomerium.io/docs/identity-providers/) after registering the app/client. |
| auth_idp_client_secret | | Client secret you get from you [identity provider](https://www.pomerium.io/docs/identity-providers/) after registering the app/client. |
| auth_idp_service_account | | Additional config some of the identity provider [identity provider](https://www.pomerium.io/docs/identity-providers/) require to have access to additional data about the user like group membership etc. |
| auth_root_dir | | Root directory where auth containers will store their data. |
| auth_signing_key | | Key which will be used to sign the JWT tokens Pomerium passes to the downstream applications. Pomerium devs suggest generating one using the  `openssl ecparam -genkey -name prime256v1  -noout | base64` shell command. |
| auth_user_id | `9005` | UID the Pomerium process in the container will use. The user will be created on the host machine if it does not exist. |
| auth_user_name | `auth` | Name of the user the traefik process in the container will use. |

## Availability

### Downtime impact on the user
All services protected by the proxy are not available, therefore this service is considered **CRITICAL**.

### High availability strategy
The container can run on any node in the cluster. The only requirement is the config files to be on the node. Pomerium may store data about active sessions but if these are lost and the service is restarted without it, the impact is negligible, the users just have to sign in again.

## Data resiliency
The service does not produce any irreplacible data.