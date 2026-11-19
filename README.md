# Automated Linux Server Configuration using Ansible

## Project Overview

This project automates Linux server configuration using Ansible and AWS EC2.

It uses a reusable Ansible playbook and role-based architecture to configure Dev, Staging, and Production environments.

The objective is to reduce manual server administration, maintain consistent configurations, and make deployments repeatable and idempotent.

## Technologies Used

- Ansible
- AWS EC2
- Ubuntu Linux
- YAML
- Jinja2
- SSH
- Nginx
- UFW Firewall
- Git and GitHub

## Architecture

Ansible Control Machine (Ubuntu) connects to AWS EC2 managed nodes through SSH.

The same playbook is executed against different environments using separate inventory files and environment-specific variables.

## Project Structure

```text
automated-linux-server/
├── ansible.cfg
├── .gitignore
├── README.md
├── files/
├── inventory/
│   ├── dev.example
│   ├── staging.example
│   ├── production.example
│   └── group_vars/
│       ├── dev.yml
│       ├── staging.yml
│       └── production.yml
├── playbooks/
│   ├── site.yml
│   ├── firewall.yml
│   ├── nginx-role.yml
│   └── other learning playbooks
├── roles/
│   └── nginx/
│       ├── defaults/
│       ├── files/
│       ├── handlers/
│       └── tasks/
└── templates/
    └── app.conf.j2
```

## Features

1. Installs and configures required packages.
2. Creates the DevOps group and user.
3. Creates application directories and log files.
4. Deploys environment-specific configuration using Jinja2 templates.
5. Configures Nginx using a reusable Ansible role.
6. Configures UFW firewall rules for SSH, HTTP, and HTTPS.
7. Supports Dev, Staging, and Production environments.
8. Uses Ansible idempotency to avoid unnecessary changes.

## Environment Configuration

| Environment | Application port | Debug |
|---|---:|---|
| Development | 8080 | true |
| Staging | 8081 | true |
| Production | 80 | false |

Environment-specific values are maintained in `inventory/group_vars/`.

## Prerequisites

- Ubuntu control machine
- Ansible installed
- AWS EC2 Ubuntu instances
- SSH access to managed nodes
- Python 3 on managed nodes
- Appropriate AWS Security Group rules

## Configure an Inventory

Copy an example inventory and customize it with your own server address and SSH key path.

For example:

```bash
cp inventory/dev.example inventory/dev
```

Edit `inventory/dev` with your actual EC2 connection details.

Repeat for staging and production when required.

**Important:** Real inventory files and private keys should never be committed to a public repository.

## Run the Playbook

Check syntax:

```bash
ansible-playbook --syntax-check playbooks/site.yml
```

Deploy to Development:

```bash
ansible-playbook -i inventory/dev playbooks/site.yml
```

Deploy to Staging:

```bash
ansible-playbook -i inventory/staging playbooks/site.yml
```

Deploy to Production:

```bash
ansible-playbook -i inventory/production playbooks/site.yml
```

## Verify Connectivity

```bash
ansible dev -i inventory/dev -m ping
ansible staging -i inventory/staging -m ping
ansible production -i inventory/production -m ping
```

## Idempotency Testing

Run the playbook again after the initial configuration.

Ansible should report no unnecessary changes when the target servers already match the desired configuration.

Final project verification:

| Environment | Tasks | Changed | Failed |
|---|---:|---:|---:|
| Dev | 18 | 0 | 0 |
| Staging | 18 | 0 | 0 |
| Production | 18 | 0 | 0 |

## Learning Outcomes

- Ansible inventory management
- Ad-hoc commands and playbooks
- Package and user management
- File and directory management
- Variables and Jinja2 templates
- Handlers and notifications
- Reusable Ansible roles
- Multi-environment automation
- Firewall configuration
- Idempotency testing

## Security Notes

- Never commit private SSH keys.
- Keep real server inventories out of public repositories.
- Verify SSH host keys.
- Restrict SSH access to trusted IP addresses.
- Use appropriate AWS Security Group and firewall rules.

## Project Status

Completed practical project covering automated Linux server configuration across three AWS EC2 environments.

