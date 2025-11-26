# ansible-spec

**Description**: Design comprehensive Ansible solutions through guided discovery and specification

**Category**: Architecture & Planning

**Source**: `claude_settings/ansible/shared/processes/ansible-specification.md`

## Usage

```bash
ansible-spec
```

## Arguments

None - This is an interactive specification process

## Execution Instructions for Claude Code

When this command is run, Claude Code should:

1. Start an iterative discovery process covering 5 required areas
2. Allow review and editing after each section
3. Use best practices with guided decisions for key choices
4. Generate comprehensive specification outputs
5. Save all outputs to a project specification directory

## Discovery Process

The command guides through these required sections:

### 1. Infrastructure Requirements
- Target environment (cloud/on-premise/hybrid)
- Server specifications and count
- Network architecture
- Storage requirements
- Geographic distribution

### 2. Application Stack
- Services to deploy
- Databases and data stores
- Message queues/caching
- Dependencies and integrations
- API requirements

### 3. Security & Compliance
- Access control requirements
- Secret management needs
- Compliance standards
- Network security
- Audit requirements

### 4. High Availability & Performance
- Uptime requirements
- Load expectations
- Scaling strategy
- Failover approach
- Performance targets

### 5. Operations & Monitoring
- Logging strategy
- Monitoring requirements
- Backup approach
- Maintenance windows
- Alerting needs

## Outputs Generated

1. **Technical Specification** (`ansible-spec-output/spec.md`)
   - Complete requirements documentation
   - Architectural decisions and rationale
   - Technology choices

2. **Ansible Structure** (`ansible-spec-output/structure.md`)
   - Recommended directory layout
   - Role breakdown
   - Playbook organization
   - Variable hierarchy

3. **Architecture Diagram** (`ansible-spec-output/architecture.txt`)
   - ASCII art component diagram
   - Data flow visualization
   - Network topology

4. **Validation Checklist** (`ansible-spec-output/validation.md`)
   - Test scenarios
   - Acceptance criteria
   - Performance benchmarks

5. **Risk Register** (`ansible-spec-output/risks.md`)
   - Identified risks
   - Impact assessment
   - Mitigation strategies

## Interactive Features

- **Section Review**: After each section, option to review and modify answers
- **Best Practice Guidance**: Recommendations based on Ansible patterns
- **Decision Trees**: Guided choices for architecture decisions
- **Validation**: Code snippets and test commands for key patterns
- **Scope Management**: Clear boundaries with future considerations tracking

## Example Flow

```
Starting Ansible Solution Specification...

=== INFRASTRUCTURE REQUIREMENTS ===
1. What is your target environment?
   a) AWS Cloud
   b) On-premise
   c) Hybrid cloud
   > a

2. What AWS regions will you deploy to?
   > us-east-1, us-west-2

[... continues through all sections ...]

=== REVIEW INFRASTRUCTURE SECTION ===
Would you like to modify any answers? (y/n)
> n

[... proceeds to next section ...]
```

## Source Content Location

The full specification process and templates are at:
`claude_settings/ansible/shared/processes/ansible-specification.md`