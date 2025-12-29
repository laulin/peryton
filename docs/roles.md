# Roles Documentation

This document details each Ansible role present in the project, its responsibilities, and associated configuration variables.

## Role List

### 1. `certificats`

Generates a local PKI infrastructure (Certificate Authority and certificates) to secure communications.

**Variables:**

| Variable | Description | Default Value | Secret? |
| :--- | :--- | :--- | :--- |
| `ca_dir` | CA storage directory | `/server/pki/localca` | No |
| `crl_dir` | CRL storage directory | `/server/pki/crl` | No |
| `certs_dir` | Generated certificates directory | `/server/pki/certs` | No |
| `ca_key` | Path to CA private key | `{{ ca_dir }}/ca.key` | **YES** |
| `ca_crt` | Path to CA certificate | `{{ ca_dir }}/ca.crt` | No |
| `ca_serial` | Path to CA serial file | `{{ ca_dir }}/ca.srl` | No |
| `wildcard_key` | Path to wildcard private key | `{{ certs_dir }}/wildcard.cyber.key` | **YES** |
| `wildcard_csr` | Path to wildcard CSR | `{{ certs_dir }}/wildcard.cyber.csr` | No |
| `wildcard_crt` | Path to wildcard certificate | `{{ certs_dir }}/wildcard.cyber.crt` | No |
| `openssl_san_conf` | Path to OpenSSL SAN config | `{{ certs_dir }}/san_wildcard.cnf` | No |
| `ca_subject` | CA certificate subject | `/C=FR/.../CN=Local Root CA` | No |
| `wildcard_subject` | Wildcard certificate subject | `/C=FR/.../CN=*.cyber` | No |
| `ca_days` | CA validity duration (days) | `3650` | No |
| `wildcard_days` | Certificate validity duration (days) | `365` | No |
| `ca_export_name` | Export name for CA cert | `ca-root.crt` | No |

---

### 2. `docker`

Installs Docker and Docker Compose on the host.

**Variables:**
No major configuration variables. Uses official packages.

---

### 3. `etc_hosts`

Configures the target machine's `/etc/hosts` file to redirect local domains (`*.cyber`) to `127.0.0.1`. Useful if no local DNS is configured upstream.

**Variables:**
Domains are hardcoded in the task (e.g., `gogs.cyber`, `homepage.cyber`).

---

### 4. `fail2ban`

Installs and configures Fail2Ban to protect the Caddy reverse proxy against brute force attacks or scanners.

**Variables:**

| Variable | Description | Default Value | Secret? |
| :--- | :--- | :--- | :--- |
| `fail2ban_bantime` | Ban duration (seconds) | `3600` | No |
| `fail2ban_findtime` | Detection time window (seconds) | `600` | No |
| `fail2ban_maxretry` | Max retry attempts before ban | `1` | No |
| `fail2ban_logpath` | Caddy access log path | `/server/caddy/logs/access.log` | No |
| `fail2ban_filter_name`| Name of the custom filter | `caddy-json` | No |
| `fail2ban_jail_name` | Name of the jail | `caddy-json` | No |
| `ignoreip` | List of IPs/CIDRs to ignore | `192.168.0.0/24` | No |

---

### 5. `gogs_config`

Initializes the Gogs instance and creates Git users.

**Variables:**

| Variable | Description | Default Value | Secret? |
| :--- | :--- | :--- | :--- |
| `gogs_users` | List of users to create (see format below) | `[]` | **YES** |
| `gogs_url` | URL of Gogs instance | `http://localhost:10880` | No |
| `gogs_admin_user` | Admin username | `toto` | **YES** |
| `gogs_admin_pass` | Admin password | `titi` | **YES** |
| `gogs_admin_email` | Admin email | `toto@example.com` | No |
| `gogs_smtp_host` | SMTP Host | (Empty) | No |
| `gogs_smtp_from` | SMTP Sender Address | (Empty) | No |
| `gogs_smtp_user` | SMTP User | (Empty) | **YES** |
| `gogs_smtp_passwd` | SMTP Password | (Empty) | **YES** |
| `gogs_offline_mode` | Enable offline mode | `false` | No |
| `gogs_disable_gravatar`| Disable Gravatar | `false` | No |
| `gogs_enable_federated_avatar`| Enable federated avatars | `false` | No |
| `gogs_disable_registration`| Disable registration | `true` | No |
| `gogs_enable_captcha` | Enable captcha | `true` | No |
| `gogs_require_sign_in_view`| Require sign-in to view | `false` | No |
| `gogs_enable_register_confirmation`| Enable register confirmation | `false` | No |
| `gogs_enable_notify_mail`| Enable notify mail | `false` | No |

Format of `gogs_users`:
```yaml
gogs_users:
  - name: "alice"
    email: "alice@example.com"
    password: "password123"
```

---

### 6. `send_credentials`

Sends connection information (password, VPN config) to users via email.

**Variables:**

| Variable | Description | Default Value | Secret? |
| :--- | :--- | :--- | :--- |
| `vpn_password` | VPN admin password | (None) | **YES** |
| `gogs_users` | User list (must contain `email`) | `[]` | **YES** |

---

### 7. `services`

Deploys the main Docker Compose stack and associated configuration files.

**Variables:**

| Variable | Description | Default Value | Secret? |
| :--- | :--- | :--- | :--- |
| `user` | System user owning files | (None) | No |
| `directories` | List of directories to create | (See defaults/main.yml) | No |

---

### 8. `ufw`

Configures the UFW firewall.

**Variables:**
Configuration is mainly static:
- Deny incoming by default.
- Allow outgoing by default.
- Allow SSH (port 22/tcp).
- Allow Wireguard (port 51820/udp).

---

### 9. `vpn_users`

Generates Wireguard client configurations via the `wg-easy` API.

**Variables:**

| Variable | Description | Default Value | Secret? |
| :--- | :--- | :--- | :--- |
| `vpn_password` | VPN admin password (required for API) | (None) | **YES** |
| `gogs_users` | List of users to create VPN access for | `[]` | **YES** |
