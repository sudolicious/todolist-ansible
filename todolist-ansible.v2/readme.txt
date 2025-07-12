Deploying the Todolist application with Ansible (infrastructure of 4 VMs) using encrypted variables via Ansible Vault.

Application is served via http://altenar-intership-2025.com

Architecture Overview:
VM1: Nginx reverse proxy serving static frontend and routing API requests
VM2 & VM3: Identical backend application servers
VM4: Database server

Requirements:
- Ansible 2.9+
- 4 VMs (Linux)
- SSH access to target servers
- Vault decryption password (will be prompted during execution)

Setup

    1. Clone the repository:
    
    git clone https://github.com/sudolicious/todolist-ansible.git
    cd todolist-ansible.v2

    2. Configure DNS on VMs:
    
    ansible-playbook playbooks/dns.yml --ask-vault-pass

    3. Deploy the application for 4 VMs:
    
    ansible-playbook playbooks/playbook.yml --ask-vault-pass

    4. Rolling Updates with Zero Downtime (2 backend versions: v1.0.0 and v1.0.1)
    
    4.1 Downgrade to v1.0.0:
    
    ansible-playbook playbooks/rolling-update.yml -e "backend_version=v1.0.0" --ask-vault-pass
    
    This version doesn't have endpoint /health
    Check on VM2 or VM3: curl -v http://localhost:8080/health
    
    You'll see output:
    [GET /health HTTP/1.1
    Host: localhost:8080
    User-Agent: curl/7.81.0
    Accept: */*

   < HTTP/1.1 404 Not Found
   < Date: Wed, 12 Jul 2023 14:30:45 GMT
   < Content-Type: text/html; charset=utf-8
   < Content-Length: 123
   < Connection: keep-alive]

   4.2 Upgrade to v1.0.1:
   
   ansible-playbook playbooks/rolling-update.yml -e "backend_version=v1.0.1" --ask-vault-pass
   
   This version has endpoint /health
   Check on VM2 or VM3: curl -v http://localhost:8080/health
    
   You'll see output:
   [GET /health HTTP/1.1
    Host: localhost:8080
    User-Agent: curl/7.81.0
    Accept: */*

    < HTTP/1.1 200 OK
    < Content-Type: application/json
    < Content-Length: 23
    < Connection: keep-alive
    < X-Response-Time: 12ms]

All sensitive data is stored encrypted in:
todolist-ansible.v2/group_vars

Project structure:

├── ansible.cfg              # Main Ansible configuration file
├── group_vars               # Group variables for different server groups
│   ├── all.yml              # Variables applied to all hosts
│   ├── backend.yml          # Backend-specific variables
│   ├── databases.yml        # Database server variables
│   └── frontend.yml         # Frontend-specific variables
├── inventory.yml            # Inventory file defining all hosts and groups
├── playbooks                # Directory containing all playbooks
│   ├── dns.yml              # Playbook for DNS configuration
│   ├── playbook.yml         # Main deployment playbook
│   ├── readme.txt           # Playbooks documentation
│   └── rolling-update.yml   # Playbook for zero-downtime updates
├── readme.txt               # Main project documentation
└── roles                    # Ansible roles directory
    ├── backend              # Backend application role
    │   ├── handlers         # Handlers for backend service
    │   │   └── main.yml     # Handler definitions (e.g., service restart)
    │   ├── tasks            # Backend deployment tasks
    │   │   └── main.yml     # Main tasks file (installation, configuration)
    │   └── templates        # Configuration templates
    │       └── backend.service.j2  # Systemd service template
    ├── frontend             # Frontend role
    │   ├── handlers         # Frontend handlers
    │   │   └── main.yml     # Frontend service handlers
    │   └── tasks            # Frontend deployment tasks
    │       └── main.yml     # Main frontend tasks (NPM install, build etc.)
    ├── nginx                # Nginx reverse proxy role
    │   ├── handlers         # Nginx handlers
    │   │   └── main.yml     # Nginx service handlers
    │   ├── tasks            # Nginx configuration tasks
    │   │   └── main.yml     # Main Nginx setup tasks
    │   └── templates        # Nginx config templates
    │       └── altenar.conf.j2  # Virtual host configuration template
    └── postgres             # PostgreSQL database role
        ├── handlers         # DB handlers
        │   └── main.yml     # Database service handlers
        └── tasks            # Database setup tasks
            └── main.yml     # Main DB tasks (installation, user creation etc.)
