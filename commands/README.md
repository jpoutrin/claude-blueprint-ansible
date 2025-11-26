# Ansible Commands Index

This directory contains command definitions for Claude Code to manage Ansible projects.

## Available Commands

### Architecture & Planning

- **[ansible-spec](ansible-spec.md)** - Design comprehensive Ansible solutions through guided discovery

### Setup & Configuration

- **[setup-ansible](setup-ansible.md)** `<project-name>` - Initialize Ansible project with best practices
- **[setup-vault](setup-vault.md)** - Configure Ansible Vault for secrets management
- **[setup-molecule](setup-molecule.md)** - Set up Molecule for testing roles
- **[configure-inventory](configure-inventory.md)** - Interactive inventory configuration wizard
- **[setup-galaxy](setup-galaxy.md)** - Configure Ansible Galaxy integration

### Development

- **[create-role](create-role.md)** `<role-name>` - Generate new Ansible role with standard structure
- **[create-playbook](create-playbook.md)** `<playbook-name>` - Create playbook from template
- **[create-collection](create-collection.md)** `<namespace>.<name>` - Initialize Ansible collection
- **[create-task-file](create-task-file.md)** `<role>/<task-name>` - Add task file to role
- **[create-handler](create-handler.md)** `<role>/<handler-name>` - Add handler to role

### Testing & Validation

- **[lint-ansible](lint-ansible.md)** `[path]` - Run ansible-lint on project
- **[test-role](test-role.md)** `<role-name>` - Run Molecule tests on role
- **[test-playbook](test-playbook.md)** `<playbook>` - Dry-run playbook with check mode
- **[validate-syntax](validate-syntax.md)** - Check all playbooks syntax
- **[test-inventory](test-inventory.md)** - Validate inventory and host connectivity

### Deployment

- **[deploy-playbook](deploy-playbook.md)** `<playbook> <inventory>` - Execute playbook with safety checks
- **[deploy-staging](deploy-staging.md)** `<playbook>` - Deploy to staging environment
- **[deploy-production](deploy-production.md)** `<playbook>` - Deploy to production with confirmations
- **[rollback](rollback.md)** - Rollback to previous deployment state
- **[deploy-check](deploy-check.md)** `<playbook>` - Pre-deployment validation

### Security & Vault

- **[encrypt-file](encrypt-file.md)** `<file>` - Encrypt file with Ansible Vault
- **[decrypt-file](decrypt-file.md)** `<file>` - Decrypt vault file for editing
- **[rotate-vault-password](rotate-vault-password.md)** - Rotate vault passwords safely
- **[audit-secrets](audit-secrets.md)** - Scan for unencrypted secrets
- **[manage-ssh-keys](manage-ssh-keys.md)** - SSH key management workflow

### Documentation

- **[generate-docs](generate-docs.md)** - Auto-generate role/playbook documentation
- **[create-readme](create-readme.md)** `<role>` - Generate README for role
- **[document-variables](document-variables.md)** - Document all variables in project
- **[create-runbook](create-runbook.md)** `<playbook>` - Generate operational runbook

### Utilities

- **[ansible-facts](ansible-facts.md)** `<host-pattern>` - Gather and display facts
- **[ansible-ping](ansible-ping.md)** - Test connectivity to all hosts
- **[show-dependencies](show-dependencies.md)** - Display role dependencies tree
- **[backup-configs](backup-configs.md)** - Backup current configurations
- **[clean-artifacts](clean-artifacts.md)** - Clean up temporary files

### Workflows

- **[quick-start](quick-start.md)** - Show Ansible quick start guide
- **[show-commands](show-commands.md)** - Display all available commands
- **[show-workflows](show-workflows.md)** - Display common workflow examples
- **[best-practices](best-practices.md)** - Show Ansible best practices guide

## Usage

To execute a command, ask Claude Code to:
1. Read the command file (e.g., `claude_settings/ansible/commands/setup-ansible.md`)
2. Follow the execution instructions in the file
3. Use the source file referenced for the full process

## Command Types

- **Setup Commands**: Initialize projects and configure tools
- **Creation Commands**: Generate roles, playbooks, and other artifacts
- **Testing Commands**: Validate and test Ansible code
- **Deployment Commands**: Safely deploy changes with checks
- **Security Commands**: Manage secrets and security configurations
- **Utility Commands**: Helper tools for daily operations

## Notes

- Commands with arguments show `<argument-name>` in their usage
- Interactive commands will guide you through configuration
- Deployment commands include safety checks and confirmations
- All commands follow Ansible best practices