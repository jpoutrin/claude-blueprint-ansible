# lint-ansible

**Description**: Run ansible-lint to check for best practices and potential issues

**Category**: Testing & Validation

**Source**: Direct command execution

## Usage

```bash
lint-ansible [path]
```

## Arguments

- `[path]`: Optional - Path to lint (defaults to current directory)

## Execution Instructions for Claude Code

When this command is run, Claude Code should:

1. Check if ansible-lint is installed
2. If not installed, offer to install it:
   ```bash
   pip install ansible-lint yamllint
   ```
3. Run ansible-lint with appropriate options:
   ```bash
   # Basic linting
   ansible-lint
   
   # With specific path
   ansible-lint playbooks/
   
   # With custom config
   ansible-lint -c .ansible-lint.yml
   
   # Verbose output
   ansible-lint -v
   
   # Also run yamllint
   yamllint .
   ```
4. Parse and present the results clearly
5. Offer to fix auto-fixable issues

## Configuration

Create `.ansible-lint.yml`:
```yaml
---
exclude_paths:
  - .cache/
  - .github/
warn_list:
  - experimental
  - role-name
  - yaml[line-length]
skip_list:
  - yaml[line-length]  # Lines can be longer than 80 chars
```

Create `.yamllint.yml`:
```yaml
---
extends: default
rules:
  line-length:
    max: 120
    level: warning
  truthy:
    allowed-values: ['true', 'false', 'yes', 'no']
```

## Common Issues Detected

- Missing `name:` for tasks
- Deprecated modules
- Incorrect YAML formatting
- Hard-coded passwords
- Missing tags
- Shell/command module usage without proper conditions

## Example Output Format

```
Checking 15 files...
playbooks/deploy.yml:23: [E502] All tasks should be named
roles/webserver/tasks/main.yml:45: [W503] Tasks that run 'command' should use 'changed_when'
```