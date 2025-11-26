# create-role

**Description**: Generate a new Ansible role with standard directory structure

**Category**: Development

**Source**: `claude_settings/ansible/shared/processes/role-development.md`

## Usage

```bash
create-role <role-name>
```

## Arguments

- `<role-name>`: Required - Name of the Ansible role to create

## Execution Instructions for Claude Code

When this command is run, Claude Code should:

1. Validate the role name follows Ansible naming conventions (lowercase, alphanumeric, underscores)
2. Create the role directory structure:
   ```
   roles/<role-name>/
   ├── defaults/
   │   └── main.yml
   ├── files/
   ├── handlers/
   │   └── main.yml
   ├── meta/
   │   └── main.yml
   ├── tasks/
   │   └── main.yml
   ├── templates/
   ├── tests/
   │   ├── inventory
   │   └── test.yml
   ├── vars/
   │   └── main.yml
   └── README.md
   ```
3. Generate appropriate content for each file
4. Optionally initialize Molecule testing if requested
5. Update the project's requirements.yml if it exists

## Interactive Options

Claude Code should ask:
1. Role description (for meta/main.yml)
2. Minimum Ansible version required
3. Supported platforms (OS/distributions)
4. Role dependencies
5. Whether to set up Molecule testing

## Files Created

- **defaults/main.yml**: Default variables for the role
- **handlers/main.yml**: Handlers for service management
- **meta/main.yml**: Role metadata and dependencies
- **tasks/main.yml**: Main task list
- **README.md**: Role documentation
- **molecule/** (optional): Testing configuration

## Example

```bash
create-role webserver
```

This creates a new role called "webserver" with all standard directories and files.