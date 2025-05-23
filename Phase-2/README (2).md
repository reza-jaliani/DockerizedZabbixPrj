
# Phase 2: Observability and Reverse Proxy – Dockerized Zabbix + Grafana + Nginx

This project is part of a multi-phase DevOps roadmap aiming to build a production-ready monitoring system using open-source tools.

**Phase 2 Goals:**
- Add observability to the Docker host and containers
- Integrate Grafana with Zabbix for dashboards
- Securely expose services via Nginx reverse proxy

---

## Features

- Zabbix 7.0 server, frontend, and agent running in Docker containers
- Grafana 12.0 connected to Zabbix via plugin
- Pre-provisioned Grafana dashboards and data source
- Reverse proxy with Nginx mapped to `/zabbix/` and `/grafana/`
- Docker host metrics exposed via Zabbix Agent 2
- Proper network isolation and `.env` configuration

---

## Requirements

- Docker + Docker Compose
- Linux machine (tested on Ubuntu Server 24.04)
- `docker.sock` access (for monitoring Docker from Zabbix Agent 2)

---

## Directory Structure

```
.
├── zabbix_stack.yml
├── grafana_stack.yml
├── nginx_stack.yml
├── .env
├── provisioning/
│   ├── zabbix-datasources/
│   │   └── zabbix-datasource.yml
│   └── zabbix-dashboard/
│       └── zabbix-dashboard.yml
├── nginx/
│   └── default.conf
├── zbx_env/
│   ├── pg_data/
│   ├── alertscripts/
│   └── externalscripts/
└── grafana_data/ (generated on first run)
```

---

## How to Run

1. **Clone the repository**  
   ```bash
   git clone https://github.com/YOUR_USERNAME/DockerizedZabbixPrj.git
   cd DockerizedZabbixPrj
   ```

2. **Edit your `.env` file if needed**  
   Contains environment variables like:
   ```env
   ZABBIX_VERSION=alpine-7.0-latest
   ZBX_DB_PASSWORD=zabbixpass
   ZBX_SERVER_NAME=Zabbix Docker Monitoring
   ZABBIX_FRONTEND_PORT=8080
   ZBX_TIMEZONE=Asia/Tehran
   GF_SERVER_ROOT_URL=/grafana
   GF_SERVER_SERVE_FROM_SUB_PATH=true
   GF_INSTALL_PLUGINS=alexanderzobnin-zabbix-app
   ```

3. **Start all services**  
   In order:
   ```bash
   docker compose -f zabbix_stack.yml up -d
   docker compose -f grafana_stack.yml up -d
   docker compose -f nginx_stack.yml up -d
   ```

4. **Access the services**

   | Service   | URL                   | Default Credentials     |
   |-----------|-----------------------|--------------------------|
   | Zabbix    | http://localhost/zabbix/  | Admin / zabbix           |
   | Grafana   | http://localhost/grafana/ | admin / admin            |

---

## Notes

- Grafana is pre-provisioned with the Zabbix plugin and a sample dashboard.
- The Nginx proxy allows clean access via `/zabbix/` and `/grafana/`.
- Ensure your user has permissions to access Docker socket if you're using Zabbix Agent 2 to monitor containers:
  ```bash
  sudo usermod -aG docker $USER
  ```

---

## Troubleshooting

- **Grafana doesn't start?** Make sure the `grafana_data` directory has correct permissions:
  ```bash
  sudo mkdir -p ./grafana_data
  sudo chown -R 472:472 ./grafana_data
  ```

- **Nginx proxy not redirecting?** Check `nginx/default.conf` and confirm you have:
  ```nginx
  location /grafana/ {
      proxy_pass http://grafana:3000/;
      rewrite ^/grafana/(.*) /$1 break;
  }
  ```

---

## Next Phase

> **Phase 3 – Production Readiness & Resilience**  
> Add backups, health checks, version control, secrets management, and basic CI/CD.

---

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.
