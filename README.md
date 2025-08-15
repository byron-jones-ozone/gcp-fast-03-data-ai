# OZPR Data AI Dev - Terraform

This directory contains the Terraform configuration for the OZPR Data AI development project.

## Prerequisites

1. Complete FAST stages: 0-bootstrap, 1-resman, 2-networking, 2-security
2. Have appropriate GCP permissions for project creation
3. Terraform installed locally

## Setup

1. Copy the sample tfvars file:
   ```bash
   cp terraform.tfvars.sample terraform.tfvars
   ```

2. Update `terraform.tfvars` with your actual values

3. Initialize Terraform:
   ```bash
   terraform init
   ```

4. Plan the deployment:
   ```bash
   terraform plan
   ```

5. Apply the configuration:
   ```bash
   terraform apply
   ```

## Project Configuration

The project configuration is defined in `data/hierarchy/data-engineering/dev/ozpr-data-ai-dev.yaml` and includes:

- BigQuery APIs enabled
- Data engineering team IAM bindings
- Service accounts for dbt and Composer
- Shared VPC configuration

## CI/CD Pipeline

This project includes a GitHub Actions workflow that:
- Triggers on pull requests to main branch
- Runs Terraform init, validate, and plan
- Applies changes when PRs are merged
- Posts detailed comments on PRs with plan output