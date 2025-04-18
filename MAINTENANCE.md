
# Container Maintenance Procedures

This document outlines standardized maintenance procedures for container service repositories in the Dangelmaiers organization.

## Regular Maintenance

### Daily Tasks

- Check container status
  ```sh
  docker compose ps
  ```
- Review container logs
  ```sh
  docker compose logs --tail=100
  ```
- Monitor resource usage
  ```sh
  docker stats --format "table {{.Name}}\t{{.CPUPerc}}\t{{.MemUsage}}\t{{.NetIO}}\t{{.BlockIO}}"
  ```

### Weekly Tasks

- Check for image updates
  ```sh
  docker compose pull
  ```
- Review volume usage
  ```sh
  docker system df -v
  ```
- Run security scans
  ```sh
  docker scan <image-name>
  ```

### Monthly Tasks

- Update images and recreate containers
  ```sh
  docker compose pull && docker compose up -d
  ```
- Clean up unused resources
  ```sh
  docker system prune -a
  ```
- Audit access controls
  ```sh
  docker network inspect service-network
  ```

## Service Monitoring

### Container Health Checks

Implement health checks in your Docker Compose file:

```yaml
services:
  main-service:
    # ... other configurations
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:8080/health"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 60s
```

### Prometheus Monitoring

Standard metrics to monitor:

- Container CPU usage
- Container memory usage
- Container network I/O
- Service-specific metrics (e.g., queue depth, response time)

Example Prometheus configuration:

```yaml
services:
  main-service:
    # ... other configurations
    labels:
      - "prometheus.enable=true"
      - "prometheus.port=9090"
```

### Log Management

Configure logging options in your Docker Compose file:

```yaml
services:
  main-service:
    # ... other configurations
    logging:
      driver: "json-file"
      options:
        max-size: "10m"
        max-file: "3"
```

## Backup Procedures

### Data Backup Schedule

| Data Type | Frequency | Retention | Method |
|-----------|-----------|-----------|--------|
| Configuration | Daily | 30 days | File backup |
| Application data | Hourly | 14 days | Volume backup |
| Database | Continuous | 7 days | Database backup |

### Volume Backup Commands

```sh
# Stop service (if required)
docker compose stop

# Backup volume
docker run --rm -v service-data:/source -v /backup:/backup alpine tar -czf /backup/service-data-$(date +%Y%m%d).tar.gz -C /source .

# Restart service
docker compose start
```

### Database Backup Commands

```sh
# Backup database container
docker compose exec database pg_dump -U username database > backup/database-$(date +%Y%m%d).sql
```

## Update Procedures

### Image Updates

1. Pull latest images
   ```sh
   docker compose pull
   ```

2. Apply updates with minimal downtime
   ```sh
   docker compose up -d
   ```

3. Verify service health
   ```sh
   docker compose ps
   curl http://localhost:8080/health
   ```

### Version Pinning

Use specific version tags in your Docker Compose file to control updates:

```yaml
services:
  main-service:
    image: service-image:1.2.3  # Specific version
    # or
    image: service-image:1.2    # Minor version
    # or
    image: service-image:1      # Major version
```

### Rollback Procedure

If an update causes issues:

1. Stop the problematic containers
   ```sh
   docker compose stop
   ```

2. Modify your Docker Compose file to specify the previous version
   ```yaml
   services:
     main-service:
       image: service-image:previous-version
   ```

3. Apply the rollback
   ```sh
   docker compose up -d
   ```

## Storage Management

### Volume Cleanup

Periodically check and clean up volume usage:

```sh
# List volumes
docker volume ls

# Check volume usage
docker system df -v

# Remove unused volumes (use with caution)
docker volume prune
```

### Log Rotation

Configure log rotation to prevent disk space issues:

```yaml
services:
  main-service:
    # ... other configurations
    logging:
      driver: "json-file"
      options:
        max-size: "10m"
        max-file: "3"
```

## Troubleshooting

### Common Issues

| Issue | Diagnosis | Resolution |
|-------|-----------|------------|
| Container repeatedly crashes | Check logs with `docker compose logs` | Fix configuration issues or increase resources |
| Out of disk space | Run `docker system df` | Prune unused resources with `docker system prune` |
| Network conflicts | Check with `docker network inspect` | Update network configuration |

### Debugging Commands

Useful commands for debugging container issues:

```sh
# Check container logs
docker compose logs --tail=100 service-name

# Execute a command inside a running container
docker compose exec service-name sh

# View container details
docker inspect container_id

# Monitor real-time events
docker events --filter container=container_id
```

## Emergency Procedures

### Service Failure

1. Assess the situation
   ```sh
   docker compose ps
   docker compose logs
   ```

2. Attempt to restart the service
   ```sh
   docker compose restart
   ```

3. If restart fails, recreate containers
   ```sh
   docker compose down
   docker compose up -d
   ```

4. If issues persist, roll back to a known working version
   ```sh
   # Edit docker-compose.yml to use previous version
   docker compose up -d
   ```

---

*This document follows the Dangelmaiers organization documentation

