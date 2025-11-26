# Ansible Project Setup Process

This guide provides a comprehensive process for setting up a new Ansible project with best practices, proper structure, and modern tooling.

## Project Structure

A well-organized Ansible project follows this structure:

```
my-ansible-project/
├── ansible.cfg              # Ansible configuration
├── requirements.yml         # Role dependencies
├── requirements.txt         # Python dependencies
├── .ansible-lint.yml       # Linting configuration
├── .yamllint.yml          # YAML linting rules
├── .gitignore             # Version control exclusions
├── README.md              # Project documentation
│
├── inventory/             # Inventory files
│   ├── production/
│   │   ├── hosts.yml     # Production hosts
│   │   └── group_vars/   # Production variables
│   ├── staging/
│   │   ├── hosts.yml     # Staging hosts
│   │   └── group_vars/   # Staging variables
│   └── development/
│       ├── hosts.yml     # Development hosts
│       └── group_vars/   # Development variables
│
├── group_vars/           # Global group variables
│   └── all/
│       ├── main.yml     # Common variables
│       └── vault.yml    # Encrypted secrets
│
├── host_vars/           # Host-specific variables
│
├── playbooks/           # Ansible playbooks
│   ├── site.yml        # Master playbook
│   ├── webserver.yml   # Web server playbook
│   └── database.yml    # Database playbook
│
├── roles/              # Ansible roles
│   ├── requirements.yml # External role dependencies
│   └── common/         # Example internal role
│
├── files/              # Static files
├── templates/          # Jinja2 templates
├── filter_plugins/     # Custom filters
├── library/            # Custom modules
└── scripts/           # Helper scripts
```

## Setup Steps

### 1. Create Project Directory

```bash
mkdir my-ansible-project && cd my-ansible-project
git init
```

### 2. Create ansible.cfg

```ini
[defaults]
# Inventory
inventory = inventory/development
host_key_checking = False
retry_files_enabled = False

# Roles
roles_path = roles:~/.ansible/roles:/usr/share/ansible/roles

# Output
stdout_callback = yaml
callback_whitelist = timer, profile_tasks, profile_roles

# SSH
pipelining = True
control_path = /tmp/ansible-ssh-%%h-%%p-%%r

# Vault
vault_password_file = .vault_pass

[inventory]
enable_plugins = host_list, script, auto, yaml, ini, toml

[ssh_connection]
ssh_args = -o ControlMaster=auto -o ControlPersist=60s
```

### 3. Create Directory Structure

```bash
# Create all necessary directories
mkdir -p {inventory/{production,staging,development}/{group_vars,host_vars},group_vars/all,host_vars,playbooks,roles,files,templates,filter_plugins,library,scripts}

# Create .gitignore
cat > .gitignore << 'EOF'
# Ansible
*.retry
*.vault_pass
.vault_pass
vault_password_file

# Python
*.pyc
__pycache__/
.pytest_cache/
venv/
.env

# IDE
.vscode/
.idea/
*.swp
*.swo

# OS
.DS_Store
Thumbs.db

# Logs
*.log
ansible.log

# Temporary
.cache/
tmp/
EOF
```

### 4. Set Up Python Dependencies

```bash
# Create requirements.txt
cat > requirements.txt << 'EOF'
ansible>=8.0.0
ansible-lint>=6.0.0
yamllint>=1.26.0
molecule>=5.0.0
molecule-plugins[docker]>=23.0.0
pytest-testinfra>=8.0.0
jmespath>=1.0.0
netaddr>=0.8.0
EOF

# Create virtual environment
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

### 5. Configure Linting

```bash
# Create .ansible-lint.yml
cat > .ansible-lint.yml << 'EOF'
---
exclude_paths:
  - .cache/
  - .github/
  - venv/
  
warn_list:
  - experimental
  - role-name[path]
  
skip_list:
  - yaml[line-length]
  - name[template]
  
enable_list:
  - fqcn-builtins
  - no-free-form
EOF

# Create .yamllint.yml
cat > .yamllint.yml << 'EOF'
---
extends: default

rules:
  line-length:
    max: 120
    level: warning
  truthy:
    allowed-values: ['true', 'false', 'yes', 'no']
  comments:
    min-spaces-from-content: 1
  braces:
    max-spaces-inside: 1
EOF
```

### 6. Create Initial Inventory

```bash
# Development inventory
cat > inventory/development/hosts.yml << 'EOF'
---
all:
  children:
    webservers:
      hosts:
        web1.dev.local:
          ansible_host: 192.168.1.10
    databases:
      hosts:
        db1.dev.local:
          ansible_host: 192.168.1.20
EOF

# Common variables
cat > group_vars/all/main.yml << 'EOF'
---
# Common variables for all hosts
ansible_user: ansible
ansible_python_interpreter: /usr/bin/python3

# Application settings
app_name: myapp
app_port: 8080

# Common packages
common_packages:
  - vim
  - htop
  - curl
  - git
EOF
```

### 7. Create Example Playbook

```bash
cat > playbooks/site.yml << 'EOF'
---
- name: Configure all servers
  hosts: all
  become: true
  
  tasks:
    - name: Update apt cache
      ansible.builtin.apt:
        update_cache: true
        cache_valid_time: 3600
      when: ansible_os_family == "Debian"
    
    - name: Install common packages
      ansible.builtin.package:
        name: "{{ common_packages }}"
        state: present

- name: Configure web servers
  hosts: webservers
  become: true
  
  roles:
    - common
    - webserver

- name: Configure databases
  hosts: databases
  become: true
  
  roles:
    - common
    - database
EOF
```

### 8. Create README

```bash
cat > README.md << 'EOF'
# My Ansible Project

Infrastructure automation using Ansible.

## Requirements

- Python 3.8+
- Ansible 8.0+
- SSH access to target hosts

## Setup

1. Create virtual environment:
   ```bash
   python3 -m venv venv
   source venv/bin/activate
   pip install -r requirements.txt
   ```

2. Configure inventory:
   - Edit `inventory/development/hosts.yml`
   - Add your hosts and variables

3. Set up vault (if using secrets):
   ```bash
   # Generate vault password
   openssl rand -base64 32 > .vault_pass
   chmod 600 .vault_pass
   ```

## Usage

Run playbooks:
```bash
# All servers
ansible-playbook playbooks/site.yml

# Specific environment
ansible-playbook -i inventory/production playbooks/site.yml

# With tags
ansible-playbook playbooks/site.yml --tags webserver

# Check mode
ansible-playbook playbooks/site.yml --check --diff
```

## Project Structure

See [ansible-setup.md](docs/ansible-setup.md) for detailed structure.

## Testing

Run linting:
```bash
ansible-lint
yamllint .
```

## License

[Your License]
EOF
```

### 9. Initialize Git Repository

```bash
git add .
git commit -m "Initial Ansible project setup"
```

## Next Steps

1. **Add Roles**: Create or download roles for your infrastructure
2. **Configure Vault**: Set up Ansible Vault for secrets management
3. **Add Tests**: Set up Molecule for role testing
4. **CI/CD**: Configure pipeline for automated testing and deployment
5. **Documentation**: Document your playbooks and roles

## Best Practices

1. **Use Roles**: Modularize your code into reusable roles
2. **Version Control**: Track all changes in Git
3. **Test Everything**: Use Molecule and ansible-lint
4. **Secure Secrets**: Always use Ansible Vault for sensitive data
5. **Document**: Keep README files updated
6. **Idempotency**: Ensure all tasks are idempotent
7. **Tags**: Use tags for selective execution
8. **Variables**: Use group_vars and host_vars appropriately