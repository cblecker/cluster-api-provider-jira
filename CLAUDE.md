# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Kubernetes Cluster API provider that uses JIRA/humans as the infrastructure provider. It's built with Kubebuilder and follows standard controller-runtime patterns.

**Important Note**: According to the README, this is not intended for production use ("Seriously don't use this").

## Development Commands

### Building and Testing
- `make build` - Build the manager binary
- `make test` - Run unit tests (includes manifests, generate, fmt, vet, and setup-envtest)
- `make test-e2e` - Run end-to-end tests using Kind cluster
- `make run` - Run the controller locally from your host

### Code Quality
- `make fmt` - Format Go code
- `make vet` - Run go vet
- `make lint` - Run golangci-lint linter
- `make lint-fix` - Run golangci-lint with automatic fixes
- `make lint-config` - Verify golangci-lint configuration
- `make generate` - Generate DeepCopy methods and other code
- `make manifests` - Generate RBAC, CRDs, and webhook configurations

### Docker Operations
- `make docker-build IMG=<registry>/cluster-api-provider-jira:tag` - Build Docker image
- `make docker-push IMG=<registry>/cluster-api-provider-jira:tag` - Push Docker image
- `make docker-buildx` - Build multi-platform images

### Deployment
- `make install` - Install CRDs into cluster
- `make deploy IMG=<registry>/cluster-api-provider-jira:tag` - Deploy controller to cluster
- `make undeploy` - Remove controller from cluster
- `make uninstall` - Remove CRDs from cluster

### Testing Infrastructure
- `make setup-test-e2e` - Set up Kind cluster for e2e tests
- `make cleanup-test-e2e` - Clean up Kind cluster after tests

### Utility Commands
- `make help` - Display all available Make targets with descriptions
- `make build-installer` - Generate consolidated YAML with CRDs and deployment
- `make all` - Default target (runs build)

## Technical Architecture

### Core Components
- **cmd/main.go**: Controller manager entry point with standard Kubebuilder setup
- **test/**: Test suites including e2e tests and utilities

### Configuration Structure
- **config/default/**: Main kustomization, patches, and metrics service
- **config/rbac/**: RBAC roles, bindings, service accounts, and leader election
- **config/manager/**: Controller deployment configuration
- **config/prometheus/**: Metrics monitoring setup with TLS patches
- **config/network-policy/**: Network policies for metrics traffic
- **config/crd/**: Custom Resource Definitions (auto-generated)

### Runtime Features
- Controller manager follows standard controller-runtime patterns
- Metrics server with optional TLS and authentication/authorization
- Health (`/healthz`) and readiness (`/readyz`) probes on port 8081
- Leader election support with configurable release behavior
- Webhook server with certificate management
- HTTP/2 disabled by default for security (CVE mitigation)
- Configurable TLS certificate paths for both metrics and webhooks

## Development Environment

### Prerequisites
- Go v1.24.0+
- Docker v17.03+
- kubectl v1.11.3+
- Access to Kubernetes v1.11.3+ cluster
- Kind (for e2e testing)

### Dependencies and Versions
- **Go**: 1.24.5
- **Controller Runtime**: v0.22.1
- **Kubernetes API**: v0.34.0
- **Testing Framework**: Ginkgo v2.22.0 and Gomega v1.36.1
- **Kustomize**: v5.7.1
- **Controller Tools**: v0.19.0
- **Golangci-lint**: v2.4.0
- **EnvTest**: Auto-detected from controller-runtime version
- **Kubernetes for testing**: Auto-detected from k8s.io/api version

### Testing Strategy
- Unit tests use envtest for Kubernetes API simulation
- E2E tests run on Kind clusters
- Ginkgo/Gomega testing framework

### Security and Configuration
- Uses Kustomize v5.7.1 for manifest generation
- Supports both HTTP and HTTPS metrics endpoints (HTTPS by default)
- Certificate management for webhooks and metrics with auto-generation fallback
- RBAC-protected metrics endpoint with authentication and authorization
- Network policies for secure metrics traffic
- Multi-platform Docker image support (linux/arm64, linux/amd64, linux/s390x, linux/ppc64le)

## Project Status

This project appears to be minimal/skeletal with very few actual controller implementations, suggesting it may be a template or proof-of-concept rather than a fully functional provider. The scaffolding includes comprehensive metrics, RBAC, and security configurations but lacks actual JIRA provider logic.