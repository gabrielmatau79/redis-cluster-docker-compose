
# Redis Cluster Setup with Docker Compose

This guide will help you set up a Redis Cluster using Docker Compose.

## Prerequisites

- Docker installed on your system
- Docker Compose installed on your system

## Steps to Set Up the Cluster

1. **Clone the Repository or Download Files**

   Ensure the `redis-cluster-docker-compose` is in your working directory.

2. **Create the Docker Network**

   Run the following command to create the Docker network:

   ```bash
   docker network create --subnet=172.29.0.0/16 redis-cluster
   ```

3. **Deploy the Cluster**

   Run the following command to deploy the cluster:

   ```bash
   docker-compose -f redis-cluster-docker-compose.yml up -d
   ```

4. **Create the Redis Cluster**

   After deploying, create the Redis Cluster:

   ```bash
   docker exec -it redis-node-1 redis-cli --cluster create \
   172.29.0.2:6379 172.29.0.3:6379 172.29.0.4:6379 \
   172.29.0.5:6379 172.29.0.6:6379 172.29.0.7:6379 \
   --cluster-replicas 1
   ```

## Files Included

- `redis-cluster-docker-compose`: Docker Compose configuration for the Redis Cluster.
- Redis nodes configuration files for each node under `./redis/node-X/conf/`.

**note:** for each file you must be ensure that `cluster-announce-ip` has the IP corresponding to node into compose.

example:

    ```
    port 6379
    bind 0.0.0.0
    cluster-enabled yes
    cluster-config-file nodes.conf
    cluster-node-timeout 5000
    cluster-announce-ip 172.29.0.2 
    cluster-announce-port 6379
    cluster-announce-bus-port 16379
    appendonly yes
    ```

## Notes

- Make sure to update IP addresses if you change the subnet.
- For more advanced configurations, modify the Redis configuration files in `./redis/node-X/conf/`.

## Troubleshooting

If you encounter any issues, check the Docker logs:

```bash
docker-compose logs
```

## License

This project is licensed under the MIT License.