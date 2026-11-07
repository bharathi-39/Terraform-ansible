# AWS Infrastructure Automation using Terraform & Ansible

## Project Overview

This project demonstrates Infrastructure as Code (IaC) and Configuration Management using Terraform, Ansible, AWS, and GitHub.

The objective is to automate the provisioning of AWS infrastructure and automate server configuration using Ansible.

---

# Architecture

Terraform
↓
AWS Infrastructure
↓
VPC
Subnet
Internet Gateway
Route Table
Security Group
EC2 Instance
↓
Ansible
↓
Nginx Installation
Website Deployment
↓
GitHub

---

# Best Practices Followed

- Infrastructure as Code (IaC)
- Reusable Terraform variables
- Version Control using Git & GitHub
- SSH Key-Based Authentication
- Security Groups for controlled access
- Separation of Infrastructure and Configuration Management
- Output Management using Terraform Outputs
- Configuration Automation using Ansible Playbooks

---

# Project Structure

```text
Terraform-Ansible-demo

├── terraform
│   ├── provider.tf
│   ├── versions.tf
│   ├── variables.tf
│   ├── terraform.tfvars
│   ├── main.tf
│   ├── outputs.tf
│
├── ansible
│   ├── inventory.ini
│   ├── install_nginx.yml
│   └── deploy_site.yml
│
├── website
│   └── index.html
│
└── README.md
```

---

# Step 1: Configure AWS

## Objective

Configure AWS CLI so Terraform can authenticate and create resources.

## Commands

```bash
aws configure
```

Provide:

```text
Access Key
Secret Key
Region = ap-south-1
Output = json
```

Verify:

```bash
aws sts get-caller-identity
```

---

# Step 2: Create Terraform Configuration

## Objective

Define Infrastructure as Code for AWS resources.

Resources Created:

- VPC
- Public Subnet
- Internet Gateway
- Route Table
- Route Table Association
- Security Group
- Key Pair
- EC2 Instance

---

# Step 3: Create Terraform Variables

## variables.tf

```hcl
variable "region" {}
variable "vpc_cidr" {}
variable "public_subnet_cidr" {}
variable "instance_type" {}
variable "ami_id" {}
variable "key_name" {}
```

---

# Step 4: Create Terraform Variable Values

## terraform.tfvars

```hcl
region             = "ap-south-1"
vpc_cidr           = "10.0.0.0/16"
public_subnet_cidr = "10.0.1.0/24"

instance_type      = "t2.micro"

ami_id             = "ami-xxxxxxxx"

key_name           = "terraform-key"
```

---

# Step 5: Create AWS Provider

## provider.tf

```hcl
provider "aws" {
  region = var.region
}
```

---

# Step 6: Create SSH Key Pair

## Objective

Generate SSH key pair for EC2 access.

```bash
ssh-keygen -t rsa -b 4096 -f terraform-key
```

Generated files:

```text
terraform-key
terraform-key.pub
```

---

# Step 7: Create AWS Key Pair Resource

## Terraform Configuration

```hcl
resource "aws_key_pair" "terraform_key" {

  key_name   = "terraform-key"

  public_key = file("terraform-key.pub")

}
```

---

# Step 8: Deploy Infrastructure

## Initialize Terraform

```bash
terraform init
```

## Validate Configuration

```bash
terraform validate
```

## Review Changes

```bash
terraform plan
```

## Create Infrastructure

```bash
terraform apply
```

Type:

```text
yes
```

Terraform creates:

- VPC
- Subnet
- Route Tables
- Security Group
- EC2 Instance

---

# Step 9: Verify EC2 Creation

Terraform Output:

```text
instance_id
instance_public_ip
```

Example:

```text
instance_id       = i-xxxxxxxxxxxx
instance_public_ip = 15.xxx.xxx.xxx
```

Verify in AWS Console:

```text
EC2
→ Instances
```

---

# Step 10: Connect to EC2

## Copy Key to Linux Home

```bash
mkdir -p ~/.ssh

cp terraform-key ~/.ssh/

chmod 400 ~/.ssh/terraform-key
```

## SSH Connection

```bash
ssh -i ~/.ssh/terraform-key ec2-user@PUBLIC-IP
```

Example:

```bash
ssh -i ~/.ssh/terraform-key ec2-user@15.xxx.xxx.xxx
```

---

# Step 11: Install Ansible

## Objective

Use Ansible for configuration management.

Install:

```bash
sudo dnf install ansible-core -y
```

Verify:

```bash
ansible --version
```

---

# Step 12: Create Inventory

## inventory.ini

```ini
[webserver]

localhost ansible_connection=local
```

---

# Step 13: Verify Ansible

```bash
ansible all -i inventory.ini -m ping
```

Expected:

```text
localhost | SUCCESS
```

---

# Step 14: Create Nginx Installation Playbook

## install_nginx.yml

```yaml
---
- hosts: webserver

  become: yes

  tasks:

    - name: Install nginx
      dnf:
        name: nginx
        state: present

    - name: Start nginx
      service:
        name: nginx
        state: started

    - name: Enable nginx
      service:
        name: nginx
        enabled: yes
```

Run:

```bash
ansible-playbook -i inventory.ini install_nginx.yml
```

---

# Step 15: Verify Nginx

Open:

```text
http://PUBLIC-IP
```

Expected:

```text
Welcome to nginx!
```

---

# Step 16: Create Sample Website

## index.html

```html
<!DOCTYPE html>
<html>

<head>
    <title>Terraform & Ansible Project</title>
</head>

<body>

<h1>Hi All!</h1>

<h2>AWS Infrastructure Automation Project</h2>

<p>Created by Bharathi Ganesh</p>

</body>

</html>
```

---

# Step 17: Deploy Website Using Ansible

## deploy_site.yml

```yaml
---
- hosts: webserver

  become: yes

  tasks:

    - name: Copy Website
      copy:
        src: ../website/index.html
        dest: /usr/share/nginx/html/index.html

    - name: Restart Nginx
      service:
        name: nginx
        state: restarted
```

Run:

```bash
ansible-playbook -i inventory.ini deploy_site.yml
```

---

# Step 18: Verify Deployment

Open:

```text
http://PUBLIC-IP
```

Expected:

```text
Hi All!

AWS Infrastructure Automation Project

Created by Bharathi Ganesh
```

---

# What Was Automated?

## Terraform Automation

- VPC Creation
- Public Subnet Creation
- Internet Gateway Creation
- Route Table Configuration
- Security Group Creation
- Key Pair Registration
- EC2 Creation

## Ansible Automation

- Package Installation
- Service Management
- Website Deployment
- Configuration Management

---

# Skills Demonstrated

- AWS
- EC2
- VPC
- IAM
- Terraform
- Infrastructure as Code (IaC)
- Ansible
- Linux
- SSH
- Git
- GitHub
- Nginx
- Automation

---

# Resume Description

### AWS Infrastructure Automation using Terraform & Ansible

- Automated AWS infrastructure provisioning using Terraform following Infrastructure as Code (IaC) principles.
- Created VPC, Public Subnet, Internet Gateway, Route Tables, Security Groups, AWS Key Pair, and EC2 Instances.
- Developed reusable Terraform configurations using variables and outputs.
- Configured Linux servers using Ansible playbooks.
- Automated Nginx installation and web application deployment.
- Managed source code through Git and GitHub.
- Implemented cloud security and automation best practices.
