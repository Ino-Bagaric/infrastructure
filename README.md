# Infrastructure

This repository includes my core infrastructure configuration. I open sourced this because my setup was of interest to some people and I figured it would be worth sharing!

## Files

### `docker-compose.yml`

This is the main configuration file that declares the services and their configuration. This compose project represents the set of core services that run on every node in my infrastructure.

Here are the services that run:

- Watchtower: This automatically updates containers when a new version of the image they use is available on Docker Hub. This behaviour is applied selectively and is avoided on vital services.
- Traefik: The web gateway that provides a reverse proxy, load balancing and automatic routing to containers.
- Portainer: A Docker management interface for general tasks such as restarting containers, reading logs, etc.
- Grafana: Provides a graphical interface with customisable dashboards for monitoring system and application metrics.
- Prometheus: A time-series database for metrics.
- Node-exporter: Exports system-level information as Prometheus metrics for system monitoring.
- cAdvisor: Exports container-level information as Prometheus metrics for Docker monitoring.

### `machinehead.json`

This is an example configuration for [Machinehead](https://github.com/Southclaws/machinehead) which facilitates [GitOps](https://www.weave.works/blog/gitops-operations-by-pull-request) by reacting to Git repository changes by running `docker-compose up` to update deployments.

### `traefik.toml`

Configures Traefik to:

- Automatically register HTTPS certificates
- Route HTTP traffic to Docker containers using label selection
- Provide a monitoring interface
- Provide metrics for Prometheus

### `prometheus.yml`

Configures Prometheus to scrape:

- Node Exporter
- cAdvisor
- Traefik

### `grafana/`

Contains Grafana configuration for provisioning data sources and dashboards as well as a set of dashboards for displaying the metrics captured by Prometheus:

- Docker and System: provides an overview of the container infrastructure including resource usage.
- Go Processes: Golang specific monitors to display metrics such as Goroutines and heap size.
- Node Exporter: Underlying host system metrics such as CPU load, disk usage, network traffic, etc.
- Traefik Realtime Stats: An interface for the traffic metrics that Traefik provides.
