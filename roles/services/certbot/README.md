# Certbot

This role sets up stack with the [homecentr/certbot](https://github.com/homecentr/docker-certbot) image. Certbot issues SSL certificate(s) used by other services and renews them automatically using the Let's encrypt service.

## Role Variables

| Name | Default value | Description |
|----|-----------|-----------|
| certbot_docker_image_version | latest | Docker image tag in case you want to pin to a specific version |
| certbot_group_id | 9003 | GID the certbot process in the container will use. The group will be created on the host machine if it does not exist. |
| certbot_group_name | certbot | Name of the group the certbot process in the container will use.  |
| certbot_user_id | 9003 | UID the certbot process in the container will use. (The user will be created on the host machine if it does not exist.) |
| certbot_user_name | certbot | Name of the user the certbot process in the container will use. |
| certbot_use_staging | false | Sets whether Certbot should use Let's encrypt's staging servers which have higher throttling limits which is useful for testing the configuration. |
| certbot_acme_email |  | E-mail you want to use with Let's encrypt. Let's encrypt will send warnings to this e-mail address when the certificate is close to expiration. This must be a valid e-mail address. |
| certbot_cloudflare_api_token | | .... |

## Availability

### Downtime impact on the user
First execution is critical as the ingress and other dependent services would not be able to start. Later on keeping this service running is **not critical** because the certificates are valid for 90 days and therefore it's good enough if a failed instance is restarted within this timeframe.

### High availability strategy
Providing that the volume where certbot stores the issues certificates is replicated across multiple nodes, swarm can restart the certbot container on any available node without any impact to the consumers.

## Data resiliency

### Human error
The SSL certificate can be automatically re-issued in case the data is lost. Creating regular snapshots of the storage is still recommended though for simpler recovery and an option to restore to a previous state.

### Hardware failure (hard drive, server node)
As a part of resiliency strategy, the data should be replicated across multiple nodes which handles this event.

### Site failure
The SSL certificate can be automatically re-issued in case the data is lost. Back up to secondary site is therefore **not required**.