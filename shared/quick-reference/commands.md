# Ansible Quick Command Reference

## Essential Commands

### Ansible Ad-hoc Commands
```bash
# Ping all hosts
ansible all -m ping

# Run command on all hosts
ansible all -m command -a "uptime"

# Copy file to hosts
ansible webservers -m copy -a "src=/local/file dest=/remote/path"

# Install package
ansible all -m package -a "name=nginx state=present" --become

# Restart service
ansible webservers -m service -a "name=nginx state=restarted" --become

# Gather facts
ansible all -m setup

# Check syntax
ansible-playbook playbook.yml --syntax-check
```

### Playbook Execution
```bash
# Run playbook
ansible-playbook playbook.yml

# Run with specific inventory
ansible-playbook -i inventory/production playbook.yml

# Run in check mode (dry run)
ansible-playbook playbook.yml --check --diff

# Run with specific tags
ansible-playbook playbook.yml --tags "config,deploy"

# Skip specific tags
ansible-playbook playbook.yml --skip-tags "slow,backup"

# Limit to specific hosts
ansible-playbook playbook.yml --limit "webserver1,webserver2"

# Start at specific task
ansible-playbook playbook.yml --start-at-task "Install packages"

# Run with extra variables
ansible-playbook playbook.yml -e "version=1.2.3 env=prod"

# Run with vault password
ansible-playbook playbook.yml --ask-vault-pass
```

### Inventory Management
```bash
# List all hosts
ansible all --list-hosts

# List hosts in group
ansible webservers --list-hosts

# Show inventory graph
ansible-inventory --graph

# Show host variables
ansible-inventory --host webserver1

# Validate inventory
ansible-inventory --list
```

### Ansible Vault
```bash
# Create encrypted file
ansible-vault create secrets.yml

# Encrypt existing file
ansible-vault encrypt plaintext.yml

# Decrypt file
ansible-vault decrypt secrets.yml

# Edit encrypted file
ansible-vault edit secrets.yml

# View encrypted file
ansible-vault view secrets.yml

# Change vault password
ansible-vault rekey secrets.yml

# Encrypt string
ansible-vault encrypt_string 'mysecret' --name 'password'
```

### Role and Collection Management
```bash
# Install role from Galaxy
ansible-galaxy role install geerlingguy.nginx

# Install role from Git
ansible-galaxy role install git+https://github.com/user/role.git

# List installed roles
ansible-galaxy role list

# Install roles from requirements
ansible-galaxy role install -r requirements.yml

# Create new role
ansible-galaxy role init myrole

# Install collection
ansible-galaxy collection install community.general

# List installed collections
ansible-galaxy collection list
```

### Testing and Debugging
```bash
# Syntax check
ansible-playbook playbook.yml --syntax-check

# Run ansible-lint
ansible-lint playbook.yml

# Run with verbose output
ansible-playbook playbook.yml -v    # Basic verbose
ansible-playbook playbook.yml -vv   # More verbose
ansible-playbook playbook.yml -vvv  # Debug level
ansible-playbook playbook.yml -vvvv # Connection debug

# Step through tasks
ansible-playbook playbook.yml --step

# Debug specific task
ansible-playbook playbook.yml --start-at-task "Task name" -vvv
```

### Performance Optimization
```bash
# Profile task execution time
export ANSIBLE_CALLBACKS_ENABLED=profile_tasks
ansible-playbook playbook.yml

# Use strategy for parallel execution
ansible-playbook playbook.yml -e ansible_strategy=free

# Limit parallel forks
ansible-playbook playbook.yml -f 10

# Disable fact gathering
ansible-playbook playbook.yml -e gather_facts=false
```

## Configuration Tips

### ansible.cfg Quick Settings
```ini
[defaults]
# Performance
pipelining = True
gathering = smart
fact_caching = jsonfile
fact_caching_connection = /tmp/ansible_cache
fact_caching_timeout = 86400

# Output
stdout_callback = yaml
callback_whitelist = profile_tasks, timer

# SSH
host_key_checking = False
```

### Environment Variables
```bash
# Set config file location
export ANSIBLE_CONFIG=/path/to/ansible.cfg

# Set inventory
export ANSIBLE_INVENTORY=/path/to/inventory

# Set vault password file
export ANSIBLE_VAULT_PASSWORD_FILE=~/.vault_pass

# Enable debug
export ANSIBLE_DEBUG=1

# Set Python interpreter
export ANSIBLE_PYTHON_INTERPRETER=/usr/bin/python3
```

## Common Patterns

### Check if service is running
```bash
ansible all -m shell -a "systemctl is-active nginx || true"
```

### Find files
```bash
ansible all -m find -a "paths=/etc patterns='*.conf'"
```

### Create user with SSH key
```bash
ansible all -m user -a "name=deploy state=present shell=/bin/bash" --become
ansible all -m authorized_key -a "user=deploy key='{{ lookup('file', '~/.ssh/id_rsa.pub') }}'" --become
```

### Quick backup before changes
```bash
ansible all -m shell -a "cp /etc/nginx/nginx.conf /etc/nginx/nginx.conf.$(date +%Y%m%d_%H%M%S)"
```

## Troubleshooting Commands

### Test connectivity
```bash
# Test SSH connection
ansible all -m ping

# Test with specific user
ansible all -m ping -u ansible

# Test with password auth
ansible all -m ping --ask-pass
```

### Gather information
```bash
# Get all facts
ansible hostname -m setup

# Get specific facts
ansible hostname -m setup -a "filter=ansible_eth*"

# Get package facts
ansible hostname -m package_facts
```

### Debug variables
```bash
# Show all variables for a host
ansible hostname -m debug -a "var=hostvars[inventory_hostname]"

# Show specific variable
ansible hostname -m debug -a "var=ansible_distribution"
```