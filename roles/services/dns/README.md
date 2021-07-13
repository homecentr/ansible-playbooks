# DNS

This role sets up a Docker swarm stack of two BIND9 based DNS servers (master and slave). Both of the instances can failover to a different swarm instance but given that DNS is a core component and its downtime has severe impact on the network, there are always two instances to ensure that the service keeps working even if one of the instances is being failed over to another swarm node.

The failover logic relies on the fact that all swarm nodes where the service can run have access to the same filesyste, i.e. the filesystem should be replicated using Ceph, GlusterFS or similar technology.

## Publish method
The containers are published using the Docker macvlan driver which gives each instance a standalone IP address. This is to abstract away from the cluster architecture and the fact that most systems (e.g. Windows) require two DNS server IP addresses and in case the nodes which you are using in the client's network configuration are down, then even if the containers fail over to other nodes, the clients would not know how to contact them.

## Monitoring
Each BIND9 container has a companion Prometheus exporter container using our [custom image](https://github.com/homecentr/docker-dns-exporter). This requires BIND9 to expose the statistics endpoint which is limited to only internal docker network and should not be exposed outside of the swarm cluster (macvlan exposes all ports by default, but BIND9 is configured to refuse any traffic from outside of the cluster).

## Variables
| Name | Default value | Description |
|----|-----------|-----------|
| container_gid | 9002 | GID the bind9 process in the container will use. The group will be created on the host machine if it does not exist. |
| container_uid | 9002 | UID the bind9 process in the container will use. The user will be created on the host machine if it does not exist. |
| data_root | /var/homecentr/dns | Root directory where the container configuration and data will be stored |
| dns_docker_image_version | latest | Docker image tag of the BIND9 image in case you want to pin to a specific version |
| dns_exporter_docker_image_version | latest | Docker image tag of the Prometheus exporter in case you want to pin to a specific version |
| dns_forwarders | 8.8.8.8, 8.8.4.4 | IP address of the DNS servers where BIND9 will forward queries it cannot serve from configuration |
| dns_publish_network | | Name of the docker network the container should connect to. The container will be connected via the [network-connector](../network-connector/README.md) |
| dns_master_ipv4 | | IPv4 address the master instance should be published with. |
| dns_slave_ipv4 | | IPv4 address the slave instance should be published with. |
| dns_master_slave_secret | | The secret master and slave instances use to secure their internal communication |
| dns_forward_zone_domain | | The domain you use for your local network, e.g. example.com. This should be either a made up domain or a domain you actually own. |
| dns_services_reverse_zone_range | | The IP range of the services the DNS forward records point to. |
| dns_internal_network_cidr | 172.16.0.16/28 | The IP range for the internal docker overlay network. |
| group_name | dns | Name of the group the bind9 process in the container will use. |
| user_name | dns | Name of the user the bind9 process in the container will use. |