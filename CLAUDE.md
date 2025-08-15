# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is the OZPR Data AI Dev Terraform configuration, part of the Google Cloud FAST (Fabric Accelerated Start Toolkit) framework. It creates and manages GCP data engineering infrastructure using the Cloud Foundation Fabric project factory pattern.

## Development Commands

### Initial Setup with FAST Framework
First, link the required FAST stage outputs:
```bash
# Using local fast-config directory
../../../stage-links.sh ~/fast-config

# Copy and paste the generated ln commands:
ln -s ~/fast-config/tfvars/0-globals.auto.tfvars.json ./
ln -s ~/fast-config/tfvars/0-bootstrap.auto.tfvars.json ./
ln -s ~/fast-config/tfvars/1-resman.auto.tfvars.json ./
ln -s ~/fast-config/tfvars/2-networking.auto.tfvars.json ./
ln -s ~/fast-config/tfvars/2-security.auto.tfvars.json ./
```

Alternative using GCS bucket:
```bash
../../../stage-links.sh gs://ozpr-prod-iac-core-outputs-0

# Copy and paste the generated gcloud commands:
gcloud alpha storage cp gs://ozpr-prod-iac-core-outputs-0/tfvars/0-globals.auto.tfvars.json ./
gcloud alpha storage cp gs://ozpr-prod-iac-core-outputs-0/tfvars/0-bootstrap.auto.tfvars.json ./
gcloud alpha storage cp gs://ozpr-prod-iac-core-outputs-0/tfvars/1-resman.auto.tfvars.json ./
gcloud alpha storage cp gs://ozpr-prod-iac-core-outputs-0/tfvars/2-networking.auto.tfvars.json ./
gcloud alpha storage cp gs://ozpr-prod-iac-core-outputs-0/tfvars/2-security.auto.tfvars.json ./
```

### Terraform Workflow
```bash
# Initialize Terraform (required first time)
terraform init

# Plan infrastructure changes
terraform plan

# Apply infrastructure changes
terraform apply

# Destroy infrastructure (use with caution)
terraform destroy

# Format Terraform files
terraform fmt

# Validate configuration
terraform validate
```

### Legacy Setup Process (if not using FAST)
```bash
# Copy sample variables file
cp terraform.tfvars.sample terraform.tfvars
# Edit terraform.tfvars with actual values before running terraform commands
```

## Architecture

### Project Factory Pattern
- Uses Google Cloud Foundation Fabric project factory module (v32.0.0)
- Configuration driven by YAML files in `data/hierarchy/` 
- Creates projects with standardized labeling, services, and IAM

### Key Components
- **Main Module**: `main.tf` - Project factory configuration
- **Variables**: `variables.tf` - Billing account and prefix configuration
- **Outputs**: `outputs.tf` - Project and service account information
- **Providers**: `providers.tf` - GCS backend and Google provider configuration

### Project Configuration
Project settings defined in `data/hierarchy/data-engineering/dev/ozpr-data-ai-dev.yaml`:
- Enables BigQuery, Container, Storage, and Composer APIs
- Creates service accounts for dbt-cloud-data-ai and composer
- Configures shared VPC with host project `ozpr-dev-net-spoke-0`
- Sets up network permissions for data engineering team

### Prerequisites
Requires completion of FAST stages: 0-bootstrap, 1-resman, 2-networking, 2-security

### State Management
- Backend: GCS bucket `ozpr-prod-teams-dataeng-0`
- State prefix: `ozpr-data-ai-dev/state`
- Service account impersonation for authentication

## File Structure
```
├── main.tf                 # Project factory module configuration
├── variables.tf            # Input variables (billing account, prefix)
├── outputs.tf              # Output values (projects, service accounts)
├── providers.tf            # Provider and backend configuration
├── terraform.tfvars.sample # Sample variables file
└── data/hierarchy/         # Project configuration YAML files
    └── data-engineering/dev/
        └── ozpr-data-ai-dev.yaml  # Project configuration
```