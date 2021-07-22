# Ingress

This role sets up stack with the [homecentr/certbot](https://github.com/homecentr/docker-certbot) image. Certbot provides SSL certificate(s) used by other services and renews them automatically.

## Variables

| Name | Default value | Description |
|----|-----------|-----------|
| docker_image_version | latest | Docker image tag in case you want to pin to a specific version |
| container_gid | 9003 | GID the connector process in the container will use. The group will be created on the host machine if it does not exist. |
| container_uid | 9003 | UID the connector process in the container will use. The user will be created on the host machine if it does not exist. |
| group_name | certbot | Name of the group the connector process in the container will use. |
| user_name | certbot | Name of the user the connector process in the container will use. |
| certbot_use_staging | false | Sets whether Certbot should use Let's encrypt's staging servers which have higher throttling limits which is useful for testing the configuration. |
| certbot_acme_email |  | E-mail you registered with Let's encrypt. |
| certbot_cloudflare_api_token | | .... |

## High availability & service criticality

**Downtime impact** - first execution is critical as the ingress and other dependent services would not start. Later on keeping this service running is **NOT CRITICAL** because the certificates are valid for 90 days and it's good enough if a failed instance is restarted within this timeframe.

**Resiliency strategy** - providing that the volume where certbot stores the issues certificates is replicated across multiple nodes, certbot can run on any of these nodes.

## Data resiliency

**Human error** - the certificate can be automatically re-issued and therefore it is easily recoverable. Creating regular snapshots of the storage is still recommended though for quicker recovery.

**Node failure** - data is replicated across multiple nodes so there's no impact.

**Site failure** - the certificate can be automatically re-issued and therefore it is easily recoverable.