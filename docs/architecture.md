# Architecture Documentation

This document describes the general architecture of the Peryton deployment and the interaction between the different components.

## Overview

The Peryton project is a "Lab in a Box" infrastructure deployed via Ansible. It aims to provide a complete environment for cybersecurity education, including web services, simulation tools, and secure VPN access.

The architecture relies on a host machine (typically a VM or dedicated server) that hosts all services containerized via Docker. Caddy is used as a reverse-proxy to securely expose these services (TLS) via local domain names (`*.cyber`).

## Deployment Flow

The Ansible playbook orchestrates the deployment according to the following logical steps:

1.  **System Preparation (`etc_hosts`, `docker`)**:
    *   Configuration of the `/etc/hosts` file for local DNS resolution of services.
    *   Installation of Docker and Docker Compose.

2.  **Security and Certificates (`certificats`, `ufw`, `fail2ban`)**:
    *   Generation of a local Certificate Authority (CA) and wildcard certificates for the `*.cyber` domain.
    *   Configuration of the UFW firewall to limit access (SSH, Wireguard).
    *   Setup of Fail2Ban to protect the reverse-proxy.

3.  **Services Deployment (`services`)**:
    *   Creation of the file tree on the host.
    *   Deployment of the Docker Compose stack (Homepage, Gogs, Pi-hole, Caddy, Wireguard, etc.).
    *   Initial configuration of services (config files, assets).

4.  **Application Configuration (`gogs_config`, `vpn_users`)**:
    *   Initialization of Gogs and creation of user accounts.
    *   Creation of Wireguard client profiles for users via the `wg-easy` API.

5.  **Access Distribution (`send_credentials`)**:
    *   Emailing users with their credentials and VPN configuration.

## Deployed Services

The main services deployed in the Docker stack are:

*   **Caddy**: Reverse proxy and TLS termination. Uses locally generated certificates.
*   **Homepage**: Central dashboard to access all services.
*   **Gogs**: Self-hosted Git server for exercises and student submissions.
*   **Pi-hole**: DNS sinkhole (optional/configurable) for the internal network.
*   **Wireguard (wg-easy)**: VPN manager with Web interface for administration.
*   **Fast-QCM**: Quiz service for assessment.
*   **Docsify**: Static documentation server.

## Data Management

Persistent data is stored on the host in the `/server` directory (configurable). The typical structure is:

```
/server/
├── caddy/          # Caddy configuration and logs
├── compose.yml     # Main Docker Compose file
├── docsify/        # Documentation content
├── fqcm/           # Fast-QCM code and data
├── gogs/           # Gogs Git data and database
├── homepage/       # Dashboard configuration
├── pihole/         # Pi-hole configuration
├── pki/            # Certificates (CA, crt, key)
└── wg-easy/        # Wireguard configuration
```

## Security

*   **TLS Everywhere**: All web services are exposed via HTTPS using Caddy and a local PKI.
*   **Isolation**: Services run in isolated containers.
*   **Firewall**: UFW is configured in "deny incoming" mode by default, allowing only SSH and the VPN UDP port. Caddy is only accessible from the local network or via the VPN (depending on configuration).
