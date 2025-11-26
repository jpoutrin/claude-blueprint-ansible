# deploy-playbook

**Description**: Execute an Ansible playbook with safety checks and confirmation

**Category**: Deployment

**Source**: `claude_settings/ansible/shared/processes/safe-deployment.md`

## Usage

```bash
deploy-playbook <playbook> <inventory>
```

## Arguments

- `<playbook>`: Required - Path to the playbook to execute
- `<inventory>`: Required - Inventory file or directory to use

## Execution Instructions for Claude Code

When this command is run, Claude Code should:

1. **Pre-deployment Checks**:
   - Verify playbook exists and is valid YAML
   - Check inventory file/directory exists
   - Run ansible-lint on the playbook
   - Test connectivity with ansible-ping

2. **Safety Checks**:
   - Run playbook in check mode first:
     ```bash
     ansible-playbook -i <inventory> <playbook> --check --diff
     ```
   - Review changes that will be made
   - Ask for confirmation to proceed

3. **Deployment Options** (interactive):
   - Ask for limit hosts: `--limit`
   - Ask for specific tags: `--tags`
   - Ask for extra variables: `--extra-vars`
   - Ask if should create backup: `--backup`
   - Ask for verbosity level

4. **Execute Deployment**:
   ```bash
   ansible-playbook -i <inventory> <playbook> \
     --diff \
     --limit <hosts> \
     --tags <tags> \
     --extra-vars <vars> \
     -v
   ```

5. **Post-deployment**:
   - Save deployment log
   - Create deployment record with timestamp
   - Run post-deployment verification if defined

## Safety Features

- Always runs in check mode first
- Shows diff of changes before applying
- Requires explicit confirmation
- Creates deployment logs
- Supports limiting to specific hosts
- Can create backups before changes

## Example

```bash
deploy-playbook playbooks/webserver.yml inventory/production
```

This will:
1. Validate the playbook
2. Run in check mode to show what will change
3. Ask for confirmation
4. Execute the deployment with full logging