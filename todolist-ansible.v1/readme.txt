Deploying the application Todolist with Ansible (all components on 1 VM)

This repository contains Ansible configuration for deploying the
ToDoList application using encrypted variables via Ansible Vault.

Requirements:
- Ansible 2.9+
- SSH access to target servers
- Vault decryption password (will be prompted during execution)

Setup

    1. Clone the repository:
    git clone https://github.com/sudolicious/todolist-ansible.git
    cd todolist-ansible.v1

    2. Run the playbook:
    ansible-playbook playbook.yml --ask-vault-pass

All sensitive data is stored encrypted in:
inventory.yml
vars.yml

Project structure
.
├── readme.txt # This file
├── ansible.cfg # Ansible configuration
├── vars.yml # Encrypted variables
├── inventory.yml # Encrypted inventory file
├── playbook.yml # Main playbook
└── roles/ # Ansible roles
├── backend/ # Backend application role
│ ├── tasks/ # Tasks
│ └── templates/ # Configuration templates
├── common/ # Common settings
│ └── tasks/ # Common tasks
├── frontend/ # Frontend application role
│ └── tasks/ # Tasks
└── postgres/ # PostgreSQL database role
└── tasks/ # Tasks
