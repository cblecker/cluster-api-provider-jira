# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with this Kubernetes Cluster API provider repository.

## Project Overview

**cluster-api-provider-jira** is a Kubernetes Cluster API provider that uses JIRA/humans as the infrastructure provider. Built with Kubebuilder following standard controller-runtime patterns.

> **⚠️ Important**: This is not intended for production use ("Seriously don't use this") - it's a proof-of-concept/template project.

**Current Status**: Minimal/skeletal implementation with comprehensive scaffolding (metrics, RBAC, security) but lacks actual JIRA provider logic.

## Quick Reference

### Essential Commands
```bash
# Development
make build              # Build manager binary
make run                # Run controller locally
make test               # Run unit tests
make test-e2e           # Run e2e tests on Kind

# Code Quality
make fmt vet lint       # Format, vet, and lint
make generate manifests # Generate code and manifests

# Docker & Deployment
make docker-build IMG=<image>  # Build container
make install deploy           # Install CRDs and deploy
```

### Key Directories
- **cmd/main.go**: Controller manager entry point
- **config/**: Kustomize configurations (RBAC, CRDs, deployment)
- **test/**: Test suites and utilities

## Development Environment

### Prerequisites
- Go v1.24.0+, Docker v17.03+, kubectl v1.11.3+
- Kubernetes v1.11.3+ cluster access
- Kind (for e2e testing)

### Technology Stack
- **Go**: 1.24.5
- **Controller Runtime**: v0.22.1
- **Testing**: Ginkgo v2.22.0 + Gomega v1.36.1
- **Build Tools**: Kustomize v5.7.1, Controller Tools v0.19.0

## Cluster API Provider Development

### Core Documentation
- **Primary Reference**: https://cluster-api.sigs.k8s.io/developer/providers/overview
- **Security Guidelines**: https://cluster-api.sigs.k8s.io/developer/providers/security-guidelines
- **Best Practices**: https://cluster-api.sigs.k8s.io/developer/providers/best-practices

## Security Guidelines

Following the official Cluster API security guidelines, infrastructure providers must implement comprehensive security measures across all aspects of the system.

### Credentials Management & Access Control

#### Least Privilege Principle
- **Minimal Permissions**: Use the absolute minimum credentials required for each operation
- **Scoped Access**: Credentials should be scoped to specific resources and operations only
- **Regular Auditing**: Periodically review and validate credential permissions
- **Credential Isolation**: Separate credentials for different environments (dev, staging, prod)

#### Short-lived Credentials
- **Automatic Renewal**: Implement mechanisms for automatic credential renewal before expiration
- **Token Rotation**: Regular rotation of access tokens and API keys
- **Expiration Policies**: Set appropriate expiration timeframes based on risk assessment
- **Failure Handling**: Graceful handling of credential renewal failures

#### Access Restrictions
- **Namespace Security**: Restrict Cluster API controller namespace access to cloud administrators only
- **RBAC Implementation**: Use Kubernetes RBAC to enforce fine-grained access controls
- **Administrative Boundaries**: Clear separation between regular users and cloud infrastructure administrators
- **Audit Logging**: Comprehensive logging of all access attempts and administrative actions

#### Authentication Requirements
- **Two-Factor Authentication (2FA)**: Mandatory 2FA for all GitHub maintainer accounts
- **Second Pair of Eyes**: Implement "second pair of eyes" principle for all privileged operations
- **Strong Authentication**: Use strong authentication methods for VM access and troubleshooting
- **Session Management**: Proper session timeout and management for all authenticated sessions

### Resource Protection & Management

#### Rate Limiting & Quotas
- **Cloud Resource Limits**: Implement rate limits for cloud resource creation and deletion operations
- **API Throttling**: Apply appropriate throttling to prevent API abuse
- **Resource Quotas**: Set quotas to prevent resource sprawl and unexpected costs
- **Monitoring & Alerting**: Monitor resource usage patterns and alert on anomalies

#### Automatic Cleanup & Garbage Collection
- **Resource Lifecycle**: Implement automatic deletion or marking of unused resources for garbage collection
- **Configurable Timeouts**: Provide configurable cleanup timeouts for different resource types
- **Orphaned Resource Detection**: Detect and clean up orphaned or abandoned resources
- **Cleanup Verification**: Verify successful cleanup and handle cleanup failures appropriately

#### Bootstrap Data Security
- **Metadata Protection**: Secure machine bootstrap data and metadata during transmission and storage
- **Immediate Cleanup**: Clean up bootstrap data immediately after successful machine initialization
- **Encryption in Transit**: Encrypt bootstrap data during transmission to target machines
- **Temporary Storage**: Minimize storage duration of sensitive bootstrap information

### Sensitive Data Protection

#### Secrets & Credentials Lifecycle
- **End-to-End Protection**: Protect cluster secrets and credentials throughout their entire lifecycle
- **Encryption at Rest**: Encrypt sensitive data when stored
- **Secure Transmission**: Use TLS/encrypted channels for all sensitive data transmission
- **Memory Protection**: Clear sensitive data from memory after use

#### Data Classification & Handling
- **Classification Scheme**: Implement data classification to identify sensitive information
- **Handling Procedures**: Establish procedures for handling different classes of sensitive data
- **Retention Policies**: Define and enforce data retention and deletion policies
- **Compliance**: Ensure compliance with relevant data protection regulations

### Infrastructure Security

#### Secure VM Access
- **Controlled Access**: Implement secure, controlled access mechanisms for VM troubleshooting
- **Bastion Hosts**: Use bastion hosts or similar controlled access points where appropriate
- **Access Logging**: Log all VM access attempts and activities for audit purposes
- **Privilege Escalation**: Controlled and logged privilege escalation procedures

#### Manual Operations Control
- **Authorization Required**: Require explicit authorization for manual cloud infrastructure operations
- **Change Management**: Implement change management processes for manual interventions
- **Documentation**: Document all manual operations and their business justification
- **Rollback Procedures**: Establish rollback procedures for manual operations

### Security Monitoring & Response

#### Continuous Monitoring
- **Security Metrics**: Implement security-focused metrics and monitoring
- **Anomaly Detection**: Monitor for unusual patterns in resource usage or access
- **Alerting Systems**: Set up alerting for security-relevant events
- **Regular Assessment**: Conduct regular security assessments and penetration testing

#### Incident Response
- **Response Procedures**: Establish clear incident response procedures for security events
- **Communication Plans**: Define communication plans for security incidents
- **Forensic Capabilities**: Maintain capabilities for security forensics and investigation
- **Recovery Procedures**: Document and test security incident recovery procedures

### Provider-Specific Adaptations
Each infrastructure provider must adapt these guidelines to their specific cloud provider's context, taking into account:
- Provider-specific security features and limitations
- Available authentication and authorization mechanisms
- Provider-specific resource management capabilities
- Compliance requirements specific to the target infrastructure

## Best Practices

### Critical Design Principles
- **⚠️ Never rely solely on naming conventions for object identification**
- Use robust identification via tagging and labeling
- Implement standard Cluster API interfaces and contracts
- Design for flexibility, extensibility, and external constraints
- Prioritize security, testability, and robust identification

### Development Workflow
- Follow Cluster API logging standards
- Support multiple controller instances for scalability
- Adopt official Cluster API test framework
- Ensure comprehensive unit and E2E test coverage

## Agent System

This repository uses specialized Claude Code agents for different development tasks. Each agent is optimized for specific responsibilities with appropriate model selection.

### Available Agents

#### arch-design (claude-sonnet-4.5)
**Purpose**: Architecture and design decisions for Cluster API provider development

**Use for**:
- Architecture decisions and system design
- API design and CRD schema planning
- CAPI contract compliance validation
- Reconciliation pattern design
- Security architecture planning

**When to invoke**:
- "How should we architect..."
- "Design the API for..."
- "Does this comply with CAPI contracts?"
- Before implementing new features or controllers

#### implementation (claude-sonnet-4.5)
**Purpose**: Implementation of Cluster API provider features

**Use for**:
- Go code implementation (controllers, webhooks, API types)
- JIRA integration code
- Reconciliation logic implementation
- Following architectural designs

**When to invoke**:
- "Implement the controller for..."
- "Add webhook validation for..."
- "Write the JIRA client integration..."
- After architectural design is complete

#### quality-security (claude-sonnet-4.5)
**Purpose**: Testing, code quality, and security validation

**Use for**:
- Writing unit and E2E tests
- Security validation and auditing
- CAPI security guidelines compliance
- Code quality checks
- Test coverage analysis

**When to invoke**:
- "Write tests for..."
- "Validate security of..."
- "Review this implementation for compliance..."
- After implementation is complete

#### docs-ops (claude-haiku-3.5)
**Purpose**: Documentation, deployment configuration, and release management

**Use for**:
- User and developer documentation
- API reference documentation
- Deployment configuration (Kustomize, manifests)
- Release management and changelogs
- Operational guides

**When to invoke**:
- "Document this feature..."
- "Create installation guide for..."
- "Update deployment configuration..."
- "Prepare release notes for..."

### Agent Workflow

Typical development flow using agents:

1. **Design Phase**: Use `arch-design` to establish architecture and contracts
2. **Implementation Phase**: Use `implementation` to write code following design
3. **Validation Phase**: Use `quality-security` to test and validate security
4. **Documentation Phase**: Use `docs-ops` to document and prepare deployment

### Model Selection Rationale

- **Sonnet 4.5**: Complex tasks requiring deep reasoning (architecture, coding, security)
- **Haiku 3.5**: Well-defined tasks with large outputs (documentation) - cost-optimized

## Project Memory

### Session Instructions
- After updating project memory, always ensure CLAUDE.md is formatted
- Tests should not be run when only updating markdown files
- If uncertain about an answer, confirm guesses before proceeding

### Runtime Features
- Standard controller-runtime patterns with leader election
- Metrics server with TLS, authentication/authorization
- Health/readiness probes on port 8081
- Webhook server with certificate management
- HTTP/2 disabled for security (CVE mitigation)
- Multi-platform Docker support (arm64, amd64, s390x, ppc64le)
