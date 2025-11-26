# Claude Blueprint - Ansible

Ansible automation blueprints for Claude, including playbook development, role creation, and infrastructure as code workflows.

## Contents

### Commands

Claude slash commands for Ansible workflows:
- Ansible setup and configuration
- Role creation
- Playbook deployment
- Ansible Vault management
- Linting and validation
- Specification creation

### Shared Files

#### Architecture
- Ansible best practices
- Role and playbook structure guidelines

#### Processes
- Ansible setup workflow
- Specification creation process

#### Templates
- Role templates
- Specification templates

#### Quick Reference
- Common Ansible commands
- Quick reference guides

## Installation

Install this blueprint using claude-blueprint-cli:

```bash
# Install latest version
claude-blueprint install ansible

# Install specific version
claude-blueprint install ansible --version v1.0.0
```

## Usage

After installation, you can use the commands in your project:

```bash
# Example: Set up Ansible
/setup-ansible

# Example: Create a new role
/create-role

# Example: Deploy a playbook
/deploy-playbook
```

## Structure

```
claude-blueprint-ansible/
├── blueprint.json          # Blueprint manifest
├── commands/              # Claude slash commands
│   ├── setup-ansible.md
│   ├── create-role.md
│   └── ...
└── shared/                # Shared configuration files
    ├── architecture/
    ├── processes/
    ├── templates/
    └── quick-reference/
```

## Version History

### v1.0.0 (Initial Release)
- Ansible setup and configuration
- Role creation workflows
- Playbook deployment commands
- Vault management
- Best practices and templates

## License

MIT License - see LICENSE file for details.

## Related Projects

- [claude-blueprint-cli](https://github.com/jpoutrin/claude-blueprint-cli) - CLI tool for managing blueprints
- [claude-blueprint](https://github.com/jpoutrin/claude-blueprint) - JavaScript version
