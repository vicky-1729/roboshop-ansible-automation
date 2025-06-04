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

The repository is organized as follows:

```
roboshop-ansible-automation/
├── ansible_module.md   # Documentation for Ansible modules used in the project
├── roboshop.ini        # Ansible inventory file with all target hosts
├── repo_config/        # Configuration templates and repo files for each service
│   ├── <service>.repo
│   └── <service>.conf
├── service/            # Systemd unit files for each microservice
│   └── <service>.service
├── mongodb.yml         # Playbook for MongoDB setup
├── mysql.yml           # Playbook for MySQL setup
├── rabbitmq.yml        # Playbook for RabbitMQ setup
├── redis.yml           # Playbook for Redis setup
├── catalogue.yml       # Playbook for Catalogue microservice
├── user.yml            # Playbook for User microservice
├── cart.yml            # Playbook for Cart microservice
├── shipping.yml        # Playbook for Shipping microservice
├── payment.yml         # Playbook for Payment microservice
├── frontend.yml        # Playbook for Frontend microservice
└── README.md           # Main documentation file
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
