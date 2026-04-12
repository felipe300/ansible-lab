# Laboratorio de Ansible + Docker Compose + LocalStack

[Go back to Readme](../README.md)

This project implements a local lab to learn and test **Ansible** by automating simulated AWS infrastructure through **LocalStack**, all orchestrated with **Docker Compose**.

The goal is to provide a 100% reproducible environment where you can practice playbooks, inventories, roles, and AWS modules without the need for a real account or incurring costs.

## Lab Objectives

- **AWS Simulation**: Run AWS services locally with LocalStack.
- **IaC (Infrastructure as Code)**: Automate the creation of resources (S3, EC2, IAM, VPC) using Ansible.
- DevOps Lifecycle\*\*: Implement a full workflow from local development to CI/CD with GitHub Actions.
- **Configuration Management**: Configure Linux servers (Docker containers) via SSH with Ansible.

## DevOps Lab: LocalStack + Docker + Ansible

Vamos a crear un laboratorio donde:

We are going to create a lab where:

- Docker Compose spins up:
  - LocalStack (simulated AWS)
  - 2 simulated Linux servers
- Ansible:
  - Creates fake AWS infrastructure in LocalStack (VPC, subnet, S3 bucket)
  - Configures Docker servers (nginx)
- GitHub Actions validates:
  - Project linting
  - Automatic infrastructure startup
  - LocalStack health checks
  - Continuous CI validation on every `push` / `pull request`

## Technologies Used

| Tools          | Purpose                      |
| -------------- | ---------------------------- |
| Ansible        | IaC Automation               |
| Docker         | Light Virtualization         |
| Docker Compose | Container Orchestration      |
| LocalStack     | AWS local Simulation         |
| Git / GitHub   | Control Version y Repository |
| GitHub Actions | Automated CI/CD              |

## Project Architecture

```bash
ansible-lab/
├── .github/
│   └── workflows/
│       └── ansible-ci.yml
├── docker/
│   ├── Dockerfile
│   └── docker-compose.yml
├── ansible/
│   ├── inventory/
│   │   └── hosts.ini
│   ├── playbooks/
│   │   ├── provision-aws.yml
│   │   └── configure-servers.yml
│   ├── ansible.cfg
│   └── requirements.yml
└── README.md
```

## Prerequisites

Before starting, make sure you have the following installed:

- Git
- Docker
- Docker Compose
- Ansible (including the boto3 library for AWS modules)
- Python 3.9+
- AWS CLI (optional, but recommended)

## DevOps Workflow

- **Environment** Setup: Start LocalStack and the target containers.
- **Provisioning**: Run playbooks to create S3 buckets, queues, or simulated instances.
- **Configuration**: Apply Ansible roles to the Docker nodes (e.g., installing Nginx).
- **Validation**: Commit changes to Git and trigger CI in GitHub Actions.

## Repository Installation

**Clone the repository**

```bash
git clone https://github.com/felipe300/ansible-lab.git
cd ansible-lab
```

**AWS Dummy Variables**

Create a `.env` file or ensure the variables are available in the `docker`/ folder. LocalStack will use "dummy" credentials:

```bash
# NOTE: This is a basic "EXAMPLE" of your credentials, change them and use new ones

# LocalStack Config
LOCALSTACK_DEBUG=1
AWS_REGION=us-east-1

# Server Config
SERVER_USER=admin
SSH_PORT_1=2221
SSH_PORT_2=2222


# Credentials (Fake for LocalStack)
AWS_ACCESS_KEY_ID=test
AWS_SECRET_ACCESS_KEY=test
```

**Start LocalStack**

The `localstack:latest` version in the `docker-compose` file will use the LocalStack PRO version. Versions `2.3` or `3.0` will be used to simulate services such as `s3`, `sqs`, `dynamodb`, or `sns`.

```bash
cd docker
docker compose up -d
```

**Verify container**

```bash
curl -L http://localhost:4566/_localstack/health | jq | grep "available"
```

First, install the necessary dependencies:

**Run Playbook**

```bash
ansible-playbook -i ansible/inventory.ini ansible/playbook.yml
```

Then, execute the deployment:

```bash
ansible-playbook -i ansible/inventory/hosts.ini ansible/playbooks/provision-aws.yml

ansible-playbook -i ansible/inventory/hosts.ini ansible/playbooks/configure-servers.yml
```

## GitHub Actions: How to Test CI

The pipeline runs automatically on:

- `push`
- `pull_request`

**To trigger the CI**:

Make changes to your project, then push them to GitHub.

## Autor

**Felipe Gutierrez**

_Educational laboratory focused on automation and DevOps culture_.
