# setup-vault

**Description**: Configure Ansible Vault for secure secret management

**Category**: Security & Vault

**Source**: `claude_settings/ansible/shared/processes/vault-management.md`

## Usage

```bash
setup-vault
```

## Arguments

None - This is an interactive setup command

## Execution Instructions for Claude Code

When this command is run, Claude Code should:

1. **Check Current Setup**:
   - Look for existing vault password files
   - Check ansible.cfg for vault settings
   - Identify any already encrypted files

2. **Interactive Setup**:
   - Ask for vault password method:
     - Password file (recommended for automation)
     - Environment variable
     - Prompt for password
   - Ask if multiple vault IDs are needed

3. **Create Vault Configuration**:
   ```bash
   # Create secure vault password file
   openssl rand -base64 32 > .vault_pass
   chmod 600 .vault_pass
   
   # Add to .gitignore
   echo ".vault_pass" >> .gitignore
   echo "*.vault" >> .gitignore
   ```

4. **Update ansible.cfg**:
   ```ini
   [defaults]
   vault_password_file = .vault_pass
   # OR for multiple vaults
   vault_identity_list = dev@.vault_pass_dev, prod@.vault_pass_prod
   ```

5. **Create Initial Encrypted Files**:
   ```bash
   # Create group_vars vault files
   ansible-vault create group_vars/all/vault.yml
   ansible-vault create group_vars/production/vault.yml
   ansible-vault create group_vars/staging/vault.yml
   ```

6. **Set Up Best Practices**:
   - Create vault variable naming convention
   - Set up pre-commit hooks for vault files
   - Create documentation for team

## Vault Usage Examples

The command should also show examples:
```bash
# Encrypt existing file
ansible-vault encrypt secrets.yml

# Edit encrypted file
ansible-vault edit secrets.yml

# View encrypted file
ansible-vault view secrets.yml

# Rekey (change password)
ansible-vault rekey secrets.yml
```

## Security Best Practices

- Never commit vault passwords to Git
- Use different passwords for different environments
- Rotate vault passwords regularly
- Use vault IDs for multi-environment setups
- Implement proper file permissions (600)