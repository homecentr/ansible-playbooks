# Auth

[Pomerium](https://www.pomerium.com/) based authentication and authorization for applications which do not support direct OAuth integration. The authentication is enabled on other services by using the `auth@pomerium` Traefik middleware.

## Role Variables

| Name | Default value | Description |
|------|---------------|-------------|
    - auth_docker_image_version
    - auth_group_id
    - auth_group_name
    - auth_user_id
    - auth_user_name
    - auth_domain
    - auth_root_dir
    - auth_cookie_secret
    - auth_oidc_client_id
    - auth_oidc_client_secret
    - auth_oidc_url
    - auth_signing_key
    - auth_enable_verification_app

# openssl ecparam  -genkey  -name prime256v1  -noout | base64

## Availability

### Downtime impact on the user
All services protected by the proxy are not available, therefore this service is considered **CRITICAL**.

### High availability strategy
The container can run on any node in the cluster. The only requirement is the config files to be on the node. Pomerium may store data about active sessions but if these are lost and the service is restarted without it, the impact is negligible, the users just have to sign in again.

## Data resiliency
The service does not produce any irreplacible data.