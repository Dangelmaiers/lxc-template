# Container Service Template

<div align="center">
  <img src="images/logo.png" alt="Dangelmaiers Logo" width="80" height="80">
  <h3 align="center">lxc-template</h3>
  <p align="center">
    Standard template for container service repositories in the Dangelmaiers organization
    <br />
    <a href="#about"><strong>Explore the docs »</strong></a>
    <br />
    <br />
    <a href="#usage">View Usage</a>
    &middot;
    <a href="https://github.com/Dangelmaiers/lxc-template/issues/new?labels=bug&template=bug-report---.md">Report Bug</a>
    &middot;
    <a href="https://github.com/Dangelmaiers/lxc-template/issues/new?labels=enhancement&template=feature-request---.md">Request Feature</a>
  </p>
</div>

<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li><a href="#about">About</a></li>
    <li><a href="#container-service-context">Container Service Context</a></li>
    <li><a href="#repository-structure">Repository Structure</a></li>
    <li><a href="#usage">Usage</a></li>
    <li><a href="#naming-conventions">Naming Conventions</a></li>
    <li><a href="#integration-points">Integration Points</a></li>
    <li><a href="#contributing">Contributing</a></li>
    <li><a href="#license">License</a></li>
    <li><a href="#contact">Contact</a></li>
  </ol>
</details>

## About

This repository serves as a template for all container-based service repositories in the Dangelmaiers organization. It follows standardized naming conventions and includes required files and directory structures for consistent documentation and management of containerized services.

### Service Categories and Tags

Depending on the service category, add relevant tags:

- `genai`: Generative AI services (e.g., Ollama, OpenWebUI)
- `search`: Search services (e.g., Elasticsearch, Meilisearch)
- `storage`: Storage solutions (e.g., Nextcloud, MinIO)
- `monitoring`: System monitoring (e.g., Grafana, Prometheus)

## Container Service Context

Container repositories in the Dangelmaiers organization are structured to deploy and manage containerized services across the infrastructure. These include:

- Generative AI services
- Search services
- Storage solutions
- Monitoring tools

Each container repository defines a specific service, its configuration, and its integration with the broader infrastructure.

## Repository Structure

All container repositories follow this standard structure:

```
lxc-*/
├── README.md                # Overview and documentation
├── SETUP.md                 # Installation and configuration instructions
├── MAINTENANCE.md           # Maintenance procedures
├── docker-compose.yml       # Container configuration
└── config/                  # Configuration files
    └── examples/            # Example configuration files
```

## Usage

To use this template for a new container-based service repository:

1. Create a new repository using this template
2. Rename according to the `lxc-*` convention (e.g., `lxc-grafana`, `lxc-elasticsearch`)
3. Update the README.md with specific service details
4. Modify the docker-compose.yml file for your specific service
5. Add relevant configuration files to the `config/examples/` directory
6. Document setup procedures in SETUP.md
7. Document maintenance procedures in MAINTENANCE.md

## Naming Conventions

The Dangelmaiers organization uses the following prefix system:

- `pve-*`: Proxmox Virtual Environment related repositories
- `lxc-*`: Container specific configurations and setups
- `doc-*`: Documentation repositories

Examples of container repository names:
- `lxc-ollama`: LLM server container setup
- `lxc-elasticsearch`: Elasticsearch setup
- `lxc-nextcloud`: File hosting and sharing

## Integration Points

Container repositories typically integrate with:

1. **Infrastructure repositories (`pve-*`)**
   - Rely on Proxmox for virtualization
   - Utilize network configuration from infrastructure
   - Use storage resources defined in infrastructure

2. **Other container repositories (`lxc-*`)**
   - Service dependencies between containers
   - Network communication between services
   - Data exchange and integration

3. **Documentation repositories (`doc-*`)**
   - Service documentation references container configuration
   - Setup and maintenance procedures are documented

## Contributing

Contributions should follow the Dangelmaiers organization standards:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/improvement`)
3. Commit your changes (`git commit -m 'Add some improvement'`)
4. Push to the branch (`git push origin feature/improvement`)
5. Open a pull request

## License

Refer to the `LICENSE.txt` file for license information.

## Contact

Container Services Team - containers@dangelmaiers.org

---

*This repository follows the Dangelmaiers organization structure standards. For more information, refer to the [doc-containers](https://github.com/Dangelmaiers/doc-containers) repository.*

