# Portainer

[Portainer](https://github.com/docker-portainer) is a web UI which allows administration of the Docker Swarm cluster Homecentr services are running in.

## Variables

| Name | Default value | Description |
|------|---------------|-------------|
| portainer_admin_password | &lt;random value&gt; | Portainer requires to create an internal admin user which must have a password. If you want to generate a random password, you can use [password lookup](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/password_lookup.html). |
| portainer_agent_docker_image_version | `latest` | Docker image tag of the portainer agent in case you want to pin to a specific version |
| portainer_agent_secret | | Shared secret portainer agents and server use to communicate with each other. |
| portainer_docker_image_version | `latest` | Docker image tag of the portainer server in case you want to pin to a specific version |
| portainer_domain | | Domain which should be used to publish the Portainer through [ingress](../ingress) |
| portainer_group_id | `9004` | GID the portainer server and portainer agent processes will use in their containers. The group will be created on the host machine if it does not exist. |
| portainer_group_name | `portainer` | Name of the group the portainer server and portainer agent processes will use in their containers. The group will be created on the host machine if it does not exist. |
| portainer_oauth_authorize_endpoint_url | | URL to the `authorize` endpoint of your OAuth provider. |
| portainer_oauth_token_endpoint_url | | URL to the `token` endpoint of your OAuth provider. |
| portainer_oauth_logout_url | | URL to the `logout` endpoint of your OAuth provider. |
| portainer_oauth_user_profile_endpoint_url | | URL to the `user profile` endpoint which returns data about the user like for example an e-mail. |
| portainer_oauth_user_profile_user_id_field | | Name of the field from the payload returned by the `user profile` endpoint Portainer will use as a user ID. This ID **must** be unique to each user and **must not** contain spaces (Portainer does not allow this). |
| portainer_oauth_client_id | | Client ID of the application. You get this when you register the client/application with your OAuth provider. |
| portainer_oauth_client_secret | | Client secret of the application. You get this when you register the client/application with your OAuth provider. The secret should be regularly rotated for security reasons. |
| portainer_root_dir | | Root directory where portainer containers will store their data. |
| portainer_session_duration | `720h` | Time before the users are forced to sign in again. Valid format is for example `8h` for 8 hours. |
| portainer_user_id | `9004` | UID the portainer server and portainer agent processes will use in their containers. The user will be created on the host machine if it does not exist. | 
| portainer_user_name | `portainer` | Name of the user the portainer server and portainer agent processes will use in their containers. The user will be created on the host machine if it does not exist. |

## Auth
Portainer is automatically configured to use an OAuth based SSO using the provided parameters. User permissions are managed in Portainer itself through the web UI.

## Availability

### Downtime impact on the user
If the service is down, user not able to manage Docker containers via Web UI but it's still possible via SSHing to the machine. No other services are impacted. Portainer is therefore considered **NOT CRITICAL**.

### Availability/failover strategy
The portainer server can run on any node in the cluster providing that the configuration is replicated across the nodes. Portainer agents run on every node and can be restarted in case of a failure.

## Data protection

### Human error
Periodical snapshots of configuration should be made to allow for easy recovery after a human error.

### Hardware failure (hard drive, server node)
Data should be replicated across all swarm nodes to allow HA and resiliency against hardware failure.

### Site failure
The service does not store/produce any irreplacible data, all configuration can be recreated by reapplying the role and following manual configuration guide below.