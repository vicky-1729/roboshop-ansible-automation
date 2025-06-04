# roboshop-ansible-automation

## Overview

`roboshop-ansible-automation` is a comprehensive automation project designed to deploy and manage the entire RoboShop e-commerce application stack using Ansible. RoboShop is a microservices-based demo application, and this repository provides all the necessary playbooks, roles, and configuration files to set up each service in a consistent, repeatable, and automated manner.

## Project Goals

- **Automate** the installation and configuration of all RoboShop microservices.
- **Standardize** deployments across environments (dev, test, prod).
- **Simplify** infrastructure management using Ansible best practices.
- **Enable** rapid, reliable, and idempotent provisioning.

## Features

- **Service Playbooks:** Individual Ansible playbooks for each microservice (MongoDB, MySQL, RabbitMQ, Redis, Catalogue, User, Cart, Shipping, Payment, Frontend).
- **Centralized Inventory:** Manage all target hosts via a single `roboshop.ini` inventory file.
- **Reusable Roles:** Modular roles for common tasks (user creation, repo setup, service management).
- **Configuration Management:** Templates and files for service configuration and systemd units.
- **Idempotency:** Safe to re-run playbooks without causing unwanted changes.
- **Documentation:** Inline comments and a dedicated `ansible_module.md` for module references.

## Directory Structure

```
roboshop-ansible-automation/
├── ansible_module.md
│   └─ Documentation of Ansible modules and their usage within the playbooks for reference and learning.
├── roboshop.ini
│   └─ Ansible inventory file listing all target hosts and their groupings (e.g., databases, application servers, frontend).
├── repo_config/
│   ├── <service>.repo
│   ├── <service>.conf
│   └─ Contains repository configuration files and service-specific configuration templates (such as YUM repo files, application config files, etc.) used by playbooks to set up each microservice.
├── service/
│   ├── <service>.service
│   └─ Systemd unit files for each microservice, enabling Ansible to register, start, and manage services as systemd-managed daemons.
├── mongodb.yml
│   └─ Ansible playbook to automate the installation and configuration of MongoDB.
├── mysql.yml
│   └─ Ansible playbook to automate the installation and configuration of MySQL.
├── rabbitmq.yml
│   └─ Ansible playbook to automate the installation and configuration of RabbitMQ.
├── redis.yml
│   └─ Ansible playbook to automate the installation and configuration of Redis.
├── catalogue.yml
│   └─ Ansible playbook to automate the deployment and configuration of the Catalogue microservice.
├── user.yml
│   └─ Ansible playbook to automate the deployment and configuration of the User microservice.
├── cart.yml
│   └─ Ansible playbook to automate the deployment and configuration of the Cart microservice.
├── shipping.yml
│   └─ Ansible playbook to automate the deployment and configuration of the Shipping microservice.
├── payment.yml
│   └─ Ansible playbook to automate the deployment and configuration of the Payment microservice.
├── frontend.yml
│   └─ Ansible playbook to automate the deployment and configuration of the Frontend microservice.
└── README.md
    └─ Main documentation file describing the project, usage instructions, and structure.
```

## How It Works

Each playbook automates the following for its respective service:
- Installs required packages and dependencies.
- Adds application users and sets permissions.
- Downloads and configures application code.
- Sets up configuration files using templates.
- Registers and starts the service with systemd.
- Verifies service status.

## Prerequisites

- **Ansible** installed on your control node (macOS/Linux/WSL).
- **SSH access** to all target hosts with sudo privileges.
- **Python** installed on target hosts.

## Getting Started

1. **Clone the repository:**
   ```sh
   git clone https://github.com/<your-username>/roboshop-ansible-automation.git
   cd roboshop-ansible-automation
   ```

2. **Edit the inventory file:**
   - Update `roboshop.ini` with your target hosts and groups.

3. **Run a playbook:**
   - To deploy MongoDB, for example:
     ```sh
     ansible-playbook -i roboshop.ini mongodb.yml
     ```
   - Repeat for other services as needed.

4. **Verify deployment:**
   - Check service status on target hosts:
     ```sh
     systemctl status <service-name>
     ```

## Example: Deploying All Services

You can run all playbooks in sequence to deploy the full stack:

```sh
ansible-playbook -i roboshop.ini mongodb.yml
ansible-playbook -i roboshop.ini mysql.yml
ansible-playbook -i roboshop.ini rabbitmq.yml
ansible-playbook -i roboshop.ini redis.yml
ansible-playbook -i roboshop.ini catalogue.yml
ansible-playbook -i roboshop.ini user.yml
ansible-playbook -i roboshop.ini cart.yml
ansible-playbook -i roboshop.ini shipping.yml
ansible-playbook -i roboshop.ini payment.yml
ansible-playbook -i roboshop.ini frontend.yml
```

## Customization

- **Variables:** Customize variables in playbooks or use group/host vars for environment-specific settings.
- **Templates:** Modify Jinja2 templates in `repo_config/` for custom configurations.
- **Roles:** Extend or create new roles for additional automation needs.

## Troubleshooting

- Use `-vvv` with `ansible-playbook` for verbose output.
- Check Ansible logs and target host logs for error details.
- Ensure SSH connectivity and sudo permissions.

## Contributing

Contributions are welcome! Please open issues or submit pull requests for improvements, bug fixes, or new features.

## License

This project is provided for educational and demonstration purposes.

---

**Author:** [Your Name or Organization]  
**Contact:** [your.email@example.com]
