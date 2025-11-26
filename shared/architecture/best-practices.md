# Ansible Best Practices Guide

## Project Organization

### Directory Layout
- Use the standard directory layout for predictability
- Separate environments using inventory directories
- Keep playbooks simple and focused on specific tasks
- Use roles for reusable components

### Naming Conventions
- Use lowercase with underscores for all names
- Be descriptive but concise
- Examples:
  - Roles: `webserver`, `database_backup`, `monitoring_agent`
  - Playbooks: `deploy_application.yml`, `update_systems.yml`
  - Variables: `nginx_port`, `db_backup_schedule`

## Variables

### Variable Precedence (lowest to highest)
1. Role defaults (`roles/x/defaults/main.yml`)
2. Inventory group_vars
3. Inventory host_vars
4. Playbook group_vars
5. Playbook host_vars
6. Host facts
7. Play vars
8. Task vars
9. Extra vars (`-e` flag)

### Variable Best Practices
- Define defaults in role defaults
- Use group_vars for environment-specific values
- Prefix role variables with role name: `nginx_port` not just `port`
- Document all variables in role README
- Use vault for sensitive data

## Tasks and Playbooks

### Task Guidelines
```yaml
# Good - descriptive name
- name: Install Nginx web server
  ansible.builtin.package:
    name: nginx
    state: present

# Good - use specific modules
- name: Start and enable Nginx service
  ansible.builtin.systemd:
    name: nginx
    state: started
    enabled: true

# Good - idempotent with conditions
- name: Create application directory
  ansible.builtin.file:
    path: /opt/myapp
    state: directory
    mode: '0755'
  when: app_deployment_needed | bool
```

### Module Usage
- Always use fully qualified collection names (FQCN)
- Prefer specific modules over command/shell
- Use `changed_when` and `failed_when` appropriately
- Register results when needed for conditions

## Roles

### Role Structure
```
roles/webserver/
├── defaults/main.yml      # Default variables (lowest precedence)
├── vars/main.yml         # Role variables (high precedence)
├── tasks/main.yml        # Main task list
├── handlers/main.yml     # Handler definitions
├── templates/            # Jinja2 templates
├── files/               # Static files
├── meta/main.yml        # Role metadata and dependencies
└── README.md           # Role documentation
```

### Role Best Practices
- One role = one purpose
- Make roles idempotent
- Use role dependencies sparingly
- Document role requirements and variables
- Tag role tasks for selective execution

## Security

### Vault Usage
```bash
# Create encrypted file
ansible-vault create group_vars/production/secrets.yml

# Encrypt existing file
ansible-vault encrypt host_vars/webserver/credentials.yml

# Edit encrypted file
ansible-vault edit secrets.yml

# Use multiple vault passwords
ansible-playbook site.yml --vault-id dev@prompt --vault-id prod@prompt
```

### Security Best Practices
- Never store passwords in plain text
- Use different vault passwords per environment
- Limit vault access to necessary personnel
- Rotate credentials regularly
- Use SSH keys instead of passwords
- Set appropriate file permissions

## Testing

### Linting
```bash
# Run ansible-lint
ansible-lint playbooks/

# Run yamllint
yamllint .

# Custom lint rules in .ansible-lint.yml
```

### Testing with Molecule
```bash
# Initialize role with Molecule
molecule init role myrole

# Test workflow
molecule create    # Create instances
molecule converge  # Run role
molecule verify    # Run tests
molecule destroy   # Clean up
```

## Performance

### Speed Optimizations
- Enable pipelining in ansible.cfg
- Use `strategy: free` for independent tasks
- Minimize fact gathering with `gather_facts: false`
- Use `serial` for rolling updates
- Cache facts when appropriate

### Connection Optimizations
```ini
[ssh_connection]
ssh_args = -o ControlMaster=auto -o ControlPersist=60s
pipelining = True
```

## Error Handling

### Graceful Failures
```yaml
- name: Attempt operation
  some_module:
    param: value
  register: result
  failed_when: false

- name: Handle failure
  debug:
    msg: "Operation failed but continuing"
  when: result is failed

# Use blocks for try-catch behavior
- block:
    - name: Try this task
      some_module:
        param: value
  rescue:
    - name: Do this on failure
      debug:
        msg: "Task failed, running recovery"
  always:
    - name: Always do this
      debug:
        msg: "Cleanup tasks"
```

## Documentation

### Playbook Documentation
```yaml
---
# Playbook: deploy_application.yml
# Purpose: Deploy the web application to production
# Author: Your Name
# Date: 2024-01-01
# 
# Requirements:
#   - Ansible 2.15+
#   - Access to production servers
#   - Vault password for secrets
#
# Usage:
#   ansible-playbook deploy_application.yml -i inventory/production
#
# Tags:
#   - config: Update configuration only
#   - deploy: Deploy application code
#   - restart: Restart services

- name: Deploy web application
  hosts: webservers
  ...
```

### Role Documentation (README.md)
- Role purpose and description
- Requirements (OS, Ansible version)
- Role variables with defaults
- Dependencies
- Example playbook
- License and author information

## Common Patterns

### Rolling Updates
```yaml
- name: Update web servers
  hosts: webservers
  serial: "25%"  # Update 25% at a time
  
  pre_tasks:
    - name: Take server out of load balancer
      # ... implementation
  
  tasks:
    - name: Deploy application
      # ... implementation
  
  post_tasks:
    - name: Return server to load balancer
      # ... implementation
```

### Conditional Includes
```yaml
- name: Include OS-specific tasks
  include_tasks: "{{ ansible_os_family }}.yml"

- name: Include environment configuration
  include_vars: "{{ env_name }}/config.yml"
```

## Debugging

### Debug Techniques
```yaml
# Print variable values
- debug:
    var: myvar
    verbosity: 2

# Print custom message
- debug:
    msg: "The value is {{ myvar }}"

# Use assertions
- assert:
    that:
      - nginx_port is defined
      - nginx_port is number
      - nginx_port > 0
      - nginx_port < 65536
```

### Verbose Output
```bash
# Increasing verbosity levels
ansible-playbook playbook.yml -v    # Basic
ansible-playbook playbook.yml -vv   # More
ansible-playbook playbook.yml -vvv  # Debug
ansible-playbook playbook.yml -vvvv # Connection debug
```