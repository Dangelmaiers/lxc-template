
# Container Setup Guide

This document provides standardized setup instructions for container service repositories in the Dangelmaiers organization.

## Prerequisites

- Docker Engine 24.x or higher
- Docker Compose v2.x
- Network access to container registries
- Proxmox VE host with adequate resources
- User permissions for container management

## Installation

1. Initial setup
   ```sh
   # Clone this repository
   git clone https://github.com/dangelmaiers/lxc-[service-name].git
   cd lxc-[service-name]
   ```

2. Environment configuration
   ```sh
   # Copy example environment file
   cp .env.example .env
   
   # Modify environment variables
   nano .env
   ```

3. Container configuration
   ```sh
   # Copy example configuration files
   cp config/examples/container.conf.example config/container.conf
   
   # Modify configuration to match your environment
   nano config/container.conf
   ```

## Docker Configuration

### Docker Compose

The standard Docker Compose file includes:

```yaml
version: '3.8'
services:
  main-service:
    image: service-image:tag
    container_name: service-name
    restart: unless-stopped
    environment:
      - VAR1=value1
      - VAR2=value2
    volumes:
      - ./config:/config
      - data-volume:/data
    ports:
      - "8080:8080"
    networks:
      - service-network

volumes:
  data-volume:
    name: service-data

networks:
  service-network:
    name: service-network
```

### Resource Limits

Standard container resource limits:

```yaml
services:
  main-service:
    # ... other configurations
    deploy:
      resources:
        limits:
          cpus: '2'
          memory: 4G
        reservations:
          cpus: '0.5'
          memory: 1G
```

## Network Configuration

### Network Modes

| Network Mode | Use Case | Configuration |
|--------------|----------|--------------|
| Host | Performance-critical services | `network_mode: host` |
| Bridge | Most services | `networks: [service-network]` |
| None | Isolated services | `network_mode: none` |

### Port Mapping

Standard port mapping approach:

```yaml
services:
  main-service:
    # ... other configurations
    ports:
      - "published:container"  # e.g., "8080:80"
```

## Volume Management

### Volume Types

| Volume Type | Use Case | Example |
|-------------|----------|---------|
| Bind Mount | Configuration files | `./config:/config` |
| Named Volume | Persistent data | `data-volume:/data` |
| Tmpfs Mount | Sensitive ephemeral data | `type: tmpfs` |

### Data Management

Best practices for volume management:

- Separate configuration from data
- Use named volumes for persistent data
- Define explicit volume names
- Set appropriate ownership and permissions

```yaml
volumes:
  service-data:
    name: service-data
    driver: local
    driver_opts:
      type: none
      device: /path/on/host/service-data
      o: bind
```

## Container Networks

Define isolated networks for your containers:

```yaml
networks:
  service-network:
    name: service-network
    driver: bridge
    ipam:
      config:
        - subnet: 172.28.0.0/16
```

## Configuration Management

Store configuration files in the `config/` directory:

- `config/service.conf`: Main service configuration
- `config/nginx/`: Web server configuration
- `config/certs/`: SSL certificates

## Verification

Steps to verify the container is running correctly:

1. Check container status
   ```sh
   docker compose ps
   ```

2. View container logs
   ```sh
   docker compose logs -f
   ```

3. Test service functionality
   ```sh
   curl http://localhost:8080/health
   ```

## Troubleshooting

Common issues and their solutions:

| Issue | Solution |
|-------|----------|
| Container fails to start | Check logs with `docker compose logs` |
| Network connectivity issues | Verify port mappings and network configuration |
| Volume permission errors | Check file ownership and permissions |

## Next Steps

- See [MAINTENANCE.md](./MAINTENANCE.md) for maintenance procedures
- Reference [doc-containers](https://github.com/Dangelmaiers/doc-containers) for comprehensive documentation
- Review example configurations in the `config/examples/` directory

---

*This document follows the Dangelmaiers organization documentation standards. For questions or issues, contact the container services team.*

