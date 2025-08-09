# Pulse — Micro-Survey MVP

Pulse is a lightweight, cloud-native **micro-survey application** built to demonstrate modern **DevOps** and **Cloud Engineering** practices using the **AWS Free Tier**.  
It’s designed as a **minimum viable product (MVP)** that balances real-world functionality with a cost-efficient architecture.

---

## 📌 Features

### For Users
- Submit short surveys (3–8 questions).
- No login required.
- Form validation before submission.

### For Admins
- Secure login via credentials stored in **AWS SSM Parameter Store**.
- View aggregated survey results.
- Export survey data to CSV, stored in **Amazon S3** with a time-limited pre-signed URL.

---

## 🛠️ Tech Stack

**Frontend**  
- Minimal HTML/CSS/JS served via **Nginx**.

**Backend**  
- **FastAPI** (Python) or Flask for APIs.
- **DynamoDB** for storing survey responses.
- **S3** for CSV exports and static assets.

**Infrastructure & DevOps**  
- **Terraform** — Infrastructure as Code (IaC).
- **Ansible** — Configuration management & app deployment.
- **Docker** + **docker-compose** — Containerized app + Nginx.
- **GitHub Actions** — CI/CD pipeline with:
  - Code linting & unit tests
  - Security scan with **Trivy**
  - SBOM generation
  - Build & push to **Amazon ECR**
  - Deployment via **AWS SSM SendCommand**
- **CloudWatch** — Centralized logs & alerts.
- **AWS Budgets** — Cost monitoring.

---

## 🏗️ Architecture

![Architecture Diagram](docs/architecture-diagram.png)

---

## 🚀 Deployment Workflow

1. **Developer** pushes code to GitHub.
2. **GitHub Actions** runs lint/tests, scans with Trivy, builds Docker image, and pushes to **ECR**.
3. **AWS SSM** triggers deployment on **EC2** via docker-compose.
4. **DynamoDB** stores form responses.
5. **S3** stores CSV exports & static files.
6. **CloudWatch** monitors logs & sends alerts.

---

## 🔐 Security Best Practices Implemented
- No hardcoded credentials — secrets stored in **SSM Parameter Store**.
- Least-privilege **IAM roles** for app, CI/CD, and infrastructure.
- No public SSH — management only via **SSM Session Manager**.
- Docker containers run as non-root.
- CI/CD fails on high-severity vulnerabilities.

---

## 📦 Getting Started

### Prerequisites
- AWS CLI configured with Free Tier account.
- Terraform ≥ 1.5
- Ansible ≥ 2.14
- Docker + docker-compose
- Python ≥ 3.10

### Quick Setup
```bash
# Clone repository
git clone https://github.com/<your-username>/pulse-mvp.git
cd pulse-mvp

# Provision AWS infrastructure
cd infra
terraform init
terraform apply

# Configure EC2 & deploy app
cd ../ansible
ansible-playbook -i inventory/aws_ec2.yml playbooks/base.yml
ansible-playbook -i inventory/aws_ec2.yml playbooks/app.yml
