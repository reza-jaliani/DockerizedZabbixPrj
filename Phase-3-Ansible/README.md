#  Dockerized Zabbix Monitoring with Ansible (Phase 3)

**Repository:** `Phase-3-Ansible`  
**Author:** [Reza Jaliani](https://github.com/reza-jaliani)  
**Purpose:** Fully automated deployment of a Dockerized Zabbix + Grafana monitoring stack with NGINX reverse proxy using Ansible and Docker compose.

---

##  Overview

This phase demonstrates real-world DevOps automation using Ansible, Docker, and modular roles. It includes:

- Installing Docker and Docker Compose
- Deploying a monitoring stack (PostgreSQL, Zabbix server, Zabbix web UI, Zabbix agent, Grafana)
- Using Jinja2 templating for configuration flexibility
- Modularizing with Ansible roles:
  - `monitoring`: sets up the Docker stack
  - `reverse-proxy`: installs & configures NGINX reverse proxy
- Implementing `healthcheck`, `depends_on` (with service health), and `handlers` for smart updates

---

##  Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/reza-jaliani/DockerizedZabbixPrj.git
cd DockerizedZabbixPrj/Phase-3-Ansible
```

### 2. Define your inventory

```ini
# inventory/hosts
[monitoring]
192.168.133.129 ansible_user=reza
```

### 3. Run the playbook

```bash
ansible-playbook -i inventory/hosts deploy-monitoring.yml --ask-become-pass
```

---

##  Project Structure

```
Phase-3-Ansible/
├── deploy-monitoring.yml             # Main playbook
├── inventory/
│   └── hosts
└── roles/
    ├── monitoring/                   # Handles Zabbix + Grafana Docker stack
    │   ├── defaults/main.yml
    │   ├── templates/docker-compose.yml.j2
    │   ├── tasks/main.yml
    │   └── handlers/main.yml
    └── reverse-proxy/               # Installs and configures NGINX
        ├── templates/nginx.conf.j2
        ├── tasks/main.yml
        └── handlers/main.yml
```

---

##  Stack Details

| Service         | Image                                             | Port(s)         | Healthcheck         |
|----------------|---------------------------------------------------|------------------|----------------------|
| PostgreSQL      | `postgres:15`                                      | —                | `pg_isready`         |
| Zabbix Server   | `zabbix/zabbix-server-pgsql:alpine-7.0-latest`    | 10051, 10050     | `zabbix_server -V`   |
| Zabbix Web UI   | `zabbix/zabbix-web-nginx-pgsql:alpine-7.0-latest` | 8080             | `curl localhost:8080`|
| Zabbix Agent    | `zabbix/zabbix-agent2:alpine-7.0-latest`          | —                | `agent.ping`         |
| Grafana         | `grafana/grafana:latest`                          | 3000             | `curl localhost:3000`|

---

##  Reverse Proxy via NGINX

The reverse-proxy role installs and configures NGINX on the host.

| Path             | Proxy Target       |
|------------------|--------------------|
| `/zabbix/`       | `localhost:8080`   |
| `/grafana/`      | `localhost:3000`   |

The default NGINX site is disabled and replaced with a custom one under `/etc/nginx/sites-available/monitoring.conf`.

---

##  Why It Matters

- Infrastructure-as-code with clean separation of logic and config
- Reusable and environment-aware via `defaults/main.yml`
- Dynamically rendered config with Jinja2
- Handlers and healthchecks make the deployment safe and smart
- Prepares the ground for TLS/SSL, Basic Auth, or domain-based routing

---

##  Next Steps

- Add TLS (Let's Encrypt or custom certs)
- Enable Basic Auth on `/zabbix` and `/grafana`
- Build CI/CD pipeline for infrastructure deployment
- Extend to monitor multiple environments/targets

---

Made by Reza Jaliani  
[LinkedIn](https://www.linkedin.com/in/reza-jaliani) | [GitHub](https://github.com/reza-jaliani)
