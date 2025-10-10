# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## OpenTelemetry Demo Overview

This is the OpenTelemetry Astronomy Shop demo - a comprehensive microservices application demonstrating OpenTelemetry instrumentation across multiple programming languages and frameworks. The demo consists of 20+ microservices implementing an e-commerce platform.

## Architecture

### Microservices Overview
The demo implements a complete e-commerce system with the following core services:

- **Frontend** (Next.js/TypeScript) - Main web UI
- **Product Catalog** (Go) - Product management service
- **Cart** (Redis) - Shopping cart functionality
- **Checkout** (Go) - Order processing service
- **Payment** (Node.js/TypeScript) - Payment processing
- **Shipping** (Rust) - Shipping calculations
- **Currency** (Node.js/TypeScript) - Currency conversion
- **Email** (Node.js/TypeScript) - Email notifications
- **Recommendation** (Python) - Product recommendations
- **Ad Service** (Java) - Advertisement service
- **Accounting** (C#/.NET) - Order accounting
- **Fraud Detection** (Kotlin) - Payment fraud detection
- **Quote** (PHP) - Quote generation

### Infrastructure Services
- **Load Generator** (Python/Locust) - Synthetic traffic generation
- **Feature Flags** (flagd) - Feature flag management
- **Frontend Proxy** (Envoy) - Ingress proxy

### Observability Stack
- **OpenTelemetry Collector** - Telemetry data collection and export
- **Jaeger** - Distributed tracing backend
- **Grafana** - Metrics and dashboards
- **Prometheus** - Metrics collection (minimal mode)

## Development Commands

### Starting the Demo
```bash
# Start full demo with all services
make start

# Start minimal version (without Prometheus/OpenSearch)
make start-minimal

# Stop all services
make stop
```

### Building and Testing
```bash
# Build all Docker images
make build

# Build and push images to registry
make build-and-push

# Run integration tests
make run-tests

# Run specific trace-based tests
make run-tracetesting

# Run documentation and code quality checks
make check

# Apply automated fixes
make fix
```

### Development Workflow
```bash
# Restart a specific service (rebuild not included)
make restart service=frontend

# Rebuild and restart a specific service
make redeploy service=frontend

# Clean Docker images
make clean-images

# Generate protocol buffers
make generate-protobuf

# Generate Kubernetes manifests
make generate-kubernetes-manifests
```

### Quality Assurance
```bash
# Spell check all markdown files
make misspell

# Lint markdown files
make markdownlint

# Validate YAML files
make yamllint

# Check license headers
make checklicense

# Check external links
make checklinks
```

## Key Configuration Files

- **docker-compose.yml** - Main service definitions
- **docker-compose.minimal.yml** - Minimal deployment without Prometheus/OpenSearch
- **docker-compose-tests.yml** - Test environment configuration
- **.env** - Environment variables and image versions
- **.env.override** - Local environment overrides (create this file for customizations)

## Service Development

### Adding a New Service
1. Create service directory under `src/`
2. Implement service with appropriate OpenTelemetry instrumentation
3. Add Dockerfile for containerization
4. Update docker-compose.yml with service definition
5. Add any necessary dependencies to other services
6. Update documentation and tests

### OpenTelemetry Instrumentation
Each service demonstrates OpenTelemetry instrumentation for its respective language:
- Automatic instrumentation where available
- Manual instrumentation for custom spans and metrics
- Proper context propagation across service boundaries
- Resource attributes and semantic conventions

## Test Structure

- **Frontend Tests** - Cypress e2e tests in `src/frontend/cypress/`
- **Trace-based Tests** - Tracetest scenarios in `test/tracetesting/`
- **Integration Tests** - Full system tests via `make run-tests`

## Multi-platform Support

For building multi-platform images (linux/amd64, linux/arm64):
```bash
# Create multi-platform builder
make create-multiplatform-builder

# Build multi-platform images
make build-multiplatform

# Build and push multi-platform images
make build-multiplatform-and-push

# Remove multi-platform builder
make remove-multiplatform-builder
```

## Ports and URLs

When running locally:
- **Demo UI**: http://localhost:8080
- **Jaeger UI**: http://localhost:8080/jaeger/ui
- **Grafana**: http://localhost:8080/grafana
- **Feature Flags UI**: http://localhost:8080/feature
- **Load Generator UI**: http://localhost:8080/loadgen

## Language-Specific Notes

- **Go services** use modules and standard OpenTelemetry Go SDK
- **Node.js services** use npm and @opentelemetry/* packages
- **Python services** use pip requirements.txt and opentelemetry-* packages
- **Java services** use Gradle and OpenTelemetry Java agent
- **C# services** use .NET and OpenTelemetry .NET SDK
- **Rust services** use Cargo and opentelemetry crate
- **PHP services** use Composer and OpenTelemetry PHP packages

## Contributing

- Follow the existing code patterns and OpenTelemetry instrumentation approaches
- Ensure changes work with both full and minimal deployment modes
- Update relevant documentation and tests
- All services should include proper observability (traces, metrics, logs)
- Maintain Docker multi-platform compatibility