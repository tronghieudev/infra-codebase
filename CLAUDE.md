# CLAUDE.md - AI Assistant Guide for infra-codebase

**Repository**: tronghieudev/infra-codebase
**Last Updated**: 2025-11-18
**Purpose**: Infrastructure as Code (IaC) repository for managing cloud infrastructure and deployment configurations

---

## Table of Contents

1. [Repository Overview](#repository-overview)
2. [Current State](#current-state)
3. [Codebase Structure](#codebase-structure)
4. [Development Workflows](#development-workflows)
5. [Key Conventions](#key-conventions)
6. [Infrastructure Best Practices](#infrastructure-best-practices)
7. [Common Tasks](#common-tasks)
8. [Testing and Validation](#testing-and-validation)
9. [Security Considerations](#security-considerations)
10. [Troubleshooting](#troubleshooting)

---

## Repository Overview

This repository serves as the central location for infrastructure as code (IaC) definitions, deployment configurations, and related automation scripts.

### Purpose
- Define and version control infrastructure resources
- Enable reproducible infrastructure deployments
- Provide disaster recovery capabilities through code-based infrastructure definitions
- Facilitate collaboration on infrastructure changes through code review processes

### Technology Stack (Expected)
Based on the repository name and common IaC patterns, this repository may contain:
- **IaC Tools**: Terraform, Pulumi, CloudFormation, or similar
- **Configuration Management**: Ansible, Chef, Puppet, or similar
- **Container Orchestration**: Kubernetes manifests, Helm charts, or Docker Compose files
- **CI/CD**: GitHub Actions, GitLab CI, Jenkins pipelines, or similar
- **Scripting**: Shell scripts, Python, or other automation languages

---

## Current State

**Status**: Initial/Minimal Setup
**Branch**: `claude/claude-md-mi489yvz0d1nbb3y-01KUgB3Y6psNeX6QGxeRLZZH`
**Last Commit**: `49bd00b Create README.md`

### Current Files
```
/home/user/infra-codebase/
├── .git/              # Git repository metadata
├── README.md          # Basic repository readme (contains "Hello")
└── CLAUDE.md          # This file - AI assistant guide
```

### Repository Status
- Clean working directory
- Minimal initial setup
- Ready for infrastructure code to be added

---

## Codebase Structure

### Recommended Directory Structure

As this repository grows, consider organizing it with the following structure:

```
infra-codebase/
├── README.md                    # Main repository documentation
├── CLAUDE.md                    # This file - AI assistant guide
├── .gitignore                   # Git ignore patterns
│
├── terraform/                   # Terraform infrastructure definitions
│   ├── environments/            # Environment-specific configurations
│   │   ├── dev/
│   │   ├── staging/
│   │   └── production/
│   ├── modules/                 # Reusable Terraform modules
│   └── global/                  # Global/shared resources
│
├── kubernetes/                  # Kubernetes manifests
│   ├── base/                    # Base configurations
│   ├── overlays/                # Kustomize overlays for environments
│   │   ├── dev/
│   │   ├── staging/
│   │   └── production/
│   └── helm/                    # Helm charts
│
├── ansible/                     # Ansible playbooks and roles
│   ├── playbooks/
│   ├── roles/
│   └── inventory/
│
├── scripts/                     # Utility and automation scripts
│   ├── deployment/
│   ├── maintenance/
│   └── backup/
│
├── docker/                      # Dockerfile and docker-compose files
│   └── compose/
│
├── ci-cd/                       # CI/CD pipeline configurations
│   ├── github-actions/
│   ├── gitlab-ci/
│   └── jenkins/
│
├── docs/                        # Additional documentation
│   ├── architecture/
│   ├── runbooks/
│   └── diagrams/
│
└── tests/                       # Infrastructure tests
    ├── unit/
    ├── integration/
    └── compliance/
```

### File Naming Conventions

1. **Infrastructure Files**:
   - Terraform: `main.tf`, `variables.tf`, `outputs.tf`, `versions.tf`
   - Use descriptive names for resource-specific files: `vpc.tf`, `databases.tf`, `security-groups.tf`

2. **Configuration Files**:
   - Use lowercase with hyphens: `config-production.yaml`, `values-staging.yaml`
   - Include environment in filename when applicable

3. **Scripts**:
   - Use lowercase with hyphens: `deploy-app.sh`, `backup-database.py`
   - Make scripts executable: `chmod +x script-name.sh`
   - Include shebang line: `#!/bin/bash` or `#!/usr/bin/env python3`

4. **Documentation**:
   - Use UPPERCASE for main docs: `README.md`, `CLAUDE.md`, `CONTRIBUTING.md`
   - Use lowercase for specific docs: `architecture.md`, `deployment-guide.md`

---

## Development Workflows

### Git Workflow

#### Branch Strategy
- **Main/Master Branch**: Production-ready infrastructure code
- **Feature Branches**: Named as `feature/description` or `claude/session-id`
- **Environment Branches** (optional): `env/dev`, `env/staging`, `env/production`

#### Working with Feature Branches

1. **Create/Switch to Feature Branch**:
   ```bash
   git checkout -b feature/add-vpc-configuration
   ```

2. **Make Changes and Commit**:
   ```bash
   git add .
   git commit -m "Add VPC configuration for production environment"
   ```

3. **Push Changes**:
   ```bash
   git push -u origin feature/add-vpc-configuration
   ```

### Commit Message Guidelines

Use conventional commits format for infrastructure changes:

```
<type>(<scope>): <subject>

<body>

<footer>
```

**Types**:
- `feat`: New infrastructure resource or capability
- `fix`: Bug fix or correction to infrastructure
- `docs`: Documentation changes
- `refactor`: Code refactoring without functionality change
- `test`: Adding or updating tests
- `chore`: Maintenance tasks, dependency updates
- `security`: Security-related changes

**Examples**:
```
feat(terraform): add RDS PostgreSQL database for production

- Configure multi-AZ deployment
- Enable automated backups with 7-day retention
- Set up parameter group for PostgreSQL 15
- Configure security group for database access

Relates-to: INFRA-123
```

```
fix(kubernetes): correct resource limits for api deployment

- Increase memory limit from 512Mi to 1Gi
- Adjust CPU request to 500m
- Resolves OOMKilled errors in production

Fixes: INFRA-456
```

### Code Review Process

When creating pull requests for infrastructure changes:

1. **Provide Clear Description**: Explain what infrastructure is being changed and why
2. **Include Plan Output**: For Terraform, include `terraform plan` output
3. **Document Impact**: Describe potential downtime or service impact
4. **Reference Issues**: Link to related issues or tickets
5. **Add Validation Results**: Include test results, linting output, security scans

---

## Key Conventions

### 1. Environment Management

**Environment Separation**:
- Keep environment configurations strictly separated
- Use variables/parameters for environment-specific values
- Never hardcode credentials or sensitive data

**Environment Naming**:
- `dev` or `development`: Development environment
- `staging` or `stage`: Staging/pre-production environment
- `prod` or `production`: Production environment
- `sandbox`: Experimental/testing environment

### 2. Variable and Parameter Naming

**Terraform**:
```hcl
# Use snake_case for variables
variable "vpc_cidr_block" {
  description = "CIDR block for the VPC"
  type        = string
  default     = "10.0.0.0/16"
}

# Use descriptive resource names with environment
resource "aws_vpc" "main" {
  cidr_block = var.vpc_cidr_block

  tags = {
    Name        = "${var.environment}-vpc"
    Environment = var.environment
    ManagedBy   = "terraform"
  }
}
```

**Kubernetes**:
```yaml
# Use kebab-case for resource names
apiVersion: v1
kind: Service
metadata:
  name: api-service
  namespace: production
  labels:
    app: api
    environment: production
```

### 3. Tagging and Labeling

**Required Tags/Labels** (when applicable):
```
Name: Descriptive resource name
Environment: dev/staging/production
ManagedBy: terraform/ansible/helm
Project: project-name
Owner: team-name
CostCenter: cost-center-id
CreatedDate: YYYY-MM-DD
```

### 4. Documentation Standards

**Inline Documentation**:
- Add comments for complex logic or non-obvious configurations
- Document why decisions were made, not just what the code does
- Include links to relevant documentation or RFCs

**README Files**:
- Every major directory should have a README.md explaining its purpose
- Include prerequisites, usage instructions, and examples
- Document any manual steps required

### 5. Secrets Management

**Never commit secrets to the repository**:
- Use environment variables
- Use secret management tools: AWS Secrets Manager, HashiCorp Vault, etc.
- Use `.gitignore` to exclude sensitive files
- Use placeholder values in example configurations

**Example .gitignore for Infrastructure**:
```gitignore
# Terraform
*.tfstate
*.tfstate.*
.terraform/
.terraform.lock.hcl
terraform.tfvars
override.tf
override.tf.json

# Ansible
*.retry
vault-password.txt

# Environment files
.env
.env.local
*.secret
secrets/

# IDE
.idea/
.vscode/
*.swp
*.swo

# OS
.DS_Store
Thumbs.db

# Credentials
credentials.json
*.pem
*.key
*.cert
```

---

## Infrastructure Best Practices

### 1. State Management (Terraform)

**Remote State**:
- Always use remote state for team collaboration
- Enable state locking to prevent concurrent modifications
- Configure state backups and versioning

**State Security**:
- Store state files securely (encrypted S3 bucket, Terraform Cloud, etc.)
- Restrict access to state files (contain sensitive data)
- Never commit state files to version control

### 2. Resource Organization

**Modularity**:
- Create reusable modules for common patterns
- Keep modules focused on single responsibility
- Version your modules for stability

**Dependency Management**:
- Explicitly define dependencies between resources
- Use data sources to reference existing resources
- Avoid circular dependencies

### 3. Idempotency

- Ensure infrastructure code can be run multiple times safely
- Use declarative syntax where possible
- Test apply/re-apply scenarios

### 4. Change Management

**Planning Before Applying**:
```bash
# Always review plan before applying
terraform plan -out=tfplan
# Review the plan
terraform show tfplan
# Apply only after review
terraform apply tfplan
```

**Gradual Rollouts**:
- Test changes in dev/staging before production
- Use feature flags for gradual enabling
- Plan for rollback procedures

### 5. High Availability and Disaster Recovery

- Design for multi-AZ/multi-region when appropriate
- Implement automated backups
- Document and test disaster recovery procedures
- Use infrastructure code for reproducible recovery

---

## Common Tasks

### Task 1: Adding a New Infrastructure Resource

```bash
# 1. Create/switch to feature branch
git checkout -b feature/add-new-resource

# 2. Add the resource definition (e.g., in Terraform)
# Edit appropriate .tf file

# 3. Validate syntax
terraform fmt
terraform validate

# 4. Review planned changes
terraform plan

# 5. Commit changes
git add .
git commit -m "feat(terraform): add new resource configuration"

# 6. Push and create PR
git push -u origin feature/add-new-resource
```

### Task 2: Updating Environment Configuration

```bash
# 1. Navigate to environment directory
cd terraform/environments/production

# 2. Update configuration files
# Edit variables.tf or terraform.tfvars

# 3. Test with plan
terraform plan

# 4. Commit with clear description
git commit -am "chore(production): update configuration parameters"
```

### Task 3: Validating Infrastructure Changes

```bash
# Format code
terraform fmt -recursive

# Validate syntax
terraform validate

# Check for security issues (using tfsec as example)
tfsec .

# Check for compliance (using checkov as example)
checkov -d .

# Static analysis
terraform-docs markdown . > README.md
```

### Task 4: Rolling Back Changes

```bash
# For Terraform
terraform state list  # List resources
terraform state show <resource>  # Show resource details

# If needed, revert to previous configuration
git revert <commit-hash>
terraform plan
terraform apply

# Or use specific state version if using remote state
```

---

## Testing and Validation

### Pre-Commit Checks

Before committing infrastructure code, run:

1. **Formatting**: `terraform fmt -recursive` or language-specific formatter
2. **Validation**: `terraform validate` or equivalent
3. **Linting**: Use tools like `tflint`, `ansible-lint`, `kubeval`
4. **Security Scanning**: `tfsec`, `checkov`, `trivy`
5. **Documentation**: Update README files, inline comments

### Testing Levels

**1. Syntax Testing**:
- Validate YAML/HCL/JSON syntax
- Check for structural errors
- Verify references and dependencies

**2. Unit Testing**:
- Test individual modules in isolation
- Use frameworks like Terratest, Kitchen-Terraform
- Mock external dependencies

**3. Integration Testing**:
- Deploy to test environment
- Verify resource creation and configuration
- Test inter-resource communication

**4. Compliance Testing**:
- Verify adherence to security policies
- Check cost optimization rules
- Validate tagging compliance

**5. Smoke Testing**:
- After deployment, verify critical functionality
- Check health endpoints
- Validate monitoring and alerting

---

## Security Considerations

### 1. Least Privilege Principle

- Grant minimum necessary permissions
- Use IAM roles and policies appropriately
- Regularly audit and rotate credentials
- Implement principle of least privilege for service accounts

### 2. Network Security

- Use private subnets for sensitive resources
- Implement network segmentation
- Configure security groups/firewalls restrictively
- Enable VPC flow logs for monitoring

### 3. Encryption

- Encrypt data at rest (storage, databases)
- Encrypt data in transit (TLS/SSL)
- Use managed encryption services where available
- Rotate encryption keys regularly

### 4. Secrets Management

- Never hardcode secrets
- Use secret management services
- Rotate secrets regularly
- Audit secret access

### 5. Compliance and Auditing

- Enable audit logging (CloudTrail, audit logs)
- Implement compliance checks in CI/CD
- Regular security assessments
- Document compliance requirements

### 6. Vulnerability Management

- Keep dependencies updated
- Scan container images for vulnerabilities
- Monitor security advisories
- Implement automated patching where possible

---

## Troubleshooting

### Common Issues and Solutions

#### Issue: Terraform State Lock Error

```
Error: Error locking state: Error acquiring the state lock
```

**Solution**:
```bash
# View lock info
terraform force-unlock <lock-id>

# Only use if you're certain no other process is running
```

#### Issue: Resource Already Exists

```
Error: Resource already exists
```

**Solution**:
```bash
# Import existing resource
terraform import <resource_type>.<resource_name> <resource_id>

# Or remove from state if no longer needed
terraform state rm <resource_type>.<resource_name>
```

#### Issue: Authentication/Credentials Error

**Solution**:
- Verify credentials are properly configured
- Check environment variables
- Ensure IAM permissions are sufficient
- Verify credential expiration

#### Issue: Kubernetes Deployment Fails

**Solution**:
```bash
# Check pod status
kubectl get pods -n <namespace>

# View pod logs
kubectl logs <pod-name> -n <namespace>

# Describe pod for events
kubectl describe pod <pod-name> -n <namespace>

# Check resource limits
kubectl top pods -n <namespace>
```

### Debugging Tips

1. **Enable Verbose Logging**:
   - Terraform: `TF_LOG=DEBUG terraform apply`
   - Ansible: `ansible-playbook -vvv playbook.yml`
   - kubectl: `kubectl --v=8 <command>`

2. **Validate Before Applying**:
   - Always run plan/dry-run first
   - Review changes carefully
   - Test in non-production first

3. **Check Dependencies**:
   - Verify version compatibility
   - Check network connectivity
   - Ensure prerequisites are met

4. **Review Documentation**:
   - Check provider documentation
   - Review resource-specific requirements
   - Search for similar issues in community forums

---

## AI Assistant Guidelines

### When Working on This Repository

1. **Always Check Current State First**:
   - Read existing files before modifying
   - Check git status and branch
   - Understand the current infrastructure setup

2. **Follow Established Patterns**:
   - Match existing code style and structure
   - Use the same tools and frameworks already in use
   - Maintain consistency with existing naming conventions

3. **Be Explicit About Changes**:
   - Clearly explain what infrastructure will be created/modified
   - Highlight potential impacts or risks
   - Document assumptions made

4. **Security First**:
   - Never commit secrets or credentials
   - Follow security best practices
   - Highlight security implications of changes

5. **Validate Before Committing**:
   - Run formatting and validation tools
   - Test in appropriate environment
   - Include validation results in commits

6. **Document Everything**:
   - Update relevant README files
   - Add inline comments for complex logic
   - Keep this CLAUDE.md file updated

7. **Ask for Clarification**:
   - When infrastructure impact is unclear
   - When multiple valid approaches exist
   - When security/compliance requirements are ambiguous

### Repository-Specific Context

- **Repository Name**: infra-codebase
- **Owner**: tronghieudev
- **Current Branch**: `claude/claude-md-mi489yvz0d1nbb3y-01KUgB3Y6psNeX6QGxeRLZZH`
- **Main Purpose**: Infrastructure as Code management
- **Current Status**: Initial setup phase

### Workflow for AI Assistants

1. **Understand the Request**: Clarify what infrastructure change is needed
2. **Analyze Impact**: Determine what will be affected
3. **Plan Approach**: Outline steps to implement the change
4. **Implement**: Make changes following conventions
5. **Validate**: Test and verify changes
6. **Document**: Update documentation as needed
7. **Commit**: Create clear commit messages
8. **Push**: Push to appropriate branch

---

## Additional Resources

### Recommended Tools

- **Terraform**: Infrastructure as Code for cloud resources
- **Ansible**: Configuration management and orchestration
- **kubectl/helm**: Kubernetes management
- **tfsec**: Terraform security scanner
- **checkov**: Infrastructure as Code security scanner
- **terraform-docs**: Documentation generator for Terraform
- **pre-commit**: Git pre-commit hook framework

### Documentation Links

- [Terraform Best Practices](https://www.terraform-best-practices.com/)
- [Kubernetes Best Practices](https://kubernetes.io/docs/concepts/configuration/overview/)
- [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/)
- [Google Cloud Architecture Framework](https://cloud.google.com/architecture/framework)
- [Azure Cloud Adoption Framework](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/)

### Community Resources

- Infrastructure as Code communities and forums
- Provider-specific documentation and examples
- Security and compliance guidelines for your industry

---

## Changelog

### 2025-11-18
- Initial CLAUDE.md creation
- Documented repository structure and conventions
- Added development workflows and best practices
- Included security considerations and troubleshooting guide

---

## Contributing

When contributing to this repository:

1. Follow the conventions outlined in this document
2. Test changes thoroughly in non-production environments
3. Update documentation as needed
4. Submit clear pull requests with detailed descriptions
5. Respond to code review feedback promptly

---

**Note**: This is a living document. As the repository evolves, update this guide to reflect new patterns, tools, and conventions adopted by the team.
