# Portainer

[Portainer](https://github.com/docker-portainer) is a web UI which allows administration of the Docker Swarm cluster Homecentr services are running in.

## Variables

| Name | Default value | Description |
|------|---------------|-------------|
| | |

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

## Manual steps
TBA