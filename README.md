Используя Ansible, автоматизировать развертывание инфраструктуры для веб-приложения на нескольких виртуальных машинах или в облаке. Проект включает настройку веб-сервера, базы данных, балансировщика нагрузки и мониторинга.

# Ansible Web App Deployment

## Overview
This project automates the deployment of a Flask web application using Ansible. The infrastructure includes:
- 2 web servers (Nginx + Flask)
- PostgreSQL database on web1
- HAProxy load balancer
- Prometheus and Grafana for monitoring

## Architecture
![Diagram](diagram.png) <!-- Создайте диаграмму в draw.io -->

## Setup
1. Install Vagrant and VirtualBox.
2. Run `vagrant up` to create 3 Debian 12 VMs.
3. Run `ansible-playbook -i inventory/hosts.yml playbooks/setup_infra.yml`.

## Access
- Application: `http://<lb-ip>`
- Prometheus: `http://<lb-ip>:9090`
- Grafana: `http://<lb-ip>:3000`

## Improvements
- Add SSL with Let's Encrypt.
- Integrate CI/CD with GitHub Actions.
- Use Ansible Vault for secrets.
