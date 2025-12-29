# Usage Guide

This guide explains how to configure and deploy the Peryton infrastructure on a new server.

## Prerequisites

*   A target machine (VM or Dedicated Server) running Debian 11/12 or Ubuntu 20.04/22.04.
*   Root or sudoer SSH access without password to the target machine.
*   Ansible installed on your control machine.

## Quick Start

### 1. Inventory Configuration

Edit the `hosts` file to add the IP address of your target server:

```ini
[peryton]
192.168.X.X ansible_user=debian
```

### 2. Variable Configuration

Copy the template of variables for your host. The filename MUST correspond to the IP address or hostname defined in the inventory.

```bash
cp host_vars/template.yml host_vars/192.168.X.X.yml
```

Edit this file `host_vars/192.168.X.X.yml` and REQUIREDLY fill in:
*   `user`: The remote system user.
*   `vpn_password`: The VPN admin password.
*   `gogs_users`: The list of students/users.

### 3. Running Deployment

Run the Ansible playbook:

```bash
ansible-playbook -i hosts playbook.yml
```

The complete deployment takes a few minutes. Once finished, the machine will reboot (configured in the Docker role).

## Common Commands

### Update Service Configurations Only

If you have modified configuration files (templates) and want to apply them without reinstalling everything:

```bash
ansible-playbook -i hosts playbook.yml --tags "services"
```

### Add New Users

If you add new users to `gogs_users`, you can run only the relevant roles:

```bash
ansible-playbook -i hosts playbook.yml --tags "gogs,vpn,email"
```
*(Note: Ensure tags exist in the playbook, otherwise run the full playbook, Ansible is idempotent).*

### Update Certificates

```bash
ansible-playbook -i hosts playbook.yml --tags "certificats"
```

## Accessing Services

Once deployed and if you are connected to the VPN (or locally), services are accessible at the following addresses:

*   **Homepage**: `https://homepage.cyber`
*   **Gogs**: `https://gogs.cyber`
*   **Wireguard UI**: `https://vpn.cyber`
*   **Documentation**: `https://doc.cyber`
