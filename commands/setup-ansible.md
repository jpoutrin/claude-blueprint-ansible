# setup-ansible

**Description**: Initialize a new Ansible project with best practices structure

**Category**: Setup & Configuration

**Source**: `claude_settings/ansible/shared/processes/ansible-setup.md`

## Usage

```bash
setup-ansible <project-name>
```

## Arguments

- `<project-name>`: Required - Name of your Ansible project

## Execution Instructions for Claude Code

When this command is run, Claude Code should:

1. Read the source file at `claude_settings/ansible/shared/processes/ansible-setup.md`
2. Replace `my-ansible-project` with the provided `<project-name>` in all commands
3. Execute the setup steps in sequence:
   - Create project directory structure
   - Initialize Git repository
   - Set up ansible.cfg with best practices
   - Create requirements files
   - Set up inventory structure
   - Configure ansible-lint and yamllint
   - Create initial playbooks
   - Set up Molecule for testing (optional)
4. Handle any errors appropriately
5. Provide clear feedback on each step

## Quick Setup Steps

1. **Directory Structure**: Creates standard Ansible project layout
2. **Configuration**: Sets up ansible.cfg with recommended settings
3. **Inventory**: Creates dynamic inventory structure
4. **Roles**: Sets up roles directory with example structure
5. **Testing**: Configures ansible-lint and optional Molecule
6. **Documentation**: Creates README and documentation templates
7. **Version Control**: Initializes Git with proper .gitignore

## Source Content Location

The full process documentation can be found at:
`claude_settings/ansible/shared/processes/ansible-setup.md`

## Example

```bash
setup-ansible infrastructure
```

This creates a new Ansible project called "infrastructure" with production-ready structure.