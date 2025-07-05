# Dockerized Zabbix Project 

This repository documents my personal journey of migrating Zabbix to a modern, Docker-based, automated infrastructure using DevOps best practices.

##  Project Phases Overview

###  Phase 1 – Core Foundation
**Goal:** Rapid and reliable Zabbix setup  
- Using **Docker Compose** to run Zabbix Server, Frontend, and Database services  
- Managing **volumes**, **networks**, and service dependencies with `depends_on`  
- Environment variables handled via `.env` file  
- Initial testing with: `docker compose up -d` and access to the Zabbix GUI

###  Phase 2 – Observability & Reverse Proxy
**Goal:** Improve observability and access security  
- Added **Zabbix Agent** on host and inside containers  
- Connected **Grafana** to the Zabbix DB and built first dashboards  
- Deployed **Nginx** (or Traefik) as a reverse proxy with optional TLS/HTTPS support
- Added **healthchecks** with shell scripts and cron jobs
- Implemented **backup strategies** (mysqldump, volume mounts)
- Managed **secrets** using `.env` (with plans for more secure storage)
- 

###  Phase 3 – Configuration Automation with Ansible
**Goal:** Begin automating Dockerized monitoring stack deployment using Ansible  
- Learned and implemented Ansible basics (inventory, playbooks, roles)
- Wrote Ansible roles to install Docker, deploy Zabbix and Grafana using Compose
- Integrated Jinja2 templates for dynamic docker-compose.yml generation
- Structured automation logic to prepare for future CI/CD implementation
- Using **Ansible** to install Docker and deploy Compose files
- Using **Ansible** to deploy monitoring and reverse proxy infrastructure

###  Phase 4 – Infrastructure as Code (IaC)
**Goal:** Full infrastructure automation  
- Using **Terraform** to provision VMs or cloud resources  
- Integration of **Terraform + Ansible** for end-to-end provisioning

###  Phase 5 and Beyond (Future Plans)
- **Alerting**: Integrate with Slack or Telegram  
- **Swarm / Kubernetes**: Upgrade to orchestration solutions  
- **Centralized Logging**: Loki + Promtail integration  
- **Advanced IaC**: Use Vault for secrets, Molecule for Ansible testing  

---

##  Repository Structure

- `.env` – Environment variables  
- `docker_compose_template.yml` – Base Docker Compose for Phase 1  
- `phase-2/` – Updated stack with Grafana & Nginx  
- `phase-3-ansible/` – Infrastructure deployment using Ansible  

---

##  Quick Start Guide

1. Clone the repo:
   ```bash
   git clone https://github.com/reza-jaliani/DockerizedZabbixPrj.git
   cd DockerizedZabbixPrj
   ```

2. Launch Phase 1 (basic stack):
   ```bash
   cp .env .env.example
   docker compose -f docker_compose_template.yml up -d
   ```

3. Launch Phase 2 (with Grafana and Nginx):
   ```bash
   cd phase-2
   cp .env .env.local
   docker compose up -d
   ```

4. Launch Phase 3 using Ansible:
   ```bash
   cd phase-3-ansible
   ansible-playbook -i inventory.yml deploy.yml
   ```

---

##  Additional Notes

- Edit your `.env` file according to your environment.  
- For better security, consider using **Vault** or **Docker Secrets**.  
- Planning to migrate to Kubernetes? This project prepares you for **Helm** and **GitOps** workflows.  
- Healthcheck scripts and backup logic are modular and can be extended.

---

##  Contributing & Feedback

This project is part of my DevOps learning journey.  
If this phase-based structure helps you or you have ideas to improve it, feel free to open an issue or a pull request.

---

**Reza Jaliani**  
Networking → Monitoring → DevOps 🚀  

_Last updated: includes Ansible and upcoming IaC strategies._
