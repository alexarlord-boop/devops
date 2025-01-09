Compose works in all environments; production, staging, development, testing, as well as CI workflows. It also has commands for managing the whole lifecycle of your application:

1. Start, stop, and rebuild services 
2. View the status of running services 
3. Stream the log output of running services 
4. Run a one-off command on a service

[Source](https://docs.docker.com/compose/)

* Allows to define and run multi-container Docker applications
* Compose cashes the build context by default.
* Use of Variables -> multi-environment support

# Use cases of Compose
## Development environment
Document and configure all of the application's service dependencies (databases, queues, caches, web service APIs, etc) in a single Compose file.
```
docker compose up -d
```

## Automated testing
Compose allows to create and destroy isolated testing environments in a simple manner.
```
docker compose up -d
./run_tests
docker compose down
```

[Compose app Example](https://docs.docker.com/compose/intro/compose-application-model/#illustrative-example)

[Sample demo apps worth checking out](https://github.com/docker/awesome-compose)


# The Compose file
* ```compose.yaml``` -- preferred canonical name, put in the working directory.
* ```docker-compose.yaml``` -- earlier name, still supported.

### Key commands
* ```docker-compose up``` -- start the services
* ```docker-compose down``` -- stop the services
* ```docker-compose ps``` -- list the services
* ```docker-compose logs``` -- view the logs


### General structure
```yaml
name: myapp  # becomes COMPOSE_PROJECT_NAME

services:
    frontend:
      ...
    backend:
      ...
    some_service:
      ...
networks:
  ...
volumes:
  ...
configs:
  ...
secrets:
  ...
```

# Top-level elements in Compose file
[Wordpress lab](./wordpress_lab) -- 2 services, 1 network, 1 volume

## [Services](https://docs.docker.com/reference/compose-file/services/)
* A service definition contains configuration that is applied to each container started for that service.
* ```build``` section is optional, it describes how to build Docker image for the service(context, DF, target). [Compose Build specification](https://docs.docker.com/reference/compose-file/build/)
* ```deploy``` section is optional, it describes container's behavioral specs(mode, replicas, resource limits). [Compose Deploy specification](https://docs.docker.com/reference/compose-file/deploy/)
* ```develop``` section is optional, it describes how to run the service in development mode(watch, action, exec..). [Compose Develop specification](https://docs.docker.com/reference/compose-file/develop/)

<details>
  <summary>Services attributes by groups</summary>

#### Basic configuration options:
```yaml
container_name: Custom name for the container.
image: Docker image to use for the service.
build: Configuration to build the image locally.
entrypoint: Override the default entrypoint for the container.
command: Override the default command for the container.
working_dir: Set the working directory inside the container.
```
#### Resource management:
```yaml
cpu_count, cpu_percent, cpu_shares, cpu_period, cpu_quota, cpu_rt_runtime, cpu_rt_period, cpus, cpuset: CPU-related configurations.
mem_limit, mem_reservation, memswap_limit, mem_swappiness: Memory usage limits and reservations.
pids_limit: Maximum number of processes allowed.
blkio_config: I/O performance configuration.
```

#### Networking:
```yaml
ports: Maps container ports to the host.
expose: Exposes ports to linked services without publishing them to the host.
networks: Specifies networks the service should connect to.
network_mode: Networking mode (e.g., bridge, host, none).
dns, dns_opt, dns_search: Custom DNS configurations.
extra_hosts: Additional hostname-to-IP mappings.
hostname: Set the container’s hostname.
domainname: Set the container’s domain name.
mac_address: Custom MAC address.
```

#### Storage & Volumes:
```yaml
volumes: Mount host or named volumes into the container.
volumes_from: Mount volumes from other containers.
tmpfs: Temporary file systems in memory.
storage_opt: Storage driver options for specific storage backends.
```

#### Environment variables:
```yaml
environment: Environment variables for the container.
env_file: External file to define environment variables.
labels, label_file: Add metadata to the service.
```

#### Logging & debugging & monitoring:
```yaml
logging: Specifies the logging driver and options.
stdin_open, tty: Keeps standard input open or allocates a pseudo-TTY.
healthcheck: Configuration for checking container health.
oom_kill_disable, oom_score_adj: OOM killer behavior for the container.
```

#### Development & deployment:
```yaml
deploy: Swarm-specific deployment configurations (replicas, update strategy).
profiles: Groups services to be selectively started.
pull_policy: Policy for pulling images (e.g., always, if-not-present).
```

#### Secrets & configs:
```yaml
secrets: Mount secrets into the container.
configs: Mount configuration data into the container.
credential_spec: Credential configuration for Windows containers.
```

#### Extension:
```yaml
extends: Reuse configuration from another service.
external_links: Connect to containers not managed by Compose.
```

#### Lifecycle:
```yaml
post_start, pre_stop: Hooks to execute custom commands during lifecycle events.
```
</details>



## [Networks](https://docs.docker.com/reference/compose-file/networks/)
* Compose creates a single network by default.
* If a network is specified: define ```networks``` attribute in ```services``` top-level element to allow services use this network.

Networks attributes:
```yaml
driver: Specifies the network driver to use for the network. Default is bridge for single-host setups and overlay for Swarm setups. (bridge, overlay, host).
driver_opts: Provides additional options for the network driver. This is a key-value dictionary of driver-specific parameters. For example, you can configure custom MTU or other driver features.
attachable: Allows standalone containers to attach to this network. Useful when you need to connect containers that are not part of the Compose file to the network. Only applicable to overlay networks.
enable_ipv6: Enables IPv6 support on the network. When set to true, the network will support IPv6 addressing in addition to IPv4.
external: Indicates that the network is managed outside the scope of the current Compose file. When set to true, Compose will not attempt to create the network and will assume it already exists.

ipam: Specifies IP Address Management (IPAM) configuration for the network. This is an object containing options such as
    driver: IPAM driver to use (e.g., default).
    config: Subnet and gateway configurations for the network.
        subnet: Subnet in CIDR format that represents a network segment
        ip_range: Range of IPs from which to allocate container IPs
        gateway: IPv4 or IPv6 gateway for the master subnet
        aux_addresses: Auxiliary IPv4 or IPv6 addresses used by Network driver, as a mapping from hostname to IP
    options: Additional driver-specific options.
    
internal: Restricts the network so it is not accessible from outside the Docker host. Containers on this network can only communicate with other containers on the same network.
labels: Adds metadata to the network as key-value pairs. This is useful for organizing or identifying networks in a system with many resources. (environment:development).
```

## Volumes
* Volumes (top-level declaration) are reusable configurable named storage units that can be mounted into one or more containers.
* Usage: define ```volumes``` attribute in ```services``` top-level element to allow services use this volume.
<details>
  <summary>Example of use in services</summary>

```yaml
services:
  backend:
    image: example/database
    volumes:
      - db-data:/etc/data

  backup:
    image: backup-service
    volumes:
      - db-data:/var/lib/backup/data

volumes:
  db-data:
```
</details>

Volume attributes:
```yaml
driver: Specifies the volume driver to use for the volume. (local, nfs, etc).
driver_opts: Provides additional options for the volume driver. This is a key-value dictionary of driver-specific parameters.
external: Indicates that the volume is managed outside the scope of the current Compose file. When set to true, Compose will not attempt to create the volume and will assume it already exists.
labels: Adds metadata to the volume as key-value pairs. This is useful for organizing or identifying volumes in a system with many resources. (environment:development).
name: Specifies the name of the volume. This is a unique identifier for the volume.
```

## [Configs](https://docs.docker.com/reference/compose-file/configs/)
* Configs are mounted files that let containers adapt without rebuilding the Docker image.
* Source of a config: ```file``` or ```external```
* If ```external```  set to true, all other attributes are ignored (except ```name```).
* ```enviroment``` variable can be used to pass the config to the container.
* ```content``` attribute can be used to pass the config content directly.
* ```name``` name of the config object.

## Secrets


# Extra features
### Watch
* allows to make changes in the source code and apply them without restarting the compose built setup.
* File changes -> Compose syncs changes under /code in the container -> app update.
```yaml
...
some_service:
  build: .
  develop:
    watch:
      - action: sync
        path: .
        target: /code
...
```
* ```docker compose up --watch``` or ```docker compose watch``` to start the watch mode.

### Include
* allows to split the compose file into multiple files.
* useful for large projects: decompose the file into smaller, more manageable parts.

how it was:
```yaml
services:
    frontend:
      ...
    backend:
      ...
    some_service:
      ...
...
```
decomposition -- created new file ```some_service.yaml```:
```yaml
services:
  some_service:
    ...
```
used ```include``` to add the sub-Compose file in the main one:
```yaml
include:
  - some_service.yaml
services:
    frontend:
      ...
    backend:
      ...
```

### Fragments

### Extensions

### Merge

### Interpolation



<table>
<tr>
<th> Good </th>
<th> Bad </th>
</tr>
<tr>
<td>

```c++
int foo() {
    int result = 4;
    return result;
}
```

</td>
<td>

```c++
int foo() { 
    int x = 4;
    return x;
}
```

</td>
</tr>
</table>